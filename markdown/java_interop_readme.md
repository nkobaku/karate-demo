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

## Conclusion
Karate's **Java Interop** provides powerful capabilities for integrating Java logic into Karate tests. It allows:

- Calling **static and instance methods** from Java classes.

- Using Java objects in Karate tests, automatically converting them into JSON.

- Executing Java methods from **feature files, `karate-config.js`, and background tasks**.

- Utilizing **Java enums and collections** effectively in test automation.


This makes Karate a flexible tool that integrates well with Java-based frameworks. 🚀
