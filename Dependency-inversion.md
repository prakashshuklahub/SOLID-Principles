The **Dependency Inversion Principle (DIP)** is the fifth and final principle in the SOLID design principles. It states that:

**"High-level modules should not depend on low-level modules. Both should depend on abstractions."**

**"Abstractions should not depend on details. Details should depend on abstractions."**

### **Explanation**:

The key idea of the Dependency Inversion Principle is to reduce the dependency between high-level and low-level modules by introducing an abstraction (such as interfaces or abstract classes). The high-level module (the one that contains complex business logic) should not directly depend on low-level modules (such as services, utilities, or specific implementations). Instead, both should depend on abstractions, making the system more flexible and maintainable.

This principle promotes:
- **Loose Coupling**: High-level classes don’t directly depend on low-level classes.
- **Flexibility**: It’s easier to switch out implementations, making the system more adaptable to changes.
- **Testability**: It becomes easier to mock dependencies and perform unit testing on the high-level modules.

### Example Violating DIP (Wrong Approach)

In this example, the `OrderService` class (high-level module) depends directly on a `MySQLDatabase` class (low-level module). If we ever want to change the database or replace it with another storage system (like a NoSQL database), we would need to modify the `OrderService` class, violating DIP.

```java
// Low-level module: MySQLDatabase class
public class MySQLDatabase {
    public void saveOrder() {
        System.out.println("Order saved in MySQL database.");
    }
}

// High-level module: OrderService class depends directly on MySQLDatabase
public class OrderService {
    private MySQLDatabase mySQLDatabase;

    public OrderService() {
        this.mySQLDatabase = new MySQLDatabase();
    }

    public void placeOrder() {
        // High-level module directly depends on low-level implementation
        mySQLDatabase.saveOrder();
    }
}

// Main class to test DIP violation
public class Main {
    public static void main(String[] args) {
        OrderService orderService = new OrderService();
        orderService.placeOrder();
    }
}
```

#### **Violation of DIP**:
- The `OrderService` class directly depends on `MySQLDatabase`, a specific implementation.
- If we want to change the storage system, we will have to modify the `OrderService` class, which violates the principle.

### Correct Approach: Adhering to DIP

In this version, we introduce an abstraction in the form of an `IDatabase` interface. The `OrderService` class depends on this abstraction instead of a concrete implementation like `MySQLDatabase`. Now, both high-level (`OrderService`) and low-level (`MySQLDatabase` and `PostgreSQLDatabase`) modules depend on the abstraction (`IDatabase`).

```java
// Abstraction: IDatabase interface
public interface IDatabase {
    void saveOrder();
}

// Low-level module: MySQLDatabase class implements IDatabase
public class MySQLDatabase implements IDatabase {
    @Override
    public void saveOrder() {
        System.out.println("Order saved in MySQL database.");
    }
}

// Low-level module: PostgreSQLDatabase class implements IDatabase
public class PostgreSQLDatabase implements IDatabase {
    @Override
    public void saveOrder() {
        System.out.println("Order saved in PostgreSQL database.");
    }
}

// High-level module: OrderService depends on IDatabase abstraction
public class OrderService {
    private IDatabase database;

    // The database is injected via constructor (Dependency Injection)
    public OrderService(IDatabase database) {
        this.database = database;
    }

    public void placeOrder() {
        // High-level module depends on abstraction, not the implementation
        database.saveOrder();
    }
}

// Main class to test DIP adherence
public class Main {
    public static void main(String[] args) {
        // We can easily switch databases without changing the OrderService class
        IDatabase mySQLDatabase = new MySQLDatabase();
        OrderService orderService = new OrderService(mySQLDatabase);
        orderService.placeOrder();

        // Switch to PostgreSQL
        IDatabase postgreSQLDatabase = new PostgreSQLDatabase();
        orderService = new OrderService(postgreSQLDatabase);
        orderService.placeOrder();
    }
}
```

#### **Adhering to DIP**:
- **IDatabase Interface**: Acts as the abstraction. Both `MySQLDatabase` and `PostgreSQLDatabase` implement this interface.
- **OrderService Class**: Depends on the `IDatabase` abstraction, not a specific implementation.
- **Dependency Injection**: The specific database implementation is injected into `OrderService` via the constructor. This allows the high-level module to remain flexible and easily switch between different database implementations without any changes in its code.

### **Benefits of Applying DIP**:
1. **Loose Coupling**: The `OrderService` class is not tightly coupled to a specific database implementation. If you need to change the database technology, you don't need to modify `OrderService`, just provide a different implementation of `IDatabase`.

2. **Testability**: Since `OrderService` depends on an abstraction, you can easily mock the `IDatabase` interface in unit tests without depending on an actual database implementation.

3. **Flexibility**: You can switch between `MySQLDatabase`, `PostgreSQLDatabase`, or any future implementation without modifying high-level business logic.

---

By adhering to the Dependency Inversion Principle, we decouple high-level and low-level components and make the system more maintainable, flexible, and scalable.