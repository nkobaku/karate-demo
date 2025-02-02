# **Karate + Spring Boot Integration**
## **Using Spring-Managed Beans in Karate for API and Database Testing**

### **Overview**
Karate is a powerful API testing framework that supports seamless integration with **Spring Boot**. By leveraging **Spring-managed beans** in Karate tests, we can:
- Access **services, repositories, and configurations** directly in feature files.
- Use **Spring’s dependency injection** to manage test dependencies efficiently.
- Perform **database validations, Kafka event processing, and external service calls** with minimal setup.
- Optimize **parallel execution** and **thread-safe data sharing**.

This document covers:
1. **Setting up Karate with Spring Boot**
2. **Using Spring Beans in Karate feature files**
3. **Implementing a Property Cache in Karate**
4. **Using Beans for API and Database Testing**
5. **Running Tests in Parallel Using JUnit5**
6. **Ensuring Thread-Safe Data Sharing in Parallel Execution**
7. **Dynamic Scenario-Level Parallel Execution**
8. **Using JUnit5 `@ParameterizedTest` for Parallel Execution**
9. **Best Practices for Integration**

---

## **1️⃣ Setting Up Karate with Spring Boot**
To access **Spring Beans** inside Karate tests, we need to:
1. **Annotate our test class with `@SpringBootTest`**.
2. **Expose the Spring `ApplicationContext` globally**.
3. **Enable Spring Boot context before executing tests**.

### **1.1 Add Dependencies**
```xml
<dependencies>
    <dependency>
        <groupId>com.intuit.karate</groupId>
        <artifactId>karate-junit5</artifactId>
        <version>1.4.0</version>
        <scope>test</scope>
    </dependency>
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
        System.setProperty("karate.env", "dev");
    }

    @Karate.Test
    Karate runAllTests() {
        return Karate.run("classpath:features").relativeTo(getClass());
    }
}
```

---

## **2️⃣ Accessing Spring Beans in Karate**
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

📂 `karate-config.js`
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

## **3️⃣ Implementing a Property Cache in Karate**
📂 `PropertyCacheService.java`
```java
package com.example.karate.config;

import org.springframework.core.env.Environment;
import org.springframework.stereotype.Service;
import javax.annotation.PostConstruct;
import java.util.Collections;
import java.util.HashMap;
import java.util.Map;

@Service
public class PropertyCacheService {

    private final Environment environment;
    private final Map<String, String> propertyCache = new HashMap<>();
    private boolean propertiesLoaded = false;

    public PropertyCacheService(Environment environment) {
        this.environment = environment;
    }

    @PostConstruct
    public void initializePropertyCache() {
        if (!propertiesLoaded) {
            for (String key : environment.getPropertySources().iterator().next().getPropertyNames()) {
                propertyCache.put(key, environment.getProperty(key));
            }
            propertiesLoaded = true;
        }
    }

    public String getProperty(String key) {
        return propertyCache.get(key);
    }

    public Map<String, String> getAllProperties() {
        return Collections.unmodifiableMap(propertyCache);
    }
}
```

---

## **4️⃣ Running Tests in Parallel Using JUnit5**
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

## **5️⃣ Ensuring Thread-Safe Data Sharing in Parallel Execution**
- Use **scenario-specific variables** instead of modifying global variables.
- Use `karate.callSingle()` for test setup.
- Ensure **each test has unique data** (e.g., randomized IDs).

📂 `random-data.feature`
```gherkin
Feature: Generate Unique Data

Scenario: Create Unique User
  * def userId = karate.randomUUID()
  * print "Generated User ID:", userId
```

---

## **6️⃣ Dynamic Scenario-Level Parallel Execution**
To execute each **scenario in parallel**, extract scenarios into individual feature files dynamically.

📂 `ScenarioExtractor.java`
```java
package com.example.karate.util;

import java.io.IOException;
import java.nio.file.*;
import java.util.List;
import java.util.stream.Collectors;

public class ScenarioExtractor {

    private static final String FEATURE_DIRECTORY = "src/test/resources/features";

    public static List<String> extractScenariosAsFeatureFiles() throws IOException {
        return Files.walk(Paths.get(FEATURE_DIRECTORY))
                .filter(Files::isRegularFile)
                .filter(path -> path.toString().endsWith(".feature"))
                .map(Path::toString)
                .collect(Collectors.toList());
    }
}
```

---

## **7️⃣ Best Practices**
| ✅ Best Practice | 🚀 Benefit |
|----------------|-----------|
| Use `ApplicationContextProvider` | Provides **global access** to Spring Beans. |
| Fetch beans in `karate-config.js` | Keeps **Karate feature files clean** and **modular**. |
| Use `karate.callSingle()` for setup | Prevents **unnecessary API calls** in tests. |
| Wrap DB operations in transactions | Ensures **data isolation** in tests. |

---

## **8️⃣ Conclusion**
By integrating **Spring-managed beans with Karate**, we can:
✅ **Use Spring Services & Repositories inside feature files**  
✅ **Leverage dependency injection for better test maintainability**  
✅ **Run API & Database tests efficiently in an automated pipeline**  
✅ **Execute test scenarios in parallel while maintaining thread safety**  

🚀 **Now your Karate tests are fully integrated with Spring Boot!** 🚀  

