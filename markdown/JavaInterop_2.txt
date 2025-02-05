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

---

## Real-World Examples

### Example 1: Handling Database Connections in Java
```gherkin
* def DBHandler = Java.type('com.example.DBHandler')
* def connection = DBHandler.getConnection()
* def user = DBHandler.getUserById(connection, 101)
* print user
```

#### Java Class (`DBHandler.java`)
```java
package com.example;
import java.sql.*;
import java.util.HashMap;
import java.util.Map;

public class DBHandler {
    public static Connection getConnection() throws Exception {
        return DriverManager.getConnection("jdbc:mysql://localhost:3306/testdb", "user", "password");
    }
    
    public static Map<String, Object> getUserById(Connection conn, int id) throws Exception {
        String query = "SELECT * FROM users WHERE id = ?";
        PreparedStatement stmt = conn.prepareStatement(query);
        stmt.setInt(1, id);
        ResultSet rs = stmt.executeQuery();
        
        Map<String, Object> user = new HashMap<>();
        if (rs.next()) {
            user.put("id", rs.getInt("id"));
            user.put("name", rs.getString("name"));
        }
        return user;
    }
}
```

### Example 2: Encrypting Data with Java
```gherkin
* def CryptoUtil = Java.type('com.example.CryptoUtil')
* def encrypted = CryptoUtil.encrypt("Hello Karate")
* print encrypted
```

#### Java Class (`CryptoUtil.java`)
```java
package com.example;
import java.util.Base64;

public class CryptoUtil {
    public static String encrypt(String data) {
        return Base64.getEncoder().encodeToString(data.getBytes());
    }
}
```

### Example 3: Making an HTTP Call from Java
```gherkin
* def HttpClient = Java.type('com.example.HttpClient')
* def response = HttpClient.get("https://jsonplaceholder.typicode.com/posts/1")
* print response
```

#### Java Class (`HttpClient.java`)
```java
package com.example;
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpClient {
    public static String get(String urlStr) throws Exception {
        URL url = new URL(urlStr);
        HttpURLConnection conn = (HttpURLConnection) url.openConnection();
        conn.setRequestMethod("GET");
        
        BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
        String inputLine;
        StringBuilder content = new StringBuilder();
        while ((inputLine = in.readLine()) != null) {
            content.append(inputLine);
        }
        in.close();
        return content.toString();
    }
}
```

---

