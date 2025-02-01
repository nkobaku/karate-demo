# Karate Configuration Guide

## Overview
Karate is a powerful API testing framework that allows you to write tests in a BDD-style syntax using Gherkin. This guide covers different levels of Karate configurations, including global configuration, feature-specific settings, shared configurations, and reusing data effectively.

---

## 1. Global Configuration
Global configurations allow you to set up common settings for all test executions. These settings are typically placed in a `karate-config.js` file at the root of your test project.

### Example:
```javascript
function fn() {
  var config = {};
  config.baseUrl = 'https://api.example.com';
  config.authToken = 'Bearer some-token';
  config.timeout = 5000;
  return config;
}
```

**Key Points:**
- This function initializes global variables.
- You can access these configurations in feature files using `karate.get('variableName')`.
- Environment-specific settings can be defined using `karate.env`.

---

## 2. Feature Configuration
Feature files in Karate allow you to define scenario-specific configurations. You can override global configurations within a specific feature file.

### Example:
```gherkin
Feature: User API Testing

Background:
  * url baseUrl
  * header Authorization = authToken
  * configure timeout = 10000

Scenario: Get User Details
  Given path 'users/1'
  When method GET
  Then status 200
```

**Key Points:**
- `Background` helps set up common configurations for all scenarios within a feature file.
- `configure timeout` overrides the default timeout for this feature.
- Variables from `karate-config.js` can be directly accessed.

---

## 3. Shared Configuration
For modularization and reusability, you can create a separate feature file for common configurations and call it in multiple feature files.

### Example of a shared configuration file (`common-config.feature`):
```gherkin
Feature: Common Configurations

Background:
  * def baseUrl = 'https://api.example.com'
  * def authToken = 'Bearer some-token'
```

### Using the shared configuration in another feature file:
```gherkin
Feature: User API Testing

Background:
  * call read('common-config.feature')

Scenario: Fetch User
  Given url baseUrl + '/users/1'
  When method GET
  Then status 200
```

**Key Points:**
- `call read('common-config.feature')` imports shared configurations.
- Helps avoid redundancy across multiple feature files.

---

## 4. Reusing Data
You can store and reuse data across tests using JSON files, JavaScript files, or within feature files.

### Example of reusing data with JSON:
#### `user-data.json`
```json
{
  "username": "testuser",
  "password": "Test@123"
}
```
#### Using it in a feature file:
```gherkin
Feature: Login API Test

Background:
  * def userData = read('user-data.json')

Scenario: Login with Valid Credentials
  Given url baseUrl + '/login'
  And request userData
  When method POST
  Then status 200
```

**Key Points:**
- `read('user-data.json')` loads external data for test cases.
- Useful for parameterization and avoiding hardcoded values.

---

## 5. Environment-Specific Configuration
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
Karate provides multiple ways to configure and manage test settings efficiently. Understanding and implementing global, feature-specific, shared, and reusable configurations help in maintaining scalable and well-structured test automation.

Happy Testing! 🚀
