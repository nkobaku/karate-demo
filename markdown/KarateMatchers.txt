# Karate Framework - Matchers, Assertions, and Validations

## Introduction
Karate provides a rich set of matchers, assertions, and validation mechanisms to perform powerful API testing. It supports strict assertions, fuzzy matching, schema validation, and custom validation logic using JavaScript functions.

---

## 1. Basic Matchers
Karate provides `match` assertions for JSON, XML, and primitive data types.

### Example: Matching JSON Response
```gherkin
Feature: Basic Match Example

  Scenario: Validate JSON Response
    * def response = { name: 'John', age: 30, active: true }
    * match response.name == 'John'
    * match response.age == 30
    * match response.active == true
```

### Example: Matching XML Response
```gherkin
Feature: XML Match Example

  Scenario: Validate XML Response
    * def response = 
    """
    <user>
      <name>John</name>
      <age>30</age>
    </user>
    """
    * match response//name == 'John'
    * match response//age == '30'
```

---

## 2. Fuzzy Matchers
Fuzzy matchers allow flexible validation, ignoring some exact values.

### Example: Matching Without Exact Values
```gherkin
Feature: Fuzzy Match Example

  Scenario: Validate JSON with Fuzzy Match
    * def response = { id: '#notnull', name: '#string', age: '#number', active: '#boolean' }
    * match response.id == '#notnull'
    * match response.name == '#string'
    * match response.age == '#number'
    * match response.active == '#boolean'
```

### Available Fuzzy Matchers
| Matcher       | Description                  |
|--------------|------------------------------|
| `#string`    | Matches any string           |
| `#number`    | Matches any number           |
| `#boolean`   | Matches any boolean          |
| `#notnull`   | Ensures the value is present |
| `#array`     | Matches an array             |
| `#object`    | Matches an object            |
| `#null`      | Matches null values          |

---

## 3. Validating JSON Arrays
Karate provides easy ways to validate JSON arrays.

### Example: Validating JSON Arrays
```gherkin
Feature: JSON Array Validation

  Scenario: Validate JSON Array
    * def response = { users: [ { name: 'John' }, { name: 'Alice' } ] }
    * match response.users[0].name == 'John'
    * match response.users[1].name == 'Alice'
```

### Example: Checking If a Value Exists in an Array
```gherkin
Feature: Array Contains Example

  Scenario: Validate Array Contains Value
    * def response = { users: [ 'John', 'Alice', 'Bob' ] }
    * match response.users contains 'Alice'
```

---

## 4. Validating XML Data
Karate supports powerful XML assertions.

### Example: Validating XML Structure
```gherkin
Feature: XML Validation Example

  Scenario: Validate XML
    * def response = 
    """
    <users>
      <user name="John" age="30"/>
      <user name="Alice" age="25"/>
    </users>
    """
    * match response//user[@name='John']/@age == '30'
    * match response//user[@name='Alice']/@age == '25'
```

---

## 5. Using Regular Expressions in Assertions
Karate allows regex-based assertions using `#regex`.

### Example: Matching with Regular Expressions
```gherkin
Feature: Regex Match Example

  Scenario: Validate Email Format
    * def response = { email: 'test@example.com' }
    * match response.email == '#regex ^[\w.-]+@[\w.-]+\.\w{2,}$'
```

---

## 6. Custom Validations with JavaScript
Karate allows writing custom validation logic using JavaScript functions.

### Example: Custom Function Validation
```gherkin
Feature: Custom JavaScript Validation

  Scenario: Validate with Custom Function
    * def isAdult = function(age) { return age >= 18 }
    * def user = { name: 'John', age: 20 }
    * match isAdult(user.age) == true
```

---

## 7. Validating Response Status and Headers
Karate can validate HTTP response status and headers easily.

### Example: Validating Status Code
```gherkin
Feature: Validate HTTP Response

  Scenario: Validate HTTP Status Code
    Given url 'https://api.example.com/users'
    When method get
    Then status 200
```

### Example: Validating Response Headers
```gherkin
Feature: Validate Response Headers

  Scenario: Check Content-Type Header
    Given url 'https://api.example.com/users'
    When method get
    Then match responseHeaders['Content-Type'][0] == 'application/json'
```

---

## 8. Validating Response Time
Karate allows asserting response time using `responseTime`.

### Example: Response Time Validation
```gherkin
Feature: Validate Response Time

  Scenario: Check API Performance
    Given url 'https://api.example.com/users'
    When method get
    Then assert responseTime < 2000  # Response time must be under 2 seconds
```

---

## Conclusion
Karate provides powerful assertion and validation features, making API testing simple and flexible. From strict validation to fuzzy matchers and custom logic, Karate covers all use cases efficiently.

## Download Source Code
You can download the full example files from the following link:
[Download Karate Validation Examples](#)

---
