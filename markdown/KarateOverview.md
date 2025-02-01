# Karate Testing Framework - README

## 📌 Introduction

### 🚀 What is Karate?
Karate is an open-source test automation framework that supports:
- **API Testing**
- **UI Testing**
- **Mocking APIs**
- **Performance Testing (Gatling)**
- **Data-Driven Testing**
- **Polyglot Execution (GraalVM Support)**

Karate is based on **Cucumber and Gherkin syntax**, making it easy to write **readable and maintainable** test cases.

---

## 📌 Installation & Setup

### 🔹 Install Karate via Maven
Add the following dependency in your `pom.xml`:
```xml
<dependency>
    <groupId>com.intuit.karate</groupId>
    <artifactId>karate-junit5</artifactId>
    <version>1.5.0</version>
</dependency>
```

### 🔹 Install Karate via Gradle
```gradle
dependencies {
    testImplementation 'com.intuit.karate:karate-junit5:1.5.0'
}
```

### 🔹 Directory Structure
```
/src/test/java
    ├── karate-config.js  (Global Configuration)
    ├── features/
        ├── sample.feature  (Feature File)
    ├── runners/
        ├── TestRunner.java  (Test Runner)
```

---

## 📌 Karate Feature File Structure
A **Karate feature file** is written in **Gherkin syntax**.

### 🔹 Sample Feature File (`sample.feature`)
```karate
Feature: Sample API Test

Scenario: Validate API Response
    Given url 'https://api.example.com/users'
    When method get
    Then status 200
    And match response contains { name: 'John' }
```

---

## 📌 Karate Release Timeline

| Version | Year | Key Features |
|---------|------|--------------|
| **0.0 (2017)** | 2017 | Initial API Testing Framework |
| **0.9 (2018)** | 2018 | UI Testing (Selenium), API Mocking, Performance Testing |
| **1.0 (2020)** | 2020 | Standalone Framework (Removed Cucumber dependency) |
| **1.2 (2022)** | 2022 | Karate UI Recorder, JUnit 5 Support |
| **1.3 (2023)** | 2023 | **Switched to GraalVM JavaScript Engine** |
| **1.5 (2025)** | 2025 | **Java 17 Requirement, Playwright UI Support** |

---

## 📌 Conclusion
Karate is a **powerful, full-stack test automation framework** that integrates **API, UI, Mocking, Performance, and Polyglot capabilities**.

📢 **Start Automating with Karate Today!**
