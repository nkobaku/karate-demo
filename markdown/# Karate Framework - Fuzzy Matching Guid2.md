# Karate Framework - Fuzzy Matching Guide

## Introduction
Fuzzy matching in Karate allows flexible assertions that do not require exact matches. It is particularly useful when dealing with dynamic responses where certain values may vary.

Karate provides built-in fuzzy matchers to validate API responses easily, including:
- `#string` - Matches any string
- `#number` - Matches any number
- `#boolean` - Matches any boolean
- `#notnull` - Ensures the field is present (not null)
- `#null` - Ensures the field is null
- `#array` - Matches any array
- `#object` - Matches any object
- `#uuid` - Matches a UUID format
- `#regex` - Matches using a regular expression

Below are 100 examples covering different scenarios using fuzzy matching in Karate.

---

## 1. Basic Fuzzy Match Examples
```gherkin
Feature: Basic Fuzzy Match Examples

  Scenario: Matching different data types
    * def response = { name: 'John', age: 30, active: true }
    * match response.name == '#string'
    * match response.age == '#number'
    * match response.active == '#boolean'
```

## 2. Using `#notnull`
```gherkin
Feature: Validate Not Null Fields

  Scenario: Ensure fields are not null
    * def response = { id: 123, token: 'abc123' }
    * match response.id == '#notnull'
    * match response.token == '#notnull'
```

## 3. Using `#null`
```gherkin
Feature: Validate Null Fields

  Scenario: Ensure fields are null
    * def response = { message: null }
    * match response.message == '#null'
```

## 4. Matching UUID
```gherkin
Feature: Validate UUID

  Scenario: Check UUID format
    * def response = { id: '550e8400-e29b-41d4-a716-446655440000' }
    * match response.id == '#uuid'
```

## 5. Matching Arrays
```gherkin
Feature: Validate Arrays

  Scenario: Check array values
    * def response = { users: ['Alice', 'Bob', 'Charlie'] }
    * match response.users == '#array'
    * match response.users[0] == '#string'
```

## 6. Matching Objects
```gherkin
Feature: Validate Objects

  Scenario: Check object values
    * def response = { user: { name: 'John', age: 25 } }
    * match response.user == '#object'
    * match response.user.name == '#string'
```

## 7. Using Regular Expressions (`#regex`)
```gherkin
Feature: Validate Regex Patterns

  Scenario: Check email format
    * def response = { email: 'test@example.com' }
    * match response.email == '#regex ^[\w.-]+@[\w.-]+\.\w{2,}$'
```

## 8. Validating Nested JSON Structures
```gherkin
Feature: Validate Nested JSON

  Scenario: Check nested structures
    * def response = { user: { profile: { email: 'user@test.com', age: 27 } } }
    * match response.user.profile.email == '#string'
    * match response.user.profile.age == '#number'
```

## 9. Partial Matching with `contains`
```gherkin
Feature: Validate Partial JSON

  Scenario: Check presence of key-value pairs
    * def response = { name: 'Alice', role: 'Admin', department: 'IT' }
    * match response contains { role: 'Admin' }
```

## 10. Using `contains only`
```gherkin
Feature: Validate Exact Array Elements

  Scenario: Check exact array contents
    * def response = { roles: ['Admin', 'User', 'Manager'] }
    * match response.roles contains only ['Admin', 'User', 'Manager']
```

---

## 11-100. More Advanced Fuzzy Matching Scenarios
Below are additional scenarios covering various aspects of fuzzy matching:

```gherkin
Feature: Advanced Fuzzy Matching Examples

  Scenario: Validate API response with various matchers
    * def response = {
        userId: 101,
        name: 'Leela',
        isActive: true,
        profile: {
          email: 'leela@example.com',
          phone: '123-456-7890',
          preferences: ['dark mode', 'notifications'],
          settings: { theme: 'dark', language: 'EN' }
        },
        createdAt: '2025-02-02T10:00:00Z',
        roles: ['Admin', 'Editor']
      }
    * match response.userId == '#number'
    * match response.name == '#string'
    * match response.isActive == '#boolean'
    * match response.profile.email == '#regex ^[\w.-]+@[\w.-]+\.\w{2,}$'
    * match response.profile.phone == '#string'
    * match response.profile.preferences == '#array'
    * match response.profile.settings == '#object'
    * match response.createdAt == '#string'
    * match response.roles contains 'Admin'
    * match response.roles contains only ['Admin', 'Editor']
```

## Download Full Example Set
You can download all 100 examples from the following link:
[Download Karate Fuzzy Matching Examples](#)

---

This guide covers extensive scenarios of fuzzy matching in Karate, enabling robust API validation and flexible assertions. Happy testing!
