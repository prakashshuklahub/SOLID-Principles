The **Interface Segregation Principle (ISP)** is the fourth principle in the SOLID design principles. It states that:

**"Clients should not be forced to depend on interfaces they do not use."**

In simpler terms, ISP encourages the creation of small, specific interfaces that are focused on a particular set of actions, rather than having large, general-purpose interfaces. This way, classes implementing the interface only need to focus on the methods they actually need, rather than being forced to implement methods that are irrelevant to them.

### Example Violating ISP (Wrong Approach)

Let's assume we have an interface for managing various employees, and the interface includes methods that not all types of employees need to implement. For example, not all employees may need access to a reporting method.

```java
// An interface for managing employee work
public interface Employee {
    void work();
    void report();
}

// A Developer class implements Employee but doesn't need 'report'
public class Developer implements Employee {
    @Override
    public void work() {
        System.out.println("Developer is working");
    }

    @Override
    public void report() {
        throw new UnsupportedOperationException("Developers don't report directly");
    }
}

// A Manager class implements Employee and needs both methods
public class Manager implements Employee {
    @Override
    public void work() {
        System.out.println("Manager is working");
    }

    @Override
    public void report() {
        System.out.println("Manager is reporting progress");
    }
}
```

#### **Violation of ISP**:
- The `Developer` class is forced to implement the `report()` method even though it's not needed, leading to an empty or exception-throwing method. This violates ISP because `Developer` is forced to depend on methods it doesn't use.

### Correct Approach: Adhering to ISP

We can fix this by splitting the large `Employee` interface into more focused interfaces. This way, a class will only implement the specific interfaces that are relevant to it.

```java
// Interface for general work-related tasks
public interface Workable {
    void work();
}

// Interface for reporting tasks
public interface Reportable {
    void report();
}

// Developer class implements only Workable
public class Developer implements Workable {
    @Override
    public void work() {
        System.out.println("Developer is writing code");
    }
}

// Manager class implements both Workable and Reportable
public class Manager implements Workable, Reportable {
    @Override
    public void work() {
        System.out.println("Manager is organizing the team");
    }

    @Override
    public void report() {
        System.out.println("Manager is reporting progress");
    }
}
```

### Key Points:
- **Workable Interface**: This interface focuses only on work-related tasks (`work()` method). Classes that perform work can implement this interface.
- **Reportable Interface**: This interface focuses only on reporting tasks (`report()` method). Only those classes that need reporting capabilities (e.g., `Manager`) will implement this interface.
- **Developer Class**: The `Developer` class only implements the `Workable` interface and is no longer forced to implement irrelevant methods like `report()`.
- **Manager Class**: The `Manager` class implements both `Workable` and `Reportable` interfaces, as it needs both functionalities.

### How This Adheres to ISP:
- Each class is only implementing the methods it actually needs, adhering to the Interface Segregation Principle.
- Classes are no longer forced to implement methods they don't use, leading to a cleaner and more maintainable design.

This ensures that interfaces remain small, focused, and relevant to the implementing classes, promoting better flexibility and adherence to the ISP principle.