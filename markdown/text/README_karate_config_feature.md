# Understanding `karate-config.js` Execution in Karate

In Karate, the `karate-config.js` file is executed **once per feature file**. This means that for each feature file executed, `karate-config.js` is invoked anew.

## 📌 Project Structure

```
karate-config-demo/
│── src/test/java/
│   ├── ParallelTestRunner.java  # Runs feature files in parallel
│── src/test/resources/
│   ├── karate-config.js         # Global configuration file
│   ├── feature1.feature         # First test feature
│   ├── feature2.feature         # Second test feature
│── README.md
```

## 🚀 How It Works

- **`karate-config.js` runs at the start of each feature file execution**, setting up variables.
- Each feature file gets a fresh instance of the global configuration.

## 📝 `karate-config.js` Example

### **karate-config.js**
```javascript
function fn() {
    karate.log("karate-config.js is executed");
    return { baseUrl: 'https://api.example.com' };
}
```
- This file prints `"karate-config.js is executed"` every time it runs.
- It sets the `baseUrl` variable, which will be used in feature files.

## 📝 Feature Files

### **feature1.feature**
```gherkin
Feature: Feature 1 - Karate Config Execution Test

  Scenario: Scenario 1 in Feature 1
    Given print "Executing Scenario 1 in Feature 1"
    * print "Base URL: " + karate.get('baseUrl')
```
### **feature2.feature**
```gherkin
Feature: Feature 2 - Karate Config Execution Test

  Scenario: Scenario 1 in Feature 2
    Given print "Executing Scenario 1 in Feature 2"
    * print "Base URL: " + karate.get('baseUrl')
```
## 🏆 Test Runner

### **ParallelTestRunner.java**
```java
import com.intuit.karate.junit5.Karate;

class ParallelTestRunner {

    @Karate.Test
    Karate testParallelExecution() {
        return Karate.run("classpath:feature1", "classpath:feature2").parallel(2);
    }
}
```
- Runs **both feature files in parallel**.

## ✅ Expected Output

```
karate-config.js is executed
Executing Scenario 1 in Feature 1
Base URL: https://api.example.com

karate-config.js is executed
Executing Scenario 1 in Feature 2
Base URL: https://api.example.com
```
- Notice that **`karate-config.js` is executed twice**—once for each feature file.

## 🔥 Key Takeaways

✅ `karate-config.js` **executes once per feature file**, ensuring independent configurations for each test.  
✅ Each feature file gets a **fresh instance of the global configuration**.  
✅ Use it to set environment variables like `baseUrl`, `env`, or API keys.  

## 📚 Learn More

- [Karate Official Documentation](https://karatelabs.github.io/karate/)
- [Parallel Execution in Karate](https://karatelabs.github.io/karate/#parallel-execution)

This setup ensures that each feature file gets a **separate execution of karate-config.js** while maintaining global configurations! 🚀

