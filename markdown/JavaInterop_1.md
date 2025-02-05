
## Java Interop in Karate
Karate allows seamless interaction with Java, enabling advanced test scenarios that require custom logic, external libraries, or data manipulation beyond Karate's built-in features. By integrating Java, you can:
- Call Java methods from feature files.
- Use Java classes for complex data handling.
- Perform advanced assertions and transformations.
- Reuse existing Java business logic within Karate tests.
- Convert data seamlessly between Java and JSON.

---

## Using Java Classes in Karate
To use Java classes within a Karate feature file, ensure the Java class is available in the classpath (`src/test/java`). You can reference Java classes using the `Java.type` function.

### Example: Referencing a Java Class
```gherkin
Feature: Using Java in Karate

Scenario: Call a Java method
    * def MyClass = Java.type('com.example.MyClass')
    * def result = MyClass.processData('Karate')
    * print result
    * match result == 'Processed: Karate'
```

### Corresponding Java Class (`MyClass.java`)
```java
package com.example;

public class MyClass {
    public static String processData(String input) {
        return "Processed: " + input;
    }
}
```

---

## Calling Java Methods from Feature Files
You can invoke static and instance methods of Java classes in Karate.

### Calling Static Methods
```gherkin
* def Utility = Java.type('com.example.Utility')
* def formattedDate = Utility.getCurrentDate()
* print formattedDate
```

### Calling Instance Methods
```gherkin
* def Person = Java.type('com.example.Person')
* def person = new Person('John', 30)
* def age = person.getAge()
* print age
* match age == 30
```

### Corresponding Java Class (`Person.java`)
```java
package com.example;

public class Person {
    private String name;
    private int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public int getAge() {
        return age;
    }
}
```

---

## Passing Data Between Karate and Java
Karate can pass complex data structures (JSON, lists, maps) to Java methods.

### Example: Passing JSON Data to Java
```gherkin
Feature: Passing JSON to Java

Scenario: Send JSON to Java
    * def MyProcessor = Java.type('com.example.MyProcessor')
    * def jsonData = { name: 'Alice', age: 25 }
    * def result = MyProcessor.process(jsonData)
    * print result
```

### Corresponding Java Class (`MyProcessor.java`)
```java
package com.example;
import java.util.Map;

public class MyProcessor {
    public static String process(Map<String, Object> data) {
        return "Received: " + data.get("name") + " (Age: " + data.get("age") + ")";
    }
}
```

---

## Type Conversion Between Java and JSON
Karate seamlessly converts between Java objects and JSON. Here’s how different types are handled:

### Conversion Table:
| Karate (JSON) | Java Equivalent |
|--------------|----------------|
| `{ key: 'value' }` | `Map<String, Object>` |
| `[1, 2, 3]` | `List<Integer>` |
| `["a", "b"]` | `List<String>` |
| `"string"` | `String` |
| `123` | `Integer` |
| `123.45` | `Double` |
| `true/false` | `Boolean` |
| `null` | `null` |
| `{ id: 1, name: 'Test' }` | `POJO (Plain Old Java Object)` |
| `[{ id: 1 }, { id: 2 }]` | `List<POJO>` |

### Example: Converting JSON to Java Map
```gherkin
* def jsonData = { name: 'John', age: 30 }
* def JavaConverter = Java.type('com.example.JavaConverter')
* def result = JavaConverter.convertToMap(jsonData)
* print result
```

### Corresponding Java Class (`JavaConverter.java`)
```java
package com.example;
import java.util.Map;

public class JavaConverter {
    public static Map<String, Object> convertToMap(Map<String, Object> json) {
        return json; // JSON in Karate is automatically converted to a Map
    }
}
```

### Example: Converting Java Object to JSON
```gherkin
* def Person = Java.type('com.example.Person')
* def person = new Person('Alice', 25)
* def json = karate.toJson(person)
* print json
```

### Corresponding Java Class (`Person.java`)
```java
package com.example;

public class Person {
    public String name;
    public int age;
    
    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

Karate automatically converts Java objects into JSON when using `karate.toJson()`, making it easier to handle data transformations.

---

## Best Practices & Performance Considerations
- **Avoid unnecessary Java calls**: Karate is powerful enough for most scenarios; use Java only when necessary.
- **Use static methods where possible**: They are more efficient and avoid object instantiation overhead.
- **Keep Java classes simple**: Use them for logic that cannot be efficiently handled in Karate.
- **Use `karate.toJson()` to convert Java objects** for easier integration.

---

