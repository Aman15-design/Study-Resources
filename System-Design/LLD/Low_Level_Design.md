
# Low Level Design

## Singleton Method Design Pattern

The Singleton design pattern ensures that the class has only one single instance and provides global access to that instance. There are multiple use cases for the Singleton class, and one of them is thread safety.

### How it can be achieved:

1. **Static Members**: Static members ensure that memory is allocated only once, thereby allowing us to create only a single instance of an object.

   ```java
   private static Singleton instance;
   ```

2. **Private Constructor**: The private constructor acts as a barricade, preventing the reinitialization of an object that should remain a singleton.

   ```java
   class Singleton {
       private Singleton() {
           // initialization here
       }
   }
   ```

3. **Static Factory Method**: The static factory method acts as a gateway to check if the object has already been initialized or not.

   ```java
   private static Singleton getInstance() {
       if (instance == null)
           instance = new Singleton();
       return instance;
   }
   ```

### Advantages of Singleton Pattern:
- **Thread Safety**: Ensures that only one instance of the class is created even in a multi-threaded environment.
- **Memory Efficiency**: Useful for resource-intensive applications as it reduces memory usage by allowing the creation of a single instance.

---

## Factory Method Pattern

The Factory Method Pattern delegates the task of object creation to subclasses or child classes, allowing for greater scalability. The superclass provides an interface for creating objects, while the subclass can decide the type of object to be created.

### Example:

```java
public interface Vehicle {
    void myCar();
}

class Honda extends Vehicle {
    @Override
    public void myCar() {
        System.out.println("My car is Honda Elevate");
    }
}

class Hyundai extends Vehicle {
    @Override
    public void myCar() {
        System.out.println("My car is Hyundai Venue");
    }
}
```

In the example above, the subclasses are able to alter the objects based on requirements, enabling greater flexibility.

### Use Cases:
- Object creation varies based on certain conditions.
- Encourages loose coupling, as new objects can easily be added based on new requirements.

---

## Abstract Factory Pattern

The Abstract Factory Pattern provides an additional layer of abstraction above the Factory Pattern, focusing on groups of related products.

### Example:

```java
public interface VehicleFactory {
    Vehicle createCar();
    Speed createTopSpeed();
}

public interface Vehicle {
    void myCar();
}

public interface Speed {
    void topSpeed();
}

class Honda implements Vehicle {
    @Override
    public void myCar() {
        System.out.println("My car is Honda Elevate");
    }
}

class HondaSpeed implements Speed {
    @Override
    public void topSpeed() {
        System.out.println("The top speed of Honda is 150 km/h");
    }
}
```

In the above example, the `Vehicle` and `Speed` interfaces implement the Factory Pattern, while the `VehicleFactory` interface is the Abstract Factory Pattern.

---

## Observer Design Pattern

The Observer Design Pattern is a behavioral pattern that defines a one-to-many dependency between objects. When the subject (one object) changes state, all its dependents (observers) are notified and updated automatically.

### Example Scenario:
Imagine you're hosting a newsletter:
- **You (Subject)**: The one with news or updates to share.
- **Subscribers (Observers)**: People who are interested in your updates.
- **Notification (Update)**: Whenever you have news, you send it to all your subscribers.

### How it Works:
- When someone subscribes, they receive notifications (updates) automatically when there's news.
- Observers can unsubscribe at any time if they no longer wish to receive updates.

### Code Implementation:

```java
// Abstract Subject interface
public interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers();
}

// Concrete Subject class
class BreakingNews implements Subject {
    private List<Observer> observers = new ArrayList<>();
    private String news;

    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    public void setNews(String news) {
        this.news = news;
        notifyObservers(); // Notify all observers
    }

    public void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(news); // Send news to all observers
        }
    }
}

// Abstract Observer interface
public interface Observer {
    void update(String news);
}

// Concrete Observer class
class Person implements Observer {
    private String name;

    public Person(String name) {
        this.name = name;
    }

    public void update(String news) {
        System.out.println(name + " received news: " + news);
    }
}

// Main class
public class ObserverExample {
    public static void main(String[] args) {
        BreakingNews newsChannel = new BreakingNews(); // Concrete Subject
        Observer john = new Person("John"); // Concrete Observer
        Observer alice = new Person("Alice"); // Concrete Observer

        newsChannel.addObserver(john); // John subscribes
        newsChannel.addObserver(alice); // Alice subscribes

        newsChannel.setNews("Big storm coming tomorrow!"); // Notify all subscribers
    }
}
```

### Key Points:
- **Automation**: Observers are automatically notified when the subject changes.
- **Loose Coupling**: The subject doesn’t need to know about the observers’ actions with the updates.

### Real-Life Examples:
- **Social Media Notifications**: You follow a page, and whenever the page posts something, you’re notified.
- **YouTube Subscriptions**: You subscribe to a channel, and whenever a new video is uploaded, you’re notified.


## Builder Design Pattern

Builder design patterns allow us to create a complex object step by step. That is, suppose you have a very complex object wherein we have an ‘n’ number of fields. One way to create that object is to know all these `n` number of fields and write them while declaring that object in the given order, which could be very difficult when we have hundreds of variables.

The builder pattern removes this complexity by allowing us to build the object step by step.

### Example: Building a House

#### Without Builder Pattern
We might end up with messy constructors with too many arguments, like this:

```java
class House {
    private String roof;
    private String walls;
    private String windows;

    public House(String roof, String walls, String windows) {
        this.roof = roof;
        this.walls = walls;
        this.windows = windows;
    }
}

// Creating a house
House house = new House("Red Roof", "Brick Walls", "Glass Windows");
```

Now there are only three variables in this example, so it's not difficult. However, if there are too many optional or complex parts, it becomes hard to manage.

#### With Builder Pattern
Here's a cleaner approach:

```java
// Product class
class House {
    private String roof;
    private String walls;
    private String windows;

    // Private constructor
    private House(Builder builder) {
        this.roof = builder.roof;
        this.walls = builder.walls;
        this.windows = builder.windows;
    }

    // Builder Class
    public static class Builder {
        private String roof;
        private String walls;
        private String windows;

        public Builder setRoof(String roof) {
            this.roof = roof;
            return this;
        }

        public Builder setWalls(String walls) {
            this.walls = walls;
            return this;
        }

        public Builder setWindows(String windows) {
            this.windows = windows;
            return this;
        }

        public House build() {
            return new House(this);
        }
    }
}

// Using the builder to construct a house
House house = new House.Builder()
                  .setRoof("Red Roof")
                  .setWalls("Brick Walls")
                  .setWindows("Glass Windows")
                  .build();
```

### Benefits of the Builder Pattern
- **Readable and Maintainable**: No messy constructors with lots of parameters.
- **Step-by-Step Construction**: Build an object gradually, adding only the parts you need.
- **Customizable**: Easily create variations of the same object.

### When to Use the Builder Pattern
- When you have a complex object with many optional parts or parameters.
- When you want to construct an object step-by-step in a controlled manner.
- When you want the object-building process to be reusable and flexible.

---

## Proxy Design Pattern

The **Proxy Design Pattern** is a **structural design pattern** that provides a placeholder or substitute for another object. It allows you to control access to the original object, add extra functionality, or delay its creation until it's actually needed.

### Think of It Like This:
Imagine you’re a celebrity, and fans want to talk to you all the time. You can’t attend to everyone personally, so you hire a manager (**proxy**). The manager:
- Controls who can actually meet you (**access control**).
- Handles basic questions on your behalf (**adds functionality**).
- Lets you relax until an important fan or interviewer needs your attention (**delays interaction**).

The **manager (proxy)** acts as an intermediary, protecting and managing access to you (the real object).

### Why Use a Proxy?
- To control access to the real object.
- To enhance functionality without modifying the original object.
- To defer resource-heavy operations (e.g., creating a large object) until they’re actually needed.

### Benefits of Proxy Pattern
- **Access Control**: Restrict or manage access to sensitive resources.
- **Lazy Initialization**: Delay creation or resource-intensive operations until absolutely necessary.
- **Additional Features**: Add features like logging, caching, or validation without changing the original object.

### When to Use the Proxy Pattern
- When you want to control access to a resource (e.g., authentication).
- When you want to defer heavy resource creation until it's needed.
- When you want to add functionality transparently to an existing class without modifying it.

In short: **Proxy Pattern** is like a middleman that manages or enhances interactions with the real object!

---

## Adapter Design Pattern

The **Adapter Design Pattern** is a **structural design pattern** that allows two incompatible interfaces to work together. It acts as a bridge between two systems or classes that otherwise couldn’t interact directly because they have different interfaces.

### Think of It Like This:
Imagine you bought a new phone, but its charger has a USB-C port, while all your old cables are micro-USB. To solve this, you use a **USB-C to Micro-USB adapter**:
- The phone expects a USB-C connection.
- The old cable uses a Micro-USB connector.
- The adapter allows them to work together by converting Micro-USB to USB-C.

The **adapter** makes it possible for the old cable (incompatible) to connect to the new phone.

### Why Use the Adapter Pattern?
- To make incompatible interfaces work together without modifying them.
- To reuse existing code or classes in a new context.
- To avoid tightly coupling systems, making the code more flexible.

### Key Components
1. **Target Interface**: The interface the client expects.
2. **Adaptee**: The existing class with an incompatible interface.
3. **Adapter**: The bridge that converts the Adaptee's interface to the Target Interface.

### Example: Charging iPhone 13 with an Android Charger

#### Step 1: Define the Target Interface
The interface the iPhone expects to use:

```java
// Target interface
interface LightningPort {
    void chargeWithLightning();
}
```

#### Step 2: Existing Class (Adaptee)
The Android charger uses USB-C:

```java
// Adaptee class
class AndroidCharger {
    public void chargeWithUsbC() {
        System.out.println("Charging using USB-C...");
    }
}
```

#### Step 3: Create the Adapter
The adapter converts the USB-C interface into a Lightning interface:

```java
// Adapter class
class LightningToUsbCAdapter implements LightningPort {
    private AndroidCharger androidCharger;

    public LightningToUsbCAdapter(AndroidCharger androidCharger) {
        this.androidCharger = androidCharger;
    }

    @Override
    public void chargeWithLightning() {
        System.out.println("Adapter converts Lightning to USB-C...");
        androidCharger.chargeWithUsbC();
    }
}
```

#### Step 4: Client Code
Use the adapter to charge the iPhone with the Android charger:

```java
// Client code
public class Main {
    public static void main(String[] args) {
        // Android charger
        AndroidCharger androidCharger = new AndroidCharger();

        // Adapter to charge iPhone
        LightningPort adapter = new LightningToUsbCAdapter(androidCharger);

        // Charging iPhone 13
        System.out.println("Charging iPhone 13 with Android charger:");
        adapter.chargeWithLightning();
    }
}
```

### Output
```plaintext
Charging iPhone 13 with Android charger:
Adapter converts Lightning to USB-C...
Charging using USB-C…
```

---

## Facade Design Pattern

The **Facade Design Pattern** is a **structural design pattern** that provides a simplified interface to a complex subsystem or a group of related classes. It acts as a **front door** that hides the complexities of the system and makes it easier for clients to interact with it.

### Think of It Like This:
Imagine you’re at a restaurant:
- Instead of directly talking to the chef, the cashier, and the server, you just place your order with the **waiter** (facade).
- The waiter handles all the behind-the-scenes interactions with the kitchen and billing system and simply delivers your food.

The **waiter** provides a single, simple interface to interact with the complex operations of the restaurant.

### Key Features of the Facade Pattern
- **Simplifies Usage**: Hides the complexity of the subsystem.
- **Unified Interface**: Offers a single entry point to interact with multiple components.
- **Improves Maintainability**: Changes to subsystem components are isolated from the client.

### When to Use the Facade Pattern
- When you want to simplify the usage of a complex system.
- When you need to decouple the client from the underlying subsystem.
- When working with legacy systems, making them easier to use without changing the core.

### Benefits
- **Reduces Complexity**: Simplifies the interface for the client.
- **Decouples Client and Subsystem**: Makes the system easier to extend or modify.
- **Improves Readability**: Provides a clean and clear interface for interacting with the system.

### Code Example

#### Step 1: Subsystems (Complex Components)
These are the individual components that need to work together:

```java
// Subsystem: TV
class TV {
    public void turnOn() {
        System.out.println("Turning on the TV...");
    }
}

// Subsystem: Sound System
class SoundSystem {
    public void turnOn() {
        System.out.println("Turning on the sound system...");
