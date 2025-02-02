# Karate Framework Guide

## Introduction
Karate is an open-source API testing framework that allows writing tests in a BDD (Behavior-Driven Development) style using Gherkin syntax. It simplifies API testing by providing built-in support for HTTP calls, assertions, JSON and XML validation, and more.

---

## 1. Data Sharing in Karate
Karate allows sharing data between scenarios, features, and steps using variables. Data can be stored in global, feature-level, or scenario-level variables.

### Example: Sharing Data Between Steps
```gherkin
Feature: Data Sharing Example

  Scenario: Share data within a scenario
    * def username = 'karateUser'
    * print 'Username is:', username
    * match username == 'karateUser'
```

### Example: Sharing Data Across Features
```gherkin
Feature: Call another feature to share data

  Scenario: Call another feature and reuse data
    * def result = call read('sharedData.feature')
    * print result.sharedValue
```

---

## 2. Reusing Features in Karate
Karate allows calling other feature files using the `call` keyword.

### Example: Calling Another Feature
```gherkin
Feature: Main Feature

  Scenario: Call a shared feature
    * def response = call read('common.feature')
    * print response
```

#### `common.feature`
```gherkin
Feature: Common Feature

  Scenario: Provide common data
    * def sharedValue = 'Hello from common feature'
    * print sharedValue
```

---

## 3. Data Tables in Karate
Karate supports data tables for parameterized tests.

### Example: Using a Data Table
```gherkin
Feature: Data Table Example

  Scenario: Validate multiple users
    * table users
      | name  | age |
      | John  | 25  |
      | Alice | 30  |
      | Bob   | 22  |
    
    * print users
```

---

## 4. Reading External Data in Karate
Karate supports reading external JSON, CSV, and XML files.

### Example: Reading JSON File
```gherkin
Feature: Read JSON Data

  Scenario: Load external JSON
    * def data = read('data.json')
    * print data
```
#### `data.json`
```json
{
  "name": "John Doe",
  "age": 28
}
```

---

## 5. Configurations in Karate
Karate supports multiple configuration levels:

### Global Configuration (`karate-config.js`)
This is the default configuration file loaded automatically.
```javascript
function fn() {
  return { baseUrl: 'https://api.example.com', timeout: 5000 };
}
```

### Feature-Level Configuration
```gherkin
Feature: Feature Level Config

  Background:
    * url baseUrl
```

### Scenario-Level Configuration
```gherkin
Feature: Scenario Level Config

  Scenario: Override URL
    * url 'https://api.test.com'
```

---

## 6. Generating Random UUID in Karate
You can generate a random UUID using JavaScript inside Karate.

### Example: UUID Generation
```gherkin
Feature: Generate UUID

  Scenario: Create unique identifier
    * def uuid = java.util.UUID.randomUUID() + ''
    * print uuid
```

---

## Download Source Code
You can download the full example files from the following link:
[Download Karate Examples](#)

---

Karate is a powerful testing framework with built-in support for API, UI, and performance testing. By leveraging its features efficiently, you can automate complex testing scenarios easily!
