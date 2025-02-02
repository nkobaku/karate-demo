# Karate Fuzzy Matching - README

## Introduction
Karate is a powerful testing framework that supports fuzzy matching, enabling flexible assertions when comparing objects and arrays. This is particularly useful in scenarios where values might change dynamically (e.g., timestamps, IDs) or when a partial comparison is required.

## Table of Contents
- [Fuzzy Matching Basics](#fuzzy-matching-basics)
- [Object Comparison](#object-comparison)
- [Array Comparison](#array-comparison)
- [Ignoring Fields in Objects](#ignoring-fields-in-objects)
- [Ignoring Fields in Arrays](#ignoring-fields-in-arrays)
- [Advanced Examples](#advanced-examples)

## Fuzzy Matching Basics
In Karate, fuzzy matching is achieved using placeholders that allow for approximate comparisons rather than exact matches.

### Common Fuzzy Matching Placeholders:
- `'#ignore'` - Ignores the field entirely.
- `'#present'` - Ensures the field exists but does not validate the value.
- `'#notpresent'` - Ensures the field is absent.
- `'#null'` - Ensures the field is null.
- `'#notnull'` - Ensures the field is not null.
- `'#string'` - Ensures the value is a string.
- `'#number'` - Ensures the value is a number.
- `'#boolean'` - Ensures the value is a boolean.
- `'#array'` - Ensures the value is an array.
- `'#object'` - Ensures the value is an object.

---
## Object Comparison
### Example 1: Exact Match
```karate
* def expected = { "name": "Alice", "age": 25 }
* match { "name": "Alice", "age": 25 } == expected
```

### Example 2: Ignoring a Field
```karate
* def expected = { "name": "Alice", "age": "#ignore" }
* match { "name": "Alice", "age": 30 } == expected  # Passes, as age is ignored
```

### Example 3: Type Checking
```karate
* def expected = { "name": "#string", "age": "#number" }
* match { "name": "Bob", "age": 40 } == expected  # Passes
* match { "name": 123, "age": 40 } == expected  # Fails
```

---
## Array Comparison
### Example 1: Exact Match
```karate
* def expected = [1, 2, 3]
* match [1, 2, 3] == expected
```

### Example 2: Ignoring Order
```karate
* def expected = "##[1,2,3]"  # Double hash ignores order
* match [3, 2, 1] == expected  # Passes
```

### Example 3: Checking Size
```karate
* def expected = "#[3]"  # Ensures the array has 3 elements
* match [10, 20, 30] == expected  # Passes
* match [10, 20] == expected  # Fails
```

---
## Ignoring Fields in Objects
### Example 1: Ignoring Multiple Fields
```karate
* def expected = { "name": "Alice", "age": "#ignore", "address": "#ignore" }
* match { "name": "Alice", "age": 25, "address": "NYC" } == expected  # Passes
```

### Example 2: Ignoring Nested Fields
```karate
* def expected = { "user": { "id": "#ignore", "name": "John" } }
* match { "user": { "id": 999, "name": "John" } } == expected  # Passes
```

---
## Ignoring Fields in Arrays
### Example 1: Ignoring Fields in Each Object
```karate
* def expected = [
    { "name": "Alice", "age": "#ignore" },
    { "name": "Bob", "age": "#ignore" }
]
* match [
    { "name": "Alice", "age": 30 },
    { "name": "Bob", "age": 35 }
] == expected  # Passes
```

### Example 2: Ignoring Order and Fields
```karate
* def expected = "##[ {"name": "Alice", "age": "#ignore"}, {"name": "Bob", "age": "#ignore"} ]"
* match [
    { "name": "Bob", "age": 35 },
    { "name": "Alice", "age": 30 }
] == expected  # Passes
```

---
## Advanced Examples
### Example 1: Checking Partial Object Match
```karate
* def expected = { "user": { "name": "John", "age": "#number" } }
* match { "user": { "name": "John", "age": 30, "city": "London" } } == expected  # Passes
```

### Example 2: Checking Arrays with Fuzzy Matching
```karate
* def expected = [ { "id": "#ignore", "value": "#number" } ]
* match [ { "id": 101, "value": 500 }, { "id": 102, "value": 600 } ] contains expected  # Passes
```

### Example 3: Ensuring a Value Exists in an Array
```karate
* def expected = "#array"
* match [10, 20, 30] == expected  # Passes
* match "Not an array" == expected  # Fails
```

### Example 4: Ensuring Objects Exist in Arrays
```karate
* def expected = [{"name": "Alice"}, {"name": "Bob"}]
* match [{"name": "Bob"}, {"name": "Alice"}] contains only expected  # Passes
```

---
## Conclusion
Karate’s fuzzy matching enables powerful, flexible assertions that are particularly useful when dealing with dynamic data structures. With `#ignore`, `#present`, `#number`, and array-specific options, you can create robust tests that validate data efficiently.

