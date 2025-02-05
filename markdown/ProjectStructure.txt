## 📂 Project Structure

```
/bdd-module/
    ├── build.gradle  (Gradle Build Script)
    ├── src/test/java/  (Test configurations, runners, utilities)
        ├── config/  (Karate & Spring Boot Test Context Setup)
        ├── runner/  (Parallel test execution runners)
        ├── util/  (Reusable Java utilities for feature files)
    ├── src/test/resources/
        ├── features/  (Karate feature files)
        ├── environments/  (Spring Boot bean configurations)
        ├── testdata/  (Data-driven testing resources - JSON, CSV, YAML)
        ├── stubs/  (WireMock API stubs)
            ├── mappings/  (Stub request-response mappings)
            ├── __files/  (Mock response files)
        ├── mocks/  (Karate Netty mock feature files)
        ├── karate-config.js  (Global Karate configuration)
        ├── application.yaml  (Global test configuration)
        ├── application-local.yml  (Local mock environment configuration)
        ├── application-<env>.yml  (Environment-specific configurations)
```

---

## Directory Breakdown

### 📁 `src/test/java/config/`

Holds **Karate & Spring Boot Test Context** configurations for dependency injection and test environment setup.

### 📁 `src/test/java/runner/`

Contains parallel test execution runners:

- `MockTestRunner.java` → Runs **mock-based** Karate tests.
- `TestRunner.java` → Executes feature tests in **parallel mode**.

### 📁 `src/test/java/util/`

Utility classes for:

- Reusable helper functions.
- API response processing.
- Data transformation & custom functions.

### 📁 `src/test/resources/features/`

Contains **Karate feature files** in **Gherkin syntax**.

- Modular and reusable test scenarios.
- Supports **data-driven testing** with variables.

### 📁 `src/test/resources/environments/`

Bootstraps **Spring Beans** used in Karate tests:

- **Database configurations**.
- **Kafka producer/publisher beans**.
- **Common services** frequently accessed in tests.

### 📁 `src/test/resources/testdata/`

Stores **test data files**:

- JSON → API payloads.
- CSV → Data-driven test cases.
- YAML → Structured configurations.

### 📁 `src/test/resources/stubs/`

Houses **WireMock API stubs**:

- `mappings/` → Defines request-response mappings.
- `__files/` → Stores mock response payloads.

### 📁 `src/test/resources/mocks/`

Used for **Karate Netty-based mock services**:

- Creates **lightweight in-memory services**.
- Simulates API interactions for independent testing.

### 📁 `src/test/resources/karate-config.js`

Global Karate configuration file for:

- Environment variable management.
- Defining global test settings (timeouts, base URLs).
- Injecting dynamic values into feature files.

### 📁 `src/test/resources/application.yaml`

- Central configuration file for test parameters.
- Used across feature files for shared settings.

### 📁 `src/test/resources/application-local.yml`

- Local configuration for **mock environments**.
- Helps simulate API responses when external services are unavailable.

### 📁 `src/test/resources/application-<env>.yml`

- Stores **environment-specific configurations**.
- Allows switching configurations dynamically without modifying feature files.

