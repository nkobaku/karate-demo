# Karate Assertions Deep Dive

Karate is a powerful API testing framework that supports various assertions for validating responses. This guide covers different types of assertions in Karate, explaining their purpose and usage with examples.

## 1. `match ==`
The `match ==` assertion is used for exact matching of values. It ensures that the actual result is identical to the expected value.

### When to Use:
- To check if the response exactly matches the expected value.
- When order and data type matter.

### Examples:
```karate
* def response = { name: 'John', age: 30 }
* match response == { name: 'John', age: 30 }  # Passes
* match response == { age: 30, name: 'John' }  # Passes (order does not matter for JSON)
* match response == { name: 'John', age: 31 }  # Fails
* match response == { name: 'John' }  # Fails (missing key)
* match response == { name: 'John', age: '30' }  # Fails (data type mismatch)
```

## 2. `match !=`
The `match !=` assertion is used to ensure that two values are not identical.

### When to Use:
- To confirm that a response does not match an exact expected value.
- To check for incorrect or unexpected data.

### Examples:
```karate
* def response = { name: 'John', age: 30 }
* match response != { name: 'Jane', age: 30 }  # Passes
* match response != { name: 'John', age: 30 }  # Fails (exact match)
* match response != { name: 'John', age: 31 }  # Passes
* match response != { name: 'John' }  # Passes (missing key)
* match response != { name: 'John', age: '30' }  # Passes (data type mismatch)
```

## 3. `match contains`
The `match contains` assertion checks if the actual response contains the expected subset.

### When to Use:
- When the response should include specific values but may have additional data.

### Examples:
```karate
* def response = { name: 'John', age: 30, city: 'New York' }
* match response contains { name: 'John' }  # Passes
* match response contains { age: 30 }  # Passes
* match response contains { city: 'Los Angeles' }  # Fails
* match response contains { name: 'John', age: 30 }  # Passes
* match response contains { country: 'USA' }  # Fails (key does not exist)
```

## 4. `match contains only`
Ensures that the response contains only the specified values and nothing extra.

### When to Use:
- To validate that no extra fields exist in the response.

### Examples:
```karate
* def response = { name: 'John', age: 30 }
* match response contains only { name: 'John', age: 30 }  # Passes
* match response contains only { name: 'John' }  # Fails (age is extra)
* match response contains only { name: 'John', age: 30, city: 'New York' }  # Fails
* match response contains only { age: 30, name: 'John' }  # Passes (order does not matter)
* match response contains only {}  # Fails (response has values)
```

## 5. `match contains any`
Ensures that the response contains at least one of the specified values.

### When to Use:
- To check if at least one expected value is present.

### Examples:
```karate
* def response = { name: 'John', age: 30, city: 'New York' }
* match response contains any { name: 'John' }  # Passes
* match response contains any { age: 40, city: 'New York' }  # Passes
* match response contains any { country: 'USA' }  # Fails
* match response contains any { age: 30, country: 'USA' }  # Passes
* match response contains any {}  # Fails
```

## 6. `match contains deep`
Ensures that nested structures match partially.

### When to Use:
- To check if a deeply nested object contains expected values.

### Examples:
```karate
* def response = { user: { name: 'John', details: { age: 30, city: 'New York' } } }
* match response contains deep { user: { details: { age: 30 } } }  # Passes
* match response contains deep { user: { details: { city: 'Los Angeles' } } }  # Fails
* match response contains deep { user: { name: 'John' } }  # Passes
* match response contains deep { user: { country: 'USA' } }  # Fails
* match response contains deep { user: {} }  # Passes
```

## 7. `match each`
Checks that all elements in an array match a condition.

### When to Use:
- To validate arrays of objects or values.

### Examples:
```karate
* def response = [ { age: 30 }, { age: 25 }, { age: 35 } ]
* match each response == { age: '#number' }  # Passes
* match each response == { age: 30 }  # Fails
* match each response contains { age: 25 }  # Passes
* match each response contains { age: 40 }  # Fails
* match each response != { age: 40 }  # Passes
```

## 8. `match header`
Validates response headers.

### When to Use:
- To assert response headers.

### Examples:
```karate
* def responseHeaders = { Content-Type: 'application/json', Authorization: 'Bearer token' }
* match header responseHeaders.Content-Type == 'application/json'  # Passes
* match header responseHeaders.Authorization contains 'Bearer'  # Passes
* match header responseHeaders.Authorization == 'Basic token'  # Fails
* match header responseHeaders.Content-Type == 'text/html'  # Fails
* match header responseHeaders contains { 'Content-Type': 'application/json' }  # Passes
```

## 9. `Fuzzy Matching`
Uses special placeholders for flexible matching.

### When to Use:
- When values may vary but follow a pattern.

### Examples:
```karate
* match response == { name: '#string', age: '#number' }  # Passes
* match response == { name: 'John', age: '#string' }  # Fails
* match response == { name: '#string?', age: 30 }  # Passes
* match response == { name: '#notnull', age: '#notnull' }  # Passes
* match response == { age: '#regex [0-9]+' }  # Passes
```

## 10. `Schema Validation`
Validates response structure.

### When to Use:
- To enforce consistent response structure.

### Examples:
```karate
* match response == { id: '#uuid', email: '#email', phone: '#phone' }  # Passes
* match response == { id: '#string', email: '#string', phone: '#number' }  # Fails
```

## Conclusion
These assertions provide powerful ways to validate API responses in Karate, ensuring data correctness and integrity.
