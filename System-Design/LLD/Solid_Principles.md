# SOLID Principles

## Index

1. [Single Responsibility Principle (SRP)](#1-single-responsibility-principle-srp)
2. [Open/Closed Principle (OCP)](#2-openclosed-principle-ocp)
3. [Liskov Substitution Principle (LSP)](#3-liskov-substitution-principle-lsp)
4. [Interface Segregation Principle (ISP)](#4-interface-segregation-principle-isp)
5. [Dependency Inversion Principle (DIP)](#5-dependency-inversion-principle-dip)

---

## 1. Single Responsibility Principle (SRP)

A class should have only one reason to change or a single responsibility.

### Example Without SRP

```java
class UserManager {
    public void addUser(String username, String email) {
        System.out.println("User added: " + username);
        sendEmail(email, "Welcome to our system!");
    }

    private void sendEmail(String email, String message) {
        System.out.println("Sending email to " + email + ": " + message);
    }
}
```

**Problems:**
- The `UserManager` class handles both user management and email-sending operations.
- Any change in email functionality impacts user management logic.

### Example With SRP

```java
// Handles email-related operations
class EmailService {
    public void sendEmail(String email, String message) {
        System.out.println("Sending email to " + email + ": " + message);
    }
}

// Handles user-related operations
class UserManager {
    private EmailService emailService;

    public UserManager(EmailService emailService) {
        this.emailService = emailService;
    }

    public void addUser(String username, String email) {
        System.out.println("User added: " + username);
        emailService.sendEmail(email, "Welcome to our system!");
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        EmailService emailService = new EmailService();
        UserManager userManager = new UserManager(emailService);

        userManager.addUser("JohnDoe", "john@example.com");
    }
}
```

**Advantages:**
- Clear separation of concerns.
- Easier maintenance.
- Reusable `EmailService` for other parts of the system.

---

## 2. Open/Closed Principle (OCP)

A class should be open for extension but closed for modification.

### Example Without OCP

```java
class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.radius * circle.radius;
        } else if (shape instanceof Rectangle) {
            Rectangle rectangle = (Rectangle) shape;
            return rectangle.length * rectangle.width;
        }
        return 0;
    }
}

class Circle {
    double radius;
    public Circle(double radius) {
        this.radius = radius;
    }
}

class Rectangle {
    double length, width;
    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }
}
```

**Problem:** Adding new shapes requires modifying `AreaCalculator`, violating OCP.

### Example With OCP

```java
abstract class Shape {
    public abstract double calculateArea();
}

class Circle extends Shape {
    double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    double length, width;

    public Rectangle(double length, double width) {
        this.length = length;
        this.width = width;
    }

    @Override
    public double calculateArea() {
        return length * width;
    }
}

class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        Shape circle = new Circle(5);
        Shape rectangle = new Rectangle(4, 6);

        AreaCalculator calculator = new AreaCalculator();
        System.out.println("Circle Area: " + calculator.calculateArea(circle));
        System.out.println("Rectangle Area: " + calculator.calculateArea(rectangle));
    }
}
```

**Advantages:**
- Easily extendable by creating new `Shape` subclasses.
- Existing code remains untouched.

---

## 3. Liskov Substitution Principle (LSP)

Subtypes must be replaceable for their base types without breaking the application.

### Example Without LSP

```java
class Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Penguin extends Bird {
    @Override
    public void fly() {
        throw new UnsupportedOperationException("Penguins can't fly!");
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        Bird bird = new Penguin();
        bird.fly(); // This breaks the program
    }
}
```

**Problem:** Penguins cannot fly, violating LSP.

### Example With LSP

```java
abstract class Bird {
    public abstract void eat();
}

class FlyingBird extends Bird {
    public void fly() {
        System.out.println("Flying...");
    }
}

class Sparrow extends FlyingBird {
    @Override
    public void eat() {
        System.out.println("Eating seeds...");
    }
}

class Penguin extends Bird {
    @Override
    public void eat() {
        System.out.println("Eating fish...");
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        Bird sparrow = new Sparrow();
        Bird penguin = new Penguin();

        sparrow.eat();
        penguin.eat();

        if (sparrow instanceof FlyingBird) {
            ((FlyingBird) sparrow).fly();
        }
    }
}
```

---

## 4. Interface Segregation Principle (ISP)

Interfaces should be specific to what the client needs, avoiding unnecessary methods.

### Example Without ISP

```java
interface RestaurantStaff {
    void cookFood();
    void serveFood();
}

class Cook implements RestaurantStaff {
    @Override
    public void cookFood() {
        System.out.println("Cooking food...");
    }

    @Override
    public void serveFood() {
        throw new UnsupportedOperationException("Cook cannot serve food!");
    }
}

class Waiter implements RestaurantStaff {
    @Override
    public void cookFood() {
        throw new UnsupportedOperationException("Waiter cannot cook food!");
    }

    @Override
    public void serveFood() {
        System.out.println("Serving food...");
    }
}
```

**Problem:** Classes implement methods they don’t need.

### Example With ISP

```java
interface CookingStaff {
    void cookFood();
}

interface ServingStaff {
    void serveFood();
}

class Cook implements CookingStaff {
    @Override
    public void cookFood() {
        System.out.println("Cooking food...");
    }
}

class Waiter implements ServingStaff {
    @Override
    public void serveFood() {
        System.out.println("Serving food...");
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        CookingStaff cook = new Cook();
        ServingStaff waiter = new Waiter();

        cook.cookFood();
        waiter.serveFood();
    }
}
```

---

## 5. Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules; both should depend on abstractions. Classes should on interfaces rahter than concrete classes.

### Example Without DIP

```java
class Keyboard {
    public void type() {
        System.out.println("Typing...");
    }
}

class Computer {
    private Keyboard keyboard;

    public Computer() {
        this.keyboard = new Keyboard();
    }

    public void useKeyboard() {
        keyboard.type();
    }
}
```

### Example With DIP

```java
interface InputDevice {
    void input();
}

class Keyboard implements InputDevice {
    @Override
    public void input() {
        System.out.println("Typing...");
    }
}

class Mouse implements InputDevice {
    @Override
    public void input() {
        System.out.println("Clicking...");
    }
}

class Computer {
    private InputDevice inputDevice;

    public Computer(InputDevice inputDevice) {
        this.inputDevice = inputDevice;
    }

    public void useInputDevice() {
        inputDevice.input();
    }
}

// Client Code
public class Main {
    public static void main(String[] args) {
        InputDevice keyboard = new Keyboard();
        InputDevice mouse = new Mouse();

        Computer computerWithKeyboard = new Computer(keyboard);
        computerWithKeyboard.useInputDevice();

        Computer computerWithMouse = new Computer(mouse);
        computerWithMouse.useInputDevice();
    }
}
```

Advantages of Following DIP

Flexibility: You can swap out one input device (e.g., Keyboard) for another (e.g., Mouse) without changing the Computer class.

Easier Maintenance: High-level modules are not tightly coupled to specific low-level modules.

Extensibility: Adding new input devices (e.g., a Trackpad) requires no changes to the existing Computer class—just implement the InputDevice interface.
