The **Liskov Substitution Principle (LSP)** is the third principle in the SOLID design principles. It states that:

**"Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program."**

In simpler terms, if class `B` is a subclass of class `A`, then objects of class `A` should be able to be replaced with objects of class `B` without altering the desirable properties of the program, like correctness or expected behavior.

This principle ensures that derived classes (subclasses) should extend the base class without changing the functionality expected by the user. Violating LSP often happens when the subclass overrides a method in a way that breaks the functionality or constraints of the superclass.

### Example Violating LSP (Wrong Approach):

Consider a base class `Bird`, and two derived classes `Sparrow` and `Penguin`. Let’s see how LSP can be violated:

```java
// Base class Bird
public class Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }
}

// Derived class Sparrow that follows the behavior of Bird
public class Sparrow extends Bird {
    // Sparrow can fly, so it works as expected
}

// Derived class Penguin that does NOT follow the behavior of Bird
public class Penguin extends Bird {
    // Penguins can't fly, but the Penguin class inherits the fly() method
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins cannot fly!");
    }
}

// Main class to test
public class Main {
    public static void main(String[] args) {
        Bird sparrow = new Sparrow();
        sparrow.fly();  // Works fine

        Bird penguin = new Penguin();
        penguin.fly();  // Throws exception at runtime, violating LSP
    }
}
```

#### **Violation of LSP**:
- **Penguin Class**: While penguins are birds, they can't fly, so the `fly()` method is overridden to throw an exception. This breaks the Liskov Substitution Principle because if you replace a `Bird` object with a `Penguin` (which is a subclass), the program will break when calling the `fly()` method.

### Correct Approach: Adhering to LSP

To adhere to LSP, we should not expect every `Bird` to fly. We need to rethink the design so that the concept of "flying" is not forced upon all birds.

```java
// Base class Bird
public abstract class Bird {
    public abstract void makeSound();
}

// Interface for flying behavior
public interface Flyable {
    void fly();
}

// Class Sparrow extends Bird and implements Flyable
public class Sparrow extends Bird implements Flyable {
    @Override
    public void makeSound() {
        System.out.println("Sparrow chirps");
    }

    @Override
    public void fly() {
        System.out.println("Sparrow is flying");
    }
}

// Class Penguin extends Bird (but does NOT implement Flyable)
public class Penguin extends Bird {
    @Override
    public void makeSound() {
        System.out.println("Penguin honks");
    }
}

// Main class to test LSP
public class Main {
    public static void main(String[] args) {
        Bird sparrow = new Sparrow();
        sparrow.makeSound();
        ((Flyable) sparrow).fly(); // Sparrow can fly

        Bird penguin = new Penguin();
        penguin.makeSound(); // Penguin can make sound, but doesn't fly
    }
}
```

### Key Points:
- **Bird Class**: The `Bird` class no longer has a `fly()` method since not all birds can fly. It focuses on behaviors common to all birds, like making a sound.
- **Flyable Interface**: The `Flyable` interface is introduced for flying behavior. Now only birds that can fly (like `Sparrow`) implement this interface.
- **Penguin Class**: The `Penguin` class only inherits from `Bird` but does not implement `Flyable`. Therefore, it avoids inheriting any flying behavior that is not relevant.

### How This Adheres to LSP:
- You can substitute any `Bird` object (e.g., `Sparrow`, `Penguin`) without causing errors or unexpected behavior. A `Sparrow` can fly, and a `Penguin` cannot, but both can still be used as `Bird` objects without violating their expected behaviors. This respects the Liskov Substitution Principle.