# Calling JavaScript Functions in Karate

## Overview
Karate allows calling JavaScript functions in multiple ways. You can define and invoke JavaScript functions directly within feature files, external `.js` files, or dynamically in `karate-config.js`. This guide covers various approaches to using JavaScript functions in Karate.

---

## 1. Inline JavaScript Functions
You can define and call JavaScript functions inline within a feature file.

### Example:
```gherkin
Feature: Inline JavaScript Functions

Scenario: Call JavaScript Function Inline
  * def add = function(a, b) { return a + b }
  * def result = add(5, 3)
  * print result
  * match result == 8
```

### Example: Inline Function Returning JSON
```gherkin
Feature: Inline Function Returning JSON

Scenario: Create JSON Object Using JavaScript
  * def createUser = function(name, age) { return { name: name, age: age } }
  * def user = createUser('Alice', 25)
  * print user
  * match user.name == 'Alice'
  * match user.age == 25
```

**Key Points:**
- Functions can be defined inline using the `function` keyword.
- The function `add(a, b)` is called with parameters `5` and `3`.
- JavaScript functions can return JSON objects, which Karate automatically converts.

---

## 2. Using JavaScript Functions from `karate-config.js`
You can define JavaScript functions globally in `karate-config.js` and use them across all feature files.

### Example:
#### **karate-config.js**
```javascript
function fn() {
  var config = {};
  config.multiply = function(a, b) { return a * b };
  config.getCurrentTimestamp = function() { return new Date().toISOString(); };
  return config;
}
```

#### **Using in a Feature File:**
```gherkin
Feature: Use JavaScript Function from karate-config.js

Scenario: Multiply Numbers
  * def result = multiply(4, 6)
  * print result
  * match result == 24

Scenario: Get Current Timestamp
  * def timestamp = getCurrentTimestamp()
  * print timestamp
```

**Key Points:**
- The function `multiply(a, b)` is defined in `karate-config.js`.
- The function `getCurrentTimestamp()` returns the current timestamp.
- Both functions are available in all feature files.

---

## 3. Calling External JavaScript Files
You can store JavaScript functions in a separate `.js` file and import them into your feature files.

### Example:
#### **helper.js**
```javascript
function subtract(a, b) {
  return a - b;
}

function generateRandomString(length) {
  var chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
  var result = '';
  for (var i = 0; i < length; i++) {
    result += chars.charAt(Math.floor(Math.random() * chars.length));
  }
  return result;
}
```

#### **Using in a Feature File:**
```gherkin
Feature: Calling JavaScript from External File

Scenario: Subtract Numbers
  * def subtract = read('classpath:helper.js')
  * def result = subtract(10, 4)
  * print result
  * match result == 6

Scenario: Generate Random String
  * def generateRandomString = read('classpath:helper.js')
  * def randomStr = generateRandomString(8)
  * print randomStr
```

**Key Points:**
- JavaScript functions can be stored in external `.js` files.
- Use `read('classpath:helper.js')` to load the function.
- Functions can generate random values dynamically.

---

## 4. Using JavaScript for Data Manipulation
JavaScript functions are useful for manipulating JSON and arrays in Karate.

### Example:
```gherkin
Feature: JavaScript for Data Manipulation

Scenario: Modify JSON Data
  * def user = { name: 'John', age: 30 }
  * def updateAge = function(user, newAge) { user.age = newAge; return user }
  * def updatedUser = updateAge(user, 35)
  * print updatedUser
  * match updatedUser.age == 35
```

### Example: Filter Array Elements
```gherkin
Feature: Filter Array Using JavaScript

Scenario: Find Even Numbers
  * def numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9]
  * def findEven = function(arr) { return arr.filter(x => x % 2 == 0) }
  * def evens = findEven(numbers)
  * print evens
  * match evens == [2, 4, 6, 8]
```

**Key Points:**
- JavaScript functions can modify JSON objects.
- The function `updateAge` updates the `age` property dynamically.
- The function `findEven` filters numbers from an array.

---

## 5. Using JavaScript Functions to Generate Test Data
JavaScript functions can generate dynamic test data, such as timestamps or random values.

### Example:
```gherkin
Feature: Generate Test Data

Scenario: Generate a Random Number
  * def randomNumber = function() { return Math.floor(Math.random() * 100) }
  * def number = randomNumber()
  * print number
```

### Example: Generate Unique Email
```gherkin
Feature: Generate Unique Email

Scenario: Create Random Email
  * def randomEmail = function() { return 'user' + Math.floor(Math.random() * 1000) + '@test.com' }
  * def email = randomEmail()
  * print email
```

**Key Points:**
- JavaScript functions can generate unique test data dynamically.
- Functions can create random numbers and formatted strings.

---

## Conclusion
Karate provides flexible ways to call JavaScript functions, including:
- **Inline JavaScript functions** defined in feature files.
- **Global JavaScript functions** from `karate-config.js`.
- **External JavaScript files** using `read()`.
- **Data manipulation** functions for JSON and arrays.
- **Generating test data dynamically**.
- **Callback functions** for advanced logic.
- **Filtering and modifying arrays** using JavaScript.

Using JavaScript in Karate enhances the ability to handle complex test scenarios and dynamic data. 🚀
