The **Single Responsibility Principle (SRP)** is one of the five SOLID principles of object-oriented programming. It states that a class should have only one reason to change, meaning it should have only one responsibility or job. Here's an example in Java that illustrates SRP.

### Scenario:
Imagine you are building an employee management system. One part of the system is to handle employee data, and another part is responsible for generating reports. According to the Single Responsibility Principle, each of these tasks should be handled by a different class.

#### Example with SRP:

```java
// Employee class that handles only employee data
public class Employee {
    private String name;
    private String id;
    private double salary;

    public Employee(String name, String id, double salary) {
        this.name = name;
        this.id = id;
        this.salary = salary;
    }

    // Getter and Setter methods for employee data
    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public double getSalary() {
        return salary;
    }

    public void setSalary(double salary) {
        this.salary = salary;
    }

    @Override
    public String toString() {
        return "Employee [name=" + name + ", id=" + id + ", salary=" + salary + "]";
    }
}

// ReportGenerator class that handles only report generation
public class ReportGenerator {

    public String generateReport(Employee employee) {
        // Logic to generate a simple report based on employee details
        return "Employee Report:\n" + employee.toString();
    }
}

// Main class to test SRP
public class Main {
    public static void main(String[] args) {
        // Create an employee
        Employee emp = new Employee("John Doe", "EMP001", 60000);

        // Generate report
        ReportGenerator reportGenerator = new ReportGenerator();
        String report = reportGenerator.generateReport(emp);

        // Print the report
        System.out.println(report);
    }
}
```

### Key Points:
- **Employee Class**: Handles only the employee data (name, ID, salary). It does not deal with report generation, ensuring SRP.
- **ReportGenerator Class**: Handles only the generation of reports. It takes an `Employee` object as input and generates a report based on its data.

This separation ensures that changes in one responsibility (like report generation) do not affect other responsibilities (like employee data management), adhering to the Single Responsibility Principle.