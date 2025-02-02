# Karate Framework README

## What is Karate?
Karate is an open-source API testing framework that allows testers and developers to write API test cases using a BDD (Behavior-Driven Development) approach. It simplifies API testing by combining API automation, performance testing, and UI testing into a single framework. Karate is built on top of Cucumber, making it easy to write tests in a human-readable format.

## When is Karate Used?
Karate is used for:
- API testing (REST and SOAP)
- UI automation testing
- Performance testing
- Data-driven testing
- Integration testing
- Mocking and stubbing
- JSON/XML validation

## Variables
Karate supports multiple variable types for storing and processing data:

- **def**: Defines a variable.
- **text**: Stores multi-line text.
- **table**: Defines a table structure.
- **yaml**: Stores data in YAML format.
- **csv**: Stores data in CSV format.
- **string**: Defines string variables.
- **json**: Stores JSON data.
- **xml**: Stores XML data.
- **xmlstring**: Stores XML data as a string.
- **bytes**: Stores binary data.
- **copy**: Creates a copy of a variable.

### Examples
```karate
Feature: Variable Examples

Scenario: Define variables
    * def name = 'John Doe'
    * def age = 30
    * def jsonData = { "name": "Alice", "age": 25 }
    * def xmlData = <person><name>Alice</name><age>25</age></person>
    * def tableData =
    """
    | name  | age |
    | John  | 30  |
    | Alice | 25  |
    """
```

## Actions
Karate provides built-in actions to perform various operations:

- **assert**: Checks if a condition is true.
- **print**: Prints output to the console.
- **replace**: Replaces values in a string.
- **get**: Extracts values from JSON or XML.
- **set**: Sets values in JSON or XML.
- **remove**: Removes elements from JSON or XML.
- **configure**: Configures test settings.
- **call**: Calls another feature file.
- **callonce**: Calls another feature file only once.
- **eval**: Evaluates JavaScript expressions.
- **listen**: Listens for an event.
- **doc**: Documents steps.
- **read()**: Reads a file.
- **compareImage**: Compares images.
- **karate JS API**: Executes JavaScript functions.

### Examples
```karate
Feature: Action Examples

Scenario: Demonstrating actions
    * def json = { "name": "John", "age": 30 }
    * assert json.age == 30
    * print "User name is", json.name
    * set json.city = 'New York'
    * remove json.age
    * print json
```

## HTTP Operations
Karate supports HTTP operations for API testing:

- **url**: Defines the base URL.
- **path**: Sets the API path.
- **request**: Defines the request payload.
- **method**: Specifies the HTTP method.
- **status**: Checks the HTTP response status.
- **soap action**: Defines SOAP action.
- **retry until**: Retries a request until a condition is met.

### Examples
```karate
Feature: HTTP Requests

Scenario: GET request
    Given url 'https://jsonplaceholder.typicode.com'
    And path 'users/1'
    When method get
    Then status 200
```

## Request Components
Karate supports different request components:

- **param**: Query parameters.
- **header**: Request headers.
- **cookie**: Cookies.
- **form field**: Form parameters.
- **multipart file**: Multipart file upload.
- **multipart field**: Multipart form fields.
- **multipart entity**: Multipart entity requests.
- **params, headers, cookies, form fields, multipart files, multipart fields**: Collections of parameters.

### Examples
```karate
Scenario: POST request with headers and form fields
    Given url 'https://api.example.com/login'
    And request { username: 'admin', password: '1234' }
    And header Content-Type = 'application/json'
    When method post
    Then status 200
```

## Response Components
Karate provides methods to validate responses:

- **response**: Stores the response body.
- **responseBytes**: Stores binary response.
- **responseStatus**: Stores the response status.
- **responseHeaders**: Stores response headers.
- **responseCookies**: Stores response cookies.
- **responseTime**: Stores response time.
- **responseType**: Stores response content type.
- **requestTimeStamp**: Stores request timestamp.

### Examples
```karate
Scenario: Validate response
    Given url 'https://jsonplaceholder.typicode.com/users/1'
    When method get
    Then status 200
    And match response.id == 1
    And print response
```

## Assertions
Karate provides assertion methods:

- **match ==, !=**: Exact match.
- **match contains**: Checks if JSON contains a value.
- **match contains only**: Matches exact content.
- **match contains any**: Matches any value.
- **match contains deep**: Deep match.
- **match !contains**: Negative contains match.
- **match each**: Applies assertion to all elements.
- **match header**: Matches headers.
- **Fuzzy Matching**: Uses regex patterns.
- **Schema Validation**: Validates JSON schema.
- **contains short-cuts**: Shortcut contains checks.

### Examples
```karate
Scenario: Validate JSON response
    Given url 'https://jsonplaceholder.typicode.com/users/1'
    When method get
    Then match response.name == 'Leanne Graham'
```

## Reuse & Advanced Features
Karate supports:
- **Calling Other Feature Files**
- **Data Driven Testing**
- **Calling JavaScript/Java Code**
- **Polling, Conditional Logic, Before/After Hooks**
- **GraphQL & Websockets Testing**

## More Features
- **Test Doubles** (Mocking APIs)
- **Performance Testing**
- **UI Testing**
- **Desktop Automation**
- **VS Code Integration**
- **Karate vs REST-assured & Cucumber**

---
This README provides a concise guide to Karate's powerful features. Happy testing! 🚀