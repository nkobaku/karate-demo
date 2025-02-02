# Karate Schema Validation - README

## Introduction
Karate is a powerful testing framework that supports schema validation, enabling efficient and flexible assertions on API responses. Schema validation ensures that JSON responses adhere to the expected structure, helping maintain API reliability.

## Table of Contents
- [Schema Validation Basics](#schema-validation-basics)
- [Defining JSON Schema](#defining-json-schema)
- [Validating Response Against Schema](#validating-response-against-schema)
- [Using Fuzzy Matching in Schema](#using-fuzzy-matching-in-schema)
- [Validating Nested Objects](#validating-nested-objects)
- [Validating Arrays](#validating-arrays)
- [Advanced Examples](#advanced-examples)

## Schema Validation Basics
Karate supports schema validation using built-in matchers and fuzzy matching. You can define expected JSON structures and validate API responses against them.

### Example: Basic Schema Validation
```karate
* def schema = { "name": "#string", "age": "#number", "active": "#boolean" }
* def response = { "name": "Alice", "age": 30, "active": true }
* match response == schema  # Passes if types match
```

---
## Defining JSON Schema
You can define reusable JSON schemas to validate multiple API responses.

```karate
* def userSchema =
  {
    "id": "#number",
    "name": "#string",
    "email": "#string",
    "isActive": "#boolean"
  }
```

---
## Validating Response Against Schema
### Example: Validating API Response
```karate
* def response =
  {
    "id": 101,
    "name": "John Doe",
    "email": "john@example.com",
    "isActive": true
  }

* match response == userSchema  # Passes
```

---
## Using Fuzzy Matching in Schema
Fuzzy matching allows more flexibility in schema validation.

### Example: Using `#ignore`
```karate
* def schema = { "name": "#string", "age": "#ignore" }
* match { "name": "Bob", "age": 35 } == schema  # Passes
```

### Example: Using `#present`
```karate
* def schema = { "id": "#present", "email": "#string" }
* match { "id": 99, "email": "user@example.com" } == schema  # Passes
* match { "email": "user@example.com" } == schema  # Fails (id missing)
```

---
## Validating Nested Objects
You can validate JSON responses that contain nested objects.

```karate
* def schema =
  {
    "user": {
      "id": "#number",
      "profile": {
        "firstName": "#string",
        "lastName": "#string"
      }
    }
  }

* def response =
  {
    "user": {
      "id": 123,
      "profile": {
        "firstName": "Jane",
        "lastName": "Doe"
      }
    }
  }

* match response == schema  # Passes
```

---
## Validating Arrays
Arrays can be validated by ensuring each element matches a defined schema.

### Example: Validating an Array of Objects
```karate
* def schema = [{ "id": "#number", "name": "#string" }]
* def response = [
    { "id": 1, "name": "Alice" },
    { "id": 2, "name": "Bob" }
  ]
* match response == schema  # Passes
```

### Example: Ensuring Non-Empty Arrays
```karate
* def schema = "#[] #present"
* match [10, 20, 30] == schema  # Passes
* match [] == schema  # Fails (empty array)
```

---
## Advanced Examples
### Example: Combining Matchers in Schema
```karate
* def schema = {
    "id": "#number",
    "name": "#string",
    "email": "#regex .+@.+\\..+",
    "isActive": "#boolean"
  }

* def response = {
    "id": 101,
    "name": "Alice",
    "email": "alice@example.com",
    "isActive": false
  }

* match response == schema  # Passes
```

### Example: Validating Mixed Data Types
```karate
* def schema = {
    "id": "#number",
    "tags": "#array",
    "metadata": "#object"
  }

* def response = {
    "id": 100,
    "tags": ["karate", "testing"],
    "metadata": { "version": 1 }
  }

* match response == schema  # Passes
```

---
## Conclusion
Karate’s schema validation feature enables robust API testing by enforcing structural and type correctness. With built-in matchers, fuzzy matching, and array validation, you can ensure API responses conform to expected standards efficiently.

