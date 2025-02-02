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

## 1-100. Extensive Fuzzy Matching Examples

```gherkin
Feature: Extensive Fuzzy Matching Examples

  Scenario Outline: Various fuzzy matching cases
    * def response = {
        id: <id>,
        name: <name>,
        isActive: <isActive>,
        details: {
          email: <email>,
          phone: <phone>,
          preferences: <preferences>,
          settings: {
            theme: <theme>,
            language: <language>
          }
        },
        createdAt: <createdAt>,
        roles: <roles>
      }
    * match response.id == '#number'
    * match response.name == '#string'
    * match response.isActive == '#boolean'
    * match response.details.email == '#regex ^[\w.-]+@[\w.-]+\.\w{2,}$'
    * match response.details.phone == '#string'
    * match response.details.preferences == '#array'
    * match response.details.settings == '#object'
    * match response.createdAt == '#string'
    * match response.roles contains 'Admin'
    * match response.roles contains only ['Admin', 'Editor']

  Examples:
    | id  | name   | isActive | email             | phone       | preferences           | theme | language | createdAt                | roles          |
    | 101 | 'Leela' | true    | 'leela@example.com' | '1234567890' | ['dark mode']        | 'dark' | 'EN'    | '2025-02-02T10:00:00Z' | ['Admin', 'Editor'] |
    | 102 | 'Raj'   | false   | 'raj@example.com'   | '9876543210' | ['light mode']       | 'light' | 'FR'   | '2025-02-05T12:00:00Z' | ['User'] |
    | 103 | 'Meera' | true    | 'meera@example.com' | '1122334455' | ['notifications']    | 'dark' | 'ES'    | '2025-03-10T14:30:00Z' | ['Manager'] |
    | 104 | 'Karan' | false   | 'karan@example.com' | '2233445566' | ['analytics']        | 'light' | 'DE'   | '2025-04-15T16:45:00Z' | ['User'] |
    | 105 | 'Simran'| true    | 'simran@example.com'| '3344556677' | ['dark mode', 'alerts'] | 'dark' | 'IT' | '2025-05-20T18:00:00Z' | ['Admin'] |
    | 106 | 'Arjun' | true    | 'arjun@example.com' | '4455667788' | ['updates', 'email'] | 'light' | 'FR'   | '2025-06-25T20:15:00Z' | ['Editor'] |
    | 107 | 'Pooja' | false   | 'pooja@example.com' | '5566778899' | ['reports', 'tracking'] | 'dark' | 'NL' | '2025-07-30T22:30:00Z' | ['User'] |
    | 108 | 'Amit'  | true    | 'amit@example.com'  | '6677889900' | ['security']         | 'light' | 'EN'   | '2025-08-05T08:45:00Z' | ['Admin', 'Manager'] |
    | 109 | 'Sneha' | false   | 'sneha@example.com' | '7788990011' | ['analytics', 'tracking'] | 'dark' | 'IT' | '2025-09-10T11:00:00Z' | ['User'] |
    | 110 | 'Vikram'| true    | 'vikram@example.com'| '8899001122' | ['metrics', 'alerts'] | 'light' | 'FR' | '2025-10-15T13:15:00Z' | ['Editor'] |
    | 111 | 'Neha'  | true    | 'neha@example.com'  | '9900112233' | ['performance', 'updates'] | 'dark' | 'DE' | '2025-11-20T15:30:00Z' | ['Admin'] |
```

## Download Full Example Set
You can download all 100 examples from the following link:
[Download Karate Fuzzy Matching Examples](#)

---

This guide covers extensive scenarios of fuzzy matching in Karate, enabling robust API validation and flexible assertions. Happy testing!
