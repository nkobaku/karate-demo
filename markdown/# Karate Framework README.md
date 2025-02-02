# Karate Framework README

## What is Karate?
Karate is an open-source API testing framework that allows users to create automated API tests using a domain-specific language inspired by Gherkin. It is built on top of Cucumber and provides powerful features for testing REST, SOAP, GraphQL, and other API types. Karate also supports UI testing, desktop automation, performance testing, and test doubles.

## When is Karate Used?
Karate is used when:
- Automating API testing in an easy-to-read format.
- Testing REST and SOAP APIs with minimal configuration.
- Performing data-driven testing using external files.
- Validating JSON and XML responses with built-in match assertions.
- Automating UI and desktop testing along with API testing.

---
## Variables
Karate supports various data types for variables:
- **def**: Defines a variable.
- **text**: Defines a multi-line string.
- **table**: Stores tabular data.
- **yaml**: Reads YAML files.
- **csv**: Reads CSV data.
- **string**: Defines a string variable.
- **json**: Defines a JSON object.
- **xml**: Defines an XML object.
- **xmlstring**: Defines an XML string.
- **bytes**: Defines a binary object.
- **copy**: Copies a variable value.

### Examples:
```karate
Feature: Variables Example
  Scenario: Define and use variables
    * def name = 'Karate'
    * def jsonObject = { "key": "value" }
    * def xmlData = <root><item>value</item></root>
    * print name, jsonObject, xmlData
```

---
## Actions
Karate provides built-in actions to manipulate and verify data:
- **assert**: Validates expressions.
- **print**: Prints output.
- **replace**: Replaces string values.
- **get**: Extracts values from JSON/XML.
- **set**: Sets new values.
- **remove**: Removes values.
- **configure**: Sets global configurations.
- **call**: Calls another feature file.
- **callonce**: Calls a feature file once.
- **eval**: Evaluates JavaScript expressions.
- **listen**: Listens to async responses.
- **doc**: Generates documentation.
- **read()**: Reads external files.
- **compareImage**: Compares images.
- **karate JS API**: Executes JavaScript in Karate.

### Examples:
```karate
Feature: Actions Example
  Scenario: Use actions in Karate
    * def num = 10
    * assert num == 10
    * print 'Hello, Karate!'
    * def text = 'karate automation'
    * replace text 'karate' = 'api'
    * print text
```

---
## HTTP Operations
Karate supports HTTP operations such as:
- **url**: Sets the base URL.
- **path**: Appends to the base URL.
- **request**: Sets the request payload.
- **method**: Specifies the HTTP method.
- **status**: Validates the response status.
- **soap action**: Sends SOAP requests.
- **retry until**: Retries a request until a condition is met.

### Examples:
```karate
Feature: HTTP Operations
  Scenario: Send an API request
    * url 'https://jsonplaceholder.typicode.com'
    * path 'posts'
    * method get
    * status 200
```

---
## Request Elements
Request elements supported:
- **param**: URL query parameters.
- **header**: Custom headers.
- **cookie**: Sets cookies.
- **form field**: Form fields for POST requests.
- **multipart file**: Uploads files.
- **multipart field**: Sends form data.
- **multipart entity**: Sends complex multipart data.
- **params, headers, cookies, form fields, multipart files, multipart fields**: Collections of parameters.

### Examples:
```karate
Feature: Request Elements
  Scenario: Set request parameters
    * url 'https://example.com'
    * param id = 1
    * header Authorization = 'Bearer token'
    * method get
    * status 200
```

---
## Response Elements
Karate provides built-in response elements:
- **response**: Holds the full response body.
- **responseBytes**: Binary response.
- **responseStatus**: Response HTTP status code.
- **responseHeaders**: Response headers.
- **responseCookies**: Response cookies.
- **responseTime**: API response time.
- **responseType**: Response content type.
- **requestTimeStamp**: Timestamp of request.

### Examples:
```karate
Feature: Response Handling
  Scenario: Validate API response
    * url 'https://jsonplaceholder.typicode.com/posts/1'
    * method get
    * status 200
    * print response
```

---
## Assertions
Karate provides powerful assertion methods:
- **match ==**: Exact match.
- **match !=**: Not equal match.
- **match contains**: Partial match.
- **match contains only**: Exact match ignoring order.
- **match contains any**: At least one match.
- **match contains deep**: Deep match for complex objects.
- **match contains only deep**: Exact deep match.
- **match !contains**: Exclusion match.
- **match each**: Iterates over collections.
- **match each contains deep**: Deep match per item.
- **match header**: Header match.
- **Fuzzy Matching**: Flexible matching.
- **Schema Validation**: JSON schema validation.
- **contains short-cuts**: Short-hand contains match.

### Examples:
```karate
Feature: Assertions
  Scenario: Validate API response
    * def expected = { id: 1, title: '#ignore', body: '#string' }
    * url 'https://jsonplaceholder.typicode.com/posts/1'
    * method get
    * match response == expected
```

---
## Advanced Topics
Karate supports advanced features like:
- **Polling**: Repeated API calls.
- **Conditional Logic**: If-else handling.
- **Before/After Hooks**: Pre/post-test execution.
- **JSON Transforms**: Data manipulation.
- **Loops**: Iterative testing.
- **HTTP Basic Auth**: Authentication.
- **Header Manipulation**: Dynamic headers.
- **GraphQL**: GraphQL API testing.
- **Websockets/Async**: Asynchronous testing.
- **call vs read()**: Reusability comparison.

### Example:
```karate
Feature: Polling Example
  Scenario: Retry API request
    * url 'https://example.com'
    * retry until responseStatus == 200
    * method get
    * status 200
```

---
## More Features
Karate supports:
- **Test Doubles**: Mock services.
- **Performance Testing**: Load testing.
- **UI Testing**: Browser automation.
- **Desktop Automation**: Windows/macOS app testing.
- **VS Code/Debug**: IDE debugging.
- **Karate vs REST-assured/Cucumber**: Comparison.
- **Examples and Demos**: Ready-to-use samples.

---
This document provides an overview of Karate's key features for API and automation testing.