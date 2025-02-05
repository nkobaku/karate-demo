# KaratePropertyService

## **Overview**
`KaratePropertyService` is a Java-based utility designed to manage and retrieve configuration properties efficiently. 
It is commonly used in applications that rely on YAML configuration files, enabling dynamic and environment-specific configurations. 
This service supports external configuration directories, environment-based overrides, and fallback mechanisms to ensure robust and 
flexible property management.

---

## **Key Features**
1. **Environment-Specific Configurations**:
   - Dynamically loads properties based on the `karate.env` system property.
   - Supports overriding default configurations using `application-<env>.yml` files.

2. **External Configurations**:
   - Allows configuration files to be stored in an external directory via the `karate.config` property.

3. **Flat Key-Value Structure**:
   - Converts nested YAML structures into flat dot-separated keys for easy access.

4. **Fallback Mechanism**:
   - Ensures configurations are loaded from internal resources if external files are unavailable.

5. **Error Handling**:
   - Logs meaningful error messages when files fail to load or parse.

6. **Singleton Design**:
   - Guarantees a single instance of the service across the application.

---

## **Configuration Loading Logic**

### **Step-by-Step Logic**
1. **Check for External Configurations (`karate.config`)**:
   - If provided, attempt to load configurations from the external directory.
2. **Check for Environment-Specific Configurations (`karate.env`)**:
   - Load `application-<env>.yml` if `karate.env` is set.
   - Fallback to `application.yml` if `karate.env` is not set.
3. **Test Environment Handling**:
   - If `karate.env` is `NULL`, `EMPTY`, or `mock`, load `application.yml` from `src/test/resources`.
4. **Final Fallback**:
   - Load `application-<env>.yml` from internal resources if no external or test configurations are available.

---

## **How Flat and Nested Structures Are Managed**

### **Flat Structure**
- Nested YAML configurations are flattened into a single-level structure with dot-separated keys for easier retrieval and filtering.

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
- Simplifies access:
  ```java
  String host = service.getProperty("database.connection.host");
  ```
- Efficient filtering using prefixes:
  ```java
  Properties dbProperties = service.getBulkProperties("database");
  ```

### **Nested Structure Challenges**
If the YAML structure were used as-is, accessing deeply nested properties would require multiple steps:
```java
Map<String, Object> database = (Map<String, Object>) config.get("database");
Map<String, Object> connection = (Map<String, Object>) database.get("connection");
String host = (String) connection.get("host");
```
**Issues:**
- Verbose and error-prone.
- Requires null checks and type casting.

---

## **Error Handling**
- Logs errors for missing or invalid configuration files:
  ```java
  log.error("External config file not loaded: " + filePath + " - " + e.getMessage());
  ```
- Ensures application continues running by falling back to defaults or internal resources.

---

## **Usage Examples**

### **1. Loading Configurations**
```java
KaratePropertyService service = KaratePropertyService.getInstance();

// Retrieve a single property
String host = service.getProperty("database.connection.host");

// Retrieve properties by prefix
Properties dbProperties = service.getBulkProperties("database");
dbProperties.forEach((key, value) -> System.out.println(key + " = " + value));
```

### **2. Dynamic Updates**
```java
// Update a property at runtime
service.getProperties().put("database.connection.host", "new-host");
```

---

## **Best Practices**
1. **Use `karate.config` for External Configurations**:
   - Store environment-specific configurations in an external directory to avoid modifying internal resources.

2. **Set `karate.env` for Environment-Specific Overrides**:
   - Use descriptive values like `dev`, `test`, or `prod`.

3. **Combine Flat and Nested Approaches**:
   - Maintain human-readable YAML files and flatten them for programmatic simplicity.

4. **Gracefully Handle Missing Configurations**:
   - Implement fallback mechanisms to ensure robust property management.

---

## **Conclusion**
`KaratePropertyService` is a robust utility for managing configurations in Java applications. 
By supporting external directories, environment-specific overrides, and fallback mechanisms, 
it provides flexibility and reliability for various deployment scenarios. Its use of flat structures ensures ease 
of access and filtering, making it ideal for modern application development.

