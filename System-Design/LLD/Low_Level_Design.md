
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
