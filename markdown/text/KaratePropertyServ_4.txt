# KaratePropertyService

## **Overview**
The `KaratePropertyService` is a utility designed to load, manage, and retrieve configuration properties efficiently. 
It uses YAML files for configuration and supports hierarchical properties while offering methods to simplify property 
retrieval using a flat structure.

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

## **Supports External Configuration Directories using `karate.config`**
The `karate.config` system property allows users to specify an external directory where configuration files are stored. If present, the service prioritizes loading configurations from this directory before falling back to internal resources.

### **How it Works**
1. The application checks if the `karate.config` property is set.
2. If present, it constructs the path to the configuration files using this directory.
3. It attempts to load the YAML configuration file from the external directory first.
4. If the file is not found or cannot be loaded, it falls back to the internal configuration files.

### **Priority Order for Configuration Loading**
1. **External Directory (`karate.config`)**: Tries to load `application-[env].yml` from the external directory.
2. **Internal YAML File (`resources/application.yml`)**: If the external file is not found, falls back to the default internal configuration.
3. **Environment-Specific Internal File (`resources/application-[env].yml`)**: Tries to load an environment-specific configuration file.

### **Advantages of Using `karate.config`**
- **Allows externalized configurations** without modifying the application code.
- **Supports different environments dynamically** by specifying different directories for each deployment.
- **Provides a fallback mechanism** ensuring configurations are always available.

---

## **Handling `application.yml` and `application-<env>.yml` in KaratePropertyService**

### **What Happens When Both Files Exist?**
1. **`application.yml` is Always Loaded**:
   - This file contains the **default configuration** and is loaded for all environments.
   - Any properties found here apply to all environments unless overridden.

2. **`application-<env>.yml` is Loaded if `karate.env` is Set**:
   - If `karate.env=dev`, `application-dev.yml` will be loaded **on top of** `application.yml`.
   - Any property in `application-<env>.yml` will override the value in `application.yml`.

### **Example Scenario**
#### **`application.yml` (Global Default Configuration)**
```yaml
server:
  port: 8080

database:
  host: localhost
  username: root
  password: root
```
#### **`application-prod.yml` (Environment-Specific Configuration for `prod`)**
```yaml
server:
  port: 9090

database:
  host: prod-db-server
  username: admin
  password: securepassword
```
#### **If `karate.env=prod` is Set, the Final Configuration Will Be:**
```yaml
server:
  port: 9090   # Overridden by application-prod.yml

database:
  host: prod-db-server  # Overridden by application-prod.yml
  username: admin       # Overridden by application-prod.yml
  password: securepassword  # Overridden by application-prod.yml
```

### **How KaratePropertyService Handles This**
1. It **always** loads `application.yml` first.
2. If `karate.env` is set, it attempts to load `application-<env>.yml`.
3. If a key exists in both files, the value from `application-<env>.yml` **overwrites** the one from `application.yml`.
4. If `karate.config` is set, it tries to load configurations from an external directory before falling back to internal files.

### **Key Takeaways**
- `application.yml` **always loads** as the base configuration.
- `application-<env>.yml` **only loads if** `karate.env` is set.
- **Environment-specific files override default settings** in `application.yml`.
- **External configuration (`karate.config`) takes priority if present**.

---

## **Conclusion**
The `KaratePropertyService` provides a structured approach to managing configurations:
1. **Global defaults** reside in `application.yml`.
2. **Environment-specific overrides** are in `application-<env>.yml`.
3. **External configurations** are loaded first if specified.
4. **Final configurations result from merging these files**, ensuring flexibility and maintainability for different environments.

