# Karate Parallel Execution Example

This project demonstrates how to run **Karate feature files in parallel**, while scenarios within a feature file execute sequentially.

## 📌 Project Structure

```
karate-parallel-demo/
│── src/test/java/
│   ├── ParallelTestRunner.java  # Runs feature files in parallel
│── src/test/resources/
│   ├── feature1.feature  # First test feature
│   ├── feature2.feature  # Second test feature
│── README.md
```

## 🚀 How It Works

- **Feature files execute in parallel.**
- **Scenarios inside a feature file execute sequentially.**

## 📝 Feature Files

### **feature1.feature**
```gherkin
Feature: Feature 1 - Parallel Execution Test

  Scenario: Scenario 1 in Feature 1
    Given print "Executing Scenario 1 in Feature 1"
    * karate.delay(2000)  # Simulating delay

  Scenario: Scenario 2 in Feature 1
    Given print "Executing Scenario 2 in Feature 1"
    * karate.delay(2000)
```

### **feature2.feature**
```gherkin
Feature: Feature 2 - Parallel Execution Test

  Scenario: Scenario 1 in Feature 2
    Given print "Executing Scenario 1 in Feature 2"
    * karate.delay(2000)

  Scenario: Scenario 2 in Feature 2
    Given print "Executing Scenario 2 in Feature 2"
    * karate.delay(2000)
```

## 🔥 Running the Tests

Execute the following command to run feature files in parallel:

```sh
mvn test
```

## 🏆 Test Runner

The Java test runner is responsible for executing feature files in parallel.

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

- `.parallel(2)` means **two feature files** will run simultaneously.
- Scenarios within each feature file **execute sequentially**.

## ✅ Expected Output

```
Executing Scenario 1 in Feature 1
Executing Scenario 1 in Feature 2
(wait 2 seconds)
Executing Scenario 2 in Feature 1
Executing Scenario 2 in Feature 2
```

## 📚 Learn More

- [Karate Official Documentation](https://karatelabs.github.io/karate/)
- [Parallel Execution in Karate](https://karatelabs.github.io/karate/#parallel-execution)

---

### 🎯 Summary

✅ Feature files execute **in parallel**  
✅ Scenarios within a feature file execute **sequentially**  
✅ Use `Runner.parallel(n)` to control parallel execution  

This setup is useful for **faster test execution** in large projects! 🚀
