# **Karate + Spring Boot Integration**
## **Using Spring-Managed Beans in Karate for API and Database Testing**

### **Overview**
Karate is a powerful API testing framework that supports seamless integration with **Spring Boot**. By leveraging **Spring-managed beans** in Karate tests, we can:
- Access **services, repositories, and configurations** directly in feature files.
- Use **Spring’s dependency injection** to manage test dependencies efficiently.
- Perform **database validations, Kafka event processing, and external service calls** with minimal setup.

This document covers:
1. **Setting up Karate with Spring Boot**
2. **Accessing Spring Beans inside Karate feature files**
3. **Using Spring Beans for API Testing**
4. **Using Spring Beans for Database Testing**
5. **Running Scenarios in Parallel Using JUnit5**
6. **Best Practices for Integration**
7. **Dynamic Scenario-Level Parallel Execution**
8. **Using JUnit5 `@ParameterizedTest` for Parallel Execution**
9. **Thread Safety and Data Sharing Between Scenarios**

---

## **1️⃣ Setting Up Karate with Spring Boot**
To access **Spring Beans** inside Karate tests, we need to:
1. **Annotate our test class with `@SpringBootTest`**.
2. **Expose the Spring `ApplicationContext` globally**.
3. **Enable Spring Boot context before executing tests**.

### **1.1 Add Dependencies**
Add the following dependencies in `pom.xml`:

```xml
<dependencies>
    <!-- Karate Testing Framework -->
    <dependency>
        <groupId>com.intuit.karate</groupId>
        <artifactId>karate-junit5</artifactId>
        <version>1.4.0</version>
        <scope>test</scope>
    </dependency>

    <!-- Spring Boot Starter for Testing -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>
```

### **1.2 Create a Karate Test Runner**
📂 `KarateSpringRunner.java`
```java
package com.example.karate;

import com.intuit.karate.junit5.Karate;
import org.junit.jupiter.api.BeforeAll;
import org.springframework.boot.test.context.SpringBootTest;

@SpringBootTest
public class KarateSpringRunner {

    @BeforeAll
    static void setup() {
        System.setProperty("karate.env", "dev"); // Set default environment
    }

    @Karate.Test
    Karate runAllTests() {
        return Karate.run("classpath:features").relativeTo(getClass());
    }
}
```

---

## **2️⃣ Accessing Spring Beans in Karate**
### **2.1 Create `ApplicationContextProvider`**
📂 `ApplicationContextProvider.java`
```java
package com.example.karate.config;

import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;
import org.springframework.stereotype.Component;

@Component
public class ApplicationContextProvider implements ApplicationContextAware {

    private static ApplicationContext applicationContext;

    @Override
    public void setApplicationContext(ApplicationContext context) {
        applicationContext = context;
    }

    public static ApplicationContext getApplicationContext() {
        return applicationContext;
    }
}
```

### **2.2 Modify `karate-config.js` to Fetch Beans**
```javascript
function fn() {
    var config = {};

    var applicationContext = Java.type('com.example.karate.config.ApplicationContextProvider')
                               .getApplicationContext();

    var propertyCacheService = applicationContext.getBean('propertyCacheService');
    var properties = propertyCacheService.getAllProperties();
    config.baseUrl = properties['base-url'];
    config.authToken = properties['auth-token'];

    karate.log('Base URL:', config.baseUrl);
    karate.log('Auth Token:', config.authToken);

    return config;
}
```

---

## **3️⃣ Using Spring Beans for API Testing**
### **3.1 Create a Spring Service**
📂 `UserService.java`
```java
package com.example.karate.service;

import org.springframework.stereotype.Service;

@Service
public class UserService {
    public String getUserById(String id) {
        return "User-" + id;
    }
}
```

### **3.2 Use `UserService` in Karate**
📂 `user.feature`
```gherkin
Feature: User API Test with Spring Bean

  Background:
    * def applicationContext = Java.type('com.example.karate.config.ApplicationContextProvider').getApplicationContext()
    * def userService = applicationContext.getBean('userService')

  Scenario: Get User By ID
    * def response = userService.getUserById('101')
    * print 'User Response:', response
    * match response == 'User-101'
```

---

## **4️⃣ Using Spring Beans for Database Testing**
### **4.1 Create a Spring Repository**
📂 `UserRepository.java`
```java
package com.example.karate.repository;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.stereotype.Repository;
import com.example.karate.model.User;

@Repository
public interface UserRepository extends JpaRepository<User, Long> {
    User findByUsername(String username);
}
```
📂 `UserService.java`
```java
package com.example.karate.service;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import com.example.karate.repository.UserRepository;
import com.example.karate.model.User;

@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public User getUserByUsername(String username) {
        return userRepository.findByUsername(username);
    }
}
```

### **4.2 Use Database Beans in Karate**
📂 `db.feature`
```gherkin
Feature: Database Test with Spring Bean

  Background:
    * def applicationContext = Java.type('com.example.karate.config.ApplicationContextProvider').getApplicationContext()
    * def userService = applicationContext.getBean('userService')

  Scenario: Fetch User from Database
    * def user = userService.getUserByUsername('Leela')
    * print 'User:', user
    * match user.email == 'leela@example.com'
```

---

## **5️⃣ Running Scenarios in Parallel Using JUnit5**
📂 `KarateParallelRunner.java`
```java
package com.example.karate.runner;

import com.intuit.karate.junit5.Karate;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.MethodSource;
import java.io.IOException;
import java.util.List;
import java.util.stream.Stream;

public class KarateParallelRunner {

    static Stream<String> featureFiles() throws IOException {
        return FeatureFileScanner.getAllFeatureFiles().stream();
    }

    @ParameterizedTest
    @MethodSource("featureFiles")
    void runFeatureInParallel(String featurePath) {
        Karate.run(featurePath).relativeTo(getClass());
    }
}
```

---

## **6️⃣ Best Practices**
| ✅ Best Practice | 🚀 Benefit |
|----------------|-----------|
| Use `ApplicationContextProvider` | Provides **global access** to Spring Beans. |
| Fetch beans in `karate-config.js` | Keeps **Karate feature files clean** and **modular**. |
| Use `karate.callSingle()` for setup | Prevents **unnecessary API calls** in tests. |
| Wrap DB operations in transactions | Ensures **data isolation** in tests. |

---

## **7️⃣ Conclusion**
By integrating **Spring-managed beans with Karate**, we can:
✅ **Use Spring Services & Repositories inside feature files**  
✅ **Leverage dependency injection for better test maintainability**  
✅ **Run API & Database tests efficiently in an automated pipeline**  

🚀 **Now your Karate tests are fully integrated with Spring Boot!** 🚀  

