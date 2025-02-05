# KaratePropertyService

## **Overview**
`KaratePropertyService` is a Java utility designed to manage YAML-based configuration properties efficiently. 
It supports environment-specific and external configurations, ensuring dynamic and flexible property management for modern applications.

---

## **Key Features**
1. **Environment-Specific Configurations**: Dynamically loads properties based on `karate.env`.
2. **External Configurations**: Supports external directories via `karate.config`.
3. **Flat Key-Value Structure**: Converts nested YAML to flat dot-separated keys.
4. **Fallback Mechanism**: Ensures configurations are always loaded.
5. **Error Handling**: Logs and gracefully handles missing or invalid files.
6. **Singleton Design**: Ensures a single service instance.

---

## **Configuration Loading Logic**

### **Logic Breakdown**
1. **If `karateConfig` is set**:
   - **If `karate.env` is set**, load:
     ```plaintext
     karateConfig + "application-" + karateEnv.toLowerCase() + ".yml"
     ```
   - **Else**, load:
     ```plaintext
     karateConfig + "application.yml"
     ```
2. **Else If `karate.env` is NULL, EMPTY, or `mock`**:
   - Load:
     ```plaintext
     src/test/resources/application.yml
     ```
3. **Else (Fallback)**:
   - Load:
     ```plaintext
     application-<env>.yml (internal resources)
     ```

---

## **Flat vs Nested Structures**

### **Flat Structure**
**Example YAML:**
```yaml
database:
  connection:
    host: localhost
    port: 3306
```
**Flattened Keys:**
```plaintext
database.connection.host = localhost
database.connection.port = 3306
```
**Advantages:**
- Direct access: `service.getProperty("database.connection.host")`
- Easy filtering: `service.getBulkProperties("database")`

### **Nested Structure Challenges**
**Access Example:**
```java
Map<String, Object> connection = (Map<String, Object>) config.get("database").get("connection");
String host = (String) connection.get("host");
```
**Issues:** Verbose, requires null checks, and is error-prone.

---

## **Usage Examples**

### **1. Load Configurations**
```java
KaratePropertyService service = KaratePropertyService.getInstance();
String host = service.getProperty("database.connection.host");
Properties dbProps = service.getBulkProperties("database");
```

### **2. Dynamic Updates**
```java
service.getProperties().put("database.connection.host", "new-host");
```

---

## **Best Practices**
1. Use `karate.config` for deployment-specific overrides.
2. Set `karate.env` for environment-specific configurations.
3. Combine nested YAML for readability with flat structures for efficiency.
4. Implement fallback mechanisms for robust configuration handling.

---

## **Conclusion**
`KaratePropertyService` simplifies configuration management by supporting external directories, environment-specific overrides, 
and fallback mechanisms. Its flat key-value approach ensures ease of access and integration, making it ideal for dynamic application needs.

