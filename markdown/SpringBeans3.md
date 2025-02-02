# **Complete Chat History - Karate + Spring Boot Integration**
## **Using Spring-Managed Beans in Karate for API and Database Testing**

### **Overview**
This document contains the **entire chat history** regarding **Karate and Spring Boot integration**. It includes:
- **All questions asked by the user**
- **All responses, explanations, and clarifications**
- **All code snippets and configurations shared**
- **Discussions on property cache, parallel execution, thread safety, and JUnit5 integration**

---

## **1️⃣ Initial Query: Using Spring Beans in Karate**
### **User's Question:**
> "@SpringBootTest, Karate, how to use Spring beans in Karate feature file?"

### **Response:**
To access **Spring Beans** inside Karate, we need to:
1. Annotate our test class with `@SpringBootTest`.
2. Expose the **Spring ApplicationContext** globally.
3. Fetch **Spring-managed beans** inside `karate-config.js`.

---

## **2️⃣ Setting Up Karate with Spring Boot**

### **User's Follow-Up:**
> "Can you provide examples?"

### **Response:**
📂 **Karate Test Runner - `KarateSpringRunner.java`**
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

📂 **ApplicationContext Provider - `ApplicationContextProvider.java`**
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

📂 **Modifying `karate-config.js` to Fetch Beans**
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
### **User's Question:**
> "Can you build a property cache that reads environment-specific properties only once?"

### **Response:**
📂 **Property Cache Service - `PropertyCacheService.java`**
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
### **User's Question:**
> "How do I run feature files in parallel using JUnit5?"

### **Response:**
📂 **JUnit5 Parallel Execution - `KarateParallelRunner.java`**
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
### **User's Question:**
> "How do I ensure data safety when running tests in parallel?"

### **Response:**
1. **Use unique test data** for each scenario to avoid conflicts.
2. **Do not modify global variables** in a shared way.
3. **Use database transactions** for cleanup.

📂 **Generating Unique Data for Parallel Execution**
```gherkin
Feature: Generate Unique Data

Scenario: Create Unique User
  * def userId = karate.randomUUID()
  * print "Generated User ID:", userId
```

---

## **6️⃣ Dynamic Scenario-Level Parallel Execution**
### **User's Question:**
> "Can I run scenarios in parallel instead of feature files?"

### **Response:**
📂 **Extracting Individual Scenarios - `ScenarioExtractor.java`**
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

