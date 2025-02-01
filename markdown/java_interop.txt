# Java Interop in Karate

## Overview
Karate provides **Java Interop** (Java interoperability), allowing you to leverage Java code within your Karate tests. This is useful for scenarios where complex logic, external dependencies, or custom utilities are required.

---

## 1. Calling Java Code in Karate
Karate allows calling Java classes and methods directly from your feature files.

### **Example of Calling a Static Java Method**
You can create a Java utility class and invoke its methods from a feature file.

#### **Java Utility Class (`StringUtils.java`)**
```java
package utils;

public class StringUtils {
    public static String toUpperCase(String input) {
        return input.toUpperCase();
    }
}
```

#### **Using the Java Class in Karate**
```gherkin
Feature: Java Interop Example

Scenario: Convert String to Uppercase
  * def StringUtils = Java.type('utils.StringUtils')
  * def result = StringUtils.toUpperCase('karate')
  * print result
  * match result == 'KARATE'
```

**Key Points:**
- `Java.type('utils.StringUtils')` loads the Java class.
- The static method `toUpperCase` is called directly.
- The result is printed and validated.

---

## 2. Using Non-Static Java Methods
If a Java class has non-static methods, you must instantiate it.

### **Example of an Instance Method**
#### **Java Class (`Calculator.java`)**
```java
package utils;

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}
```

#### **Using in Karate**
```gherkin
Feature: Java Object Example

Scenario: Add Two Numbers
  * def Calculator = Java.type('utils.Calculator')
  * def calculator = new Calculator()
  * def sum = calculator.add(5, 3)
  * print sum
  * match sum == 8
```

**Key Points:**
- `new Calculator()` creates an instance of the Java class.
- The method `add(5,3)` is called on the instance.

---

## 3. Using Java Methods in `karate-config.js`
You can also use Java methods in the `karate-config.js` file.

#### **Example**
```javascript
function fn() {
  var config = {};
  var StringUtils = Java.type('utils.StringUtils');
  config.upperName = StringUtils.toUpperCase('karate');
  return config;
}
```
This will make `upperName` available across all feature files.

---

## 4. Using Java Objects in JSON Responses
Java objects can be used as test data.

### **Example**
#### **Java Class (`User.java`)**
```java
package utils;

public class User {
    public String name;
    public int age;

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

#### **Karate Feature**
```gherkin
Feature: Java Object to JSON

Scenario: Convert Java Object to JSON
  * def User = Java.type('utils.User')
  * def user = new User('John Doe', 30)
  * print user
  * match user.name == 'John Doe'
  * match user.age == 30
```
Karate automatically converts Java objects to JSON.

---

## 5. Running Java Methods as a Background Task
You can execute Java methods in the background while running Karate tests.

### **Example: Generating Random Data in Java**
#### **Java Class (`RandomUtils.java`)**
```java
package utils;

import java.util.UUID;

public class RandomUtils {
    public static String generateUUID() {
        return UUID.randomUUID().toString();
    }
}
```

#### **Using It in Karate**
```gherkin
Feature: Java Background Task Example

Scenario: Generate a Random UUID
  * def RandomUtils = Java.type('utils.RandomUtils')
  * def uuid = RandomUtils.generateUUID()
  * print uuid
```
This allows using Java for generating dynamic test data.

---

## 6. Executing Java Code from `call`
You can also call Java methods using the `call` keyword.

#### **Example**
```gherkin
Feature: Calling Java from Karate

Scenario: Using call to execute Java code
  * def result = call read('classpath:utils/StringUtils.java')
  * print result
```
This approach helps modularize reusable Java logic.

---

## 7. Java Enum Example
Enums can be used in Karate to represent predefined constant values.

#### **Java Enum (`Status.java`)**
```java
package utils;

public enum Status {
    SUCCESS,
    FAILURE,
    PENDING;
}
```

#### **Using It in Karate**
```gherkin
Feature: Java Enum Example

Scenario: Use Enum in Karate
  * def Status = Java.type('utils.Status')
  * def currentStatus = Status.SUCCESS
  * print currentStatus
  * match currentStatus == 'SUCCESS'
```

---

## 8. Java List Manipulation Example
You can also use Java collections in Karate.

#### **Java Class (`ListUtils.java`)**
```java
package utils;

import java.util.Arrays;
import java.util.List;

public class ListUtils {
    public static List<String> getFruits() {
        return Arrays.asList("Apple", "Banana", "Cherry");
    }
}
```

#### **Using It in Karate**
```gherkin
Feature: Java List Example

Scenario: Use Java List in Karate
  * def ListUtils = Java.type('utils.ListUtils')
  * def fruits = ListUtils.getFruits()
  * print fruits
  * match fruits contains 'Apple'
  * match fruits contains 'Banana'
```

---

## 9. Environment-Specific Configuration
Karate allows setting up environment-specific configurations dynamically in `karate-config.js`.

### Example:
```javascript
function fn() {
  var env = karate.env; // get system property 'karate.env'
  var config = {};

  if (env === 'dev') {
    config.baseUrl = 'https://dev-api.example.com';
  } else if (env === 'qa') {
    config.baseUrl = 'https://qa-api.example.com';
  } else {
    config.baseUrl = 'https://prod-api.example.com';
  }
  return config;
}
```

### Running Tests with Different Environments:
```sh
mvn test -Dkarate.env=qa
```

**Key Points:**
- `karate.env` helps in switching configurations dynamically.
- You can pass the environment at runtime using `-Dkarate.env=qa`.

---

## Conclusion
Karate's **Java Interop** provides powerful capabilities for integrating Java logic into Karate tests. It allows:
- Calling **static and instance methods** from Java classes.
- Using Java objects in Karate tests, automatically converting them into JSON.
- Executing Java methods from **feature files, `karate-config.js`, and background tasks**.
- Utilizing **Java enums and collections** effectively in test automation.

This makes Karate a flexible tool that integrates well with Java-based frameworks. 🚀
