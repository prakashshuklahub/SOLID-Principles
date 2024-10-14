The **Open-Closed Principle (OCP)** is another key concept from the SOLID principles of object-oriented programming. It states that:

**"Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification."**

This means that the behavior of a class can be extended without modifying its source code. This can be achieved through abstraction, inheritance, and composition. By doing this, the system remains flexible and adaptable without risking breaking existing code.

### Scenario:
Let's assume we are building a system to calculate the area of different shapes. Initially, we start with a rectangle, but later on, we need to support more shapes like circles or triangles. Instead of modifying the existing code every time a new shape is added, we can extend the system by adding new classes that adhere to the existing structure.

#### Example following OCP:

```java
// Base interface for Shape
public interface Shape {
    double calculateArea();
}

// Rectangle class that implements Shape
public class Rectangle implements Shape {
    private double length;
    private double width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double calculateArea() {
        return length * width;
    }
}

// Circle class that implements Shape
public class Circle implements Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

// Triangle class that implements Shape
public class Triangle implements Shape {
    private double base;
    private double height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}

// AreaCalculator class to calculate area
public class AreaCalculator {
    public double calculateTotalArea(Shape[] shapes) {
        double totalArea = 0;
        for (Shape shape : shapes) {
            totalArea += shape.calculateArea(); // Polymorphism in action
        }
        return totalArea;
    }
}

// Main class to test Open-Closed Principle
public class Main {
    public static void main(String[] args) {
        // Create shapes
        Shape rectangle = new Rectangle(5, 10);
        Shape circle = new Circle(7);
        Shape triangle = new Triangle(6, 8);

        // Add shapes to an array
        Shape[] shapes = { rectangle, circle, triangle };

        // Calculate total area
        AreaCalculator areaCalculator = new AreaCalculator();
        double totalArea = areaCalculator.calculateTotalArea(shapes);

        System.out.println("Total Area: " + totalArea);
    }
}
```

### Key Points:
- **Shape Interface**: This defines the contract (method `calculateArea()`) for all shapes. Any new shape (e.g., `Rectangle`, `Circle`, `Triangle`) can implement this interface without modifying existing code.
- **Rectangle, Circle, and Triangle Classes**: These are specific implementations of `Shape`. Each class has its own logic for calculating the area.
- **AreaCalculator Class**: This class works with the `Shape` interface to calculate the total area. It uses polymorphism so it doesn't need to change when new shapes are added.

#### **How This Follows OCP**:
- The system is **closed for modification** because `AreaCalculator` does not need to change its logic when new shapes are added.
- The system is **open for extension** because we can easily add new shapes (e.g., `Square`, `Polygon`, etc.) by implementing the `Shape` interface without modifying the existing code.

This way, new functionality (like new shapes) can be added without altering the existing code structure, adhering to the Open-Closed Principle.