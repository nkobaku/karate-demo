# KaratePropertyService

## **Overview**
The `KaratePropertyService` is a utility designed to load, manage, and retrieve configuration properties efficiently. 
It uses YAML files for configuration and supports hierarchical properties while offering methods to simplify property retrieval 
using a flat structure.

---

## **Key Features**
1. **Environment-Specific Configurations**:
   - Loads configurations dynamically based on the `karate.env` system property.
   - Supports external configuration directories using `karate.config`.

2. **Recursive Parsing**:
   - Flattens nested YAML structures into a flat key-value structure for efficient access.

3. **Filtering and Bulk Operations**:
   - Provides methods like `getBulkProperties()` to filter properties by prefix.

4. **Error Handling**:
   - Logs meaningful error messages when configurations fail to load or parse.

5. **Singleton Design**:
   - Ensures a single instance of the property cache is used across the application.

---

## **How It Works**

### **Initialization**
- Reads `karate.env` and `karate.config` system properties to determine which YAML files to load.
- Tries to load properties from the following locations (in order of priority):
  1. External directory specified by `karate.config`.
  2. Default `application.yml` file.
  3. Environment-specific file (e.g., `application-dev.yml`).

### **Parsing**
- Nested YAML configurations are parsed into a nested map.
- The `parse()` method recursively converts this nested map into a flat structure with dot-separated keys.

### **Property Retrieval**
- **Single Property**:
  Use `getProperty(String key)` to retrieve a specific property by key.
  ```java
  String host = service.getProperty("database.connection.host");
  ```
- **Filtered Properties**:
  Use `getBulkProperties(String parentKey)` to retrieve all properties starting with a specific prefix.
  ```java
  Properties dbProperties = service.getBulkProperties("database");
  ```

---

## **Flat vs Nested Structures**

### **Flat Structure**
The `KaratePropertyService` flattens nested YAML configurations into a dot-separated key-value format for better usability.

**Example YAML:**
```yaml
database:
  connection:
    host: localhost
    port: 3306
```
**Flattened Structure:**
```plaintext
database.connection.host = localhost
database.connection.port = 3306
```

**Advantages:**
1. **Easy Retrieval**:
   ```java
   String host = service.getProperty("database.connection.host");
   ```
2. **Filtering**:
   Retrieve all keys starting with `database`:
   ```java
   Properties dbProperties = service.getBulkProperties("database");
   ```
3. **Framework Compatibility**:
   Works seamlessly with frameworks like Spring Boot that expect flat properties.

### **Nested Structure**
If the nested YAML structure were used directly, retrieval would be cumbersome:
```java
Map<String, Object> database = (Map<String, Object>) config.get("database");
Map<String, Object> connection = (Map<String, Object>) database.get("connection");
String host = (String) connection.get("host");
```
**Challenges:**
1. Verbose and error-prone.
2. Requires null checks at every level.
3. Hard to filter or bulk-retrieve related keys.

### **Conclusion**
By flattening nested structures, the `KaratePropertyService` simplifies access, reduces verbosity, and aligns with modern frameworks, making it a more practical approach for configuration management.

---


## **Usage Examples**

### **Load and Retrieve Properties**
```java
KaratePropertyService service = KaratePropertyService.getInstance();

// Retrieve a single property
String host = service.getProperty("database.connection.host");

// Retrieve properties by prefix
Properties dbProperties = service.getBulkProperties("database");
dbProperties.forEach((key, value) -> System.out.println(key + " = " + value));
```

### **Dynamic Updates**
```java
// Update a property at runtime
service.getProperties().put("database.connection.host", "new-host");
```

---

## **Best Practices**
1. Use flat keys for easier access and compatibility.
2. Always handle `null` values gracefully when retrieving properties.
3. Use environment-specific YAML files to separate configurations for `dev`, `test`, and `prod` environments.

---

## **Conclusion**
The `KaratePropertyService` is a powerful utility for managing application configurations. 
Its use of flat structures simplifies property access, filtering, and updates, while its ability to parse nested 
YAML files ensures hierarchical clarity. This combination makes it an ideal choice for modern applications.

