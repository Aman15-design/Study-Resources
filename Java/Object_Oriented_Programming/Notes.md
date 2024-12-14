
# Object-Oriented Programming in Java

## Classes and Objects

Suppose you have been asked by your teacher to create a list of students' names. You could easily do this using a `List` as given below:

```java
List<String> students = new ArrayList<>();
students.add("Aman");
students.add("Sharma");
```

But when she again asks you to create a variation and give her the student's roll number, name, and marks in a list, what will you do?

This is where **classes** come into play.

A **class** is a group of objects that share some common properties. A class is a collection of objects.

### Example:

```java
class Student {
    int rollNumber;
    String name;
    int marks;
}

public class Main {
    public static void main(String[] args) {
        Student aman = new Student();
        aman.rollNumber = 1;
        aman.name = "Aman";
        aman.marks = 95;
    }
}
```

An **object** is a real-world entity that has a state and behavior.

In the above example:
- `Student` is a class.
- `aman` is an object that uses that class.

---

## `new` Keyword

The `new` keyword in Java is used to create an instance of a class. In other words, it instantiates a class by allocating memory to an object and returning a reference to that memory. Memory is allocated at runtime, and it invokes the object's constructor.

### Example:

```java
Student aman = new Student();
```

---

## Constructor

A **constructor** is a special method that is used to initialize an object. It is called when an object is created.

### Example:

```java
class Student {
    int rollNumber;
    String name;
    int marks;

    // Constructor
    Student(int rollNumber, String name, int marks) {
        this.rollNumber = rollNumber;
        this.name = name;
        this.marks = marks;
    }
}

public class Main {
    public static void main(String[] args) {
        Student aman = new Student(1, "Aman", 95);
    }
}
```

---

## Constructor Overloading

When we have multiple constructors with different parameter lists, it is called **constructor overloading**.

### Example:

```java
class Student {
    int rollNumber;
    String name;
    int marks;

    // Constructor with no parameters
    Student() {
        this.rollNumber = 0;
        this.name = "Unknown";
        this.marks = 0;
    }

    // Constructor with parameters
    Student(int rollNumber, String name, int marks) {
        this.rollNumber = rollNumber;
        this.name = name;
        this.marks = marks;
    }
}

public class Main {
    public static void main(String[] args) {
        Student unknown = new Student();
        Student aman = new Student(1, "Aman", 95);
    }
}
```

**Note:**
- There is no **constructor overriding** in Java, as constructors cannot have different return types.

---

## `this` Keyword

The `this` keyword in Java is used to refer to the current instance of an object. If there is ambiguity between the instance variables and parameters, the `this` keyword resolves the problem of ambiguity.

### Example:

```java
class Student {
    int rollNumber;
    String name;

    Student(int rollNumber, String name) {
        this.rollNumber = rollNumber; // Refers to instance variable
        this.name = name; // Refers to instance variable
    }
}
```

In the above example, the parameters and instance variables have the same name. Using the `this` keyword helps distinguish between them.

---

## Wrapper Class

A **Wrapper class** in Java is a class whose object wraps or contains primitive data types. When we create an object of a wrapper class, it contains a field, and in this field, we can store primitive data types.

### Examples:

- `int` → `Integer`
- `double` → `Double`
- `boolean` → `Boolean`

### Why Use Wrapper Classes?

1. **Object-Oriented Features:** Some Java collections (e.g., `ArrayList`, `HashMap`) can only store objects, not primitives.

    ```java
    ArrayList<Integer> list = new ArrayList<>(); // Uses Integer, not int
    ```

2. **Utility Methods:** Wrapper classes provide useful methods (e.g., `Integer.parseInt()`).
3. **Nullability:** Unlike primitives, wrapper objects can be `null`.

---

## Final Keyword

The `final` keyword is used to restrict the user. It can be applied to:

1. **Variables:** A `final` variable cannot be reassigned. It becomes a constant.

    ```java
    final int x = 10;
    x = 20; // Error: Cannot assign a value to a final variable
    ```

2. **Methods:** A `final` method cannot be overridden.

    ```java
    class Parent {
        final void display() {
            System.out.println("This is a final method.");
        }
    }

    class Child extends Parent {
        void display() { // Error: Cannot override final method
            System.out.println("Cannot override.");
        }
    }
    ```

3. **Classes:** A `final` class cannot be extended.

    ```java
    final class Parent {
    }

    class Child extends Parent { // Error: Cannot inherit from final class
    }
    
