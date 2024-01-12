---
title: "Rediscovering SOLID Principles"
datePublished: Fri Jan 12 2024 13:42:36 GMT+0000 (Coordinated Universal Time)
cuid: clraoubjy000409l5cg74269e
slug: rediscovering-solid-principles
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1705065781937/5de3bb12-8bc6-4e93-8a9a-bf2ca869ab6b.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1705066887478/17dd3de9-9e67-4e55-8644-a38f92b8b165.png
tags: java, system-design, solid-principles, first-principle

---

# Introduction

Recently, I found myself training those I call ***"the tech leaders of tomorrow."*** I realized the importance of revisiting foundational concepts. As I navigated the exciting world of teaching budding tech leaders, I circled back to some old friends—the fundamental principles that lay the groundwork for sturdy software development. While steering the new leaders through the fantastic journey, I stumbled upon the enduring value of SOLID principles. It hit me that no matter how fast technology races ahead, these principles stand tall, providing valuable insights into building code that lasts.

So, why am I putting pen to paper (or fingers to keys) on this? Simple—it's personal. I wanted to document the foundational knowledge that has helped me in my career so far and hopefully to someone who might need it in the future. Whether you've been in the field for years or are just starting in coding, sometimes the most enduring lessons come from returning to basics.

# SOLID Principles

Robert C. Martin introduced the SOLID principles, often called "Uncle Bob," in the software development community. He presented these principles as guidelines to help developers create more maintainable and scalable software designs. The SOLID acronym represents five design principles:

1. Single Responsibility Principle (SRP)
    
2. Open/Closed Principle (OCP)
    
3. Liskov Substitution Principle (LSP)
    
4. Interface Segregation Principle (ISP)
    
5. Dependency Inversion Principle (DIP)
    

These principles aim to promote better software design, enhance code readability, and facilitate more manageable maintenance and expansion of software systems.

# **Single Responsibility Principle - SRP**

The Single Responsibility Principle (SRP) is a fundamental concept in software development aimed at promoting code maintainability, readability, and extensibility. The SRP states that:

"**There should never be more than one reason for your class to change.**"

In other words, a class should have a single, well-defined responsibility or purpose within a software system. It should encapsulate a single functionality or a specific aspect of the system.

To adhere to the Single Responsibility Principle, you should regularly review your classes and ensure they have a clear and single responsibility. If a class accumulates multiple duties over time, consider refactoring it into numerous smaller classes, each with its distinct purpose.

It's important to note that applying the SRP may require judgment and trade-offs. Striking the right balance between overly granular and too broad classes is essential. The key is to find a level of granularity that makes the codebase maintainable and understandable while keeping the principles of SRP in mind.

### Violation Example:

[**Employee.java**](http://Employee.java) **(Violation)**

```java
public class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    public void calculateSalary() {
        // Calculate the employee's salary based on some complex logic
        // For simplicity, we'll add a fixed bonus
        salary += 1000;
    }

    public void saveToDatabase() {
        // Save employee data to a database
        System.out.println("Saving employee to the database: " + name);
    }

    // Getters and setters for name and salary

    @Override
    public String toString() {
        return "Employee [name=" + name + ", salary=" + salary + "]";
    }
}
```

[**Main.java**](http://Employee.java) **(Violation)**

```java
public class Main {
    public static void main(String[] args) {
        // Create an employee
        Employee employee = new Employee("John Doe", 50000.0);
        // Calculate the salary and save it to the database
        employee.calculateSalary();
        employee.saveToDatabase();
        // Display the employee data
        System.out.println("Employee data: " + employee);
    }
}
```

### Adherence Example:

[**Employee.java**](http://Employee.java) **(Adherence)**

```java
public class Employee {
    private String name;
    private double salary;

    public Employee(String name, double salary) {
        this.name = name;
        this.salary = salary;
    }

    // Getters and setters for name and salary

    @Override
    public String toString() {
        return "Employee [name=" + name + ", salary=" + salary + "]";
    }
}
```

[**SalaryCalculator.java**](http://Employee.java) **(Adherence)**

```java
public class SalaryCalculator {
    public double calculateSalary(Employee employee) {
        // Calculate the employee's salary based on some complex logic
        // For simplicity, let's assume a fixed salary for this example
        return employee.getSalary() + 1000; // Add a fixed bonus
    }
}
```

[**EmployeeDatabase.java**](http://Employee.java) **(Adherence)**

```java
public class EmployeeDatabase {
    public void saveToDatabase(Employee employee) {
        // Save employee data to a database
        // For simplicity, we'll print the data for this example
        System.out.println("Saving employee to the database: " + employee);
    }
}
```

[**Main.java**](http://Employee.java) **(Adherence)**

```java
public class Main {
    public static void main(String[] args) {
        // Create an employee
        Employee employee = new Employee("John Doe", 50000.0);
        // Calculate the salary
        SalaryCalculator salaryCalculator = new SalaryCalculator();
        double calculatedSalary = salaryCalculator.calculateSalary(employee);
        // Save employee data to the database
        EmployeeDatabase employeeDatabase = new EmployeeDatabase();
        employeeDatabase.saveToDatabase(employee);
        // Display the calculated salary
        System.out.println("Calculated Salary: $" + calculatedSalary);
    }
}
```

# **Open Closed Principle - OCP**

The Open-Closed Principle (OCP) states:

"**Software entities (classes, modules, functions, etc.) should be open for extension but closed for modification.**"

i.e., after a class has been implemented and is functioning as intended, it should not undergo any further modifications. Instead, you should be able to extend its behavior through Inheritance or composition without altering the existing Code. This principle promotes code stability and minimizes the risk of introducing bugs when making changes.

Using Java, let's illustrate the Open-Closed Principle with examples of violation and adherence.

### Violation of the Open-Closed Principle

In a violation example, we'll create a basic system that calculates the total price of products, and later, we'll see how it can lead to issues when adding new types of products.

// Violation Example

```java
public class Product {
    private String name;
    private double price;

    public Product(String name, double price) {
        this.name = name;
        this.price = price;
    }

    public double getPrice() {
        return price;
    }

}

public class ShoppingCart {
    private List<Product> items = new ArrayList<>();

    public void addProduct(Product product) {
        items.add(product);
    }

    public double calculateTotal() {
        double total = 0;
        for (Product product : items) {
            total += product.getPrice();
        }
        return total;
    }
}
```

// Client code

```java
public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        cart.addProduct(new Product("Item 1", 10.0));
        cart.addProduct(new Product("Item 2", 5.0));
        System.out.println("Total Price: $" + cart.calculateTotal());
    }
}
```

In this example, the Product and ShoppingCart classes are tightly coupled. If you want to add new types of products (e.g., discounts or promotions), you would need to modify the ShoppingCart class, violating the Open-Closed Principle.

### Adherence to the Open-Closed Principle

To adhere to the Open-Closed Principle, we can introduce an abstract class or interface for products and use polymorphism to handle different product types without modifying existing Code.

// Adherence Example

```java
public interface Product {
    double getPrice();
}

public class BasicProduct implements Product {
    private String name;
    private double price;

    public BasicProduct(String name, double price) {
        this.name = name;
        this.price = price;
    }

    @Override
    public double getPrice() {
        return price;
    }

}

public class DiscountedProduct implements Product {
    private String name;
    private double price;
    private double discount;

    public DiscountedProduct(String name, double price, double discount) {
        this.name = name;
        this.price = price;
        this.discount = discount;
    }

    @Override

    public double getPrice() {
        return price - (price * discount);
    }

}

public class ShoppingCart {
    private List<Product> items = new ArrayList<>();

    public void addProduct(Product product) {
        items.add(product);

    }

    public double calculateTotal() {
        double total = 0;
        for (Product product : items) {
            total += product.getPrice();
        }
        return total;

    }

}


// Client code

public class Main {
    public static void main(String[] args) {
        ShoppingCart cart = new ShoppingCart();
        cart.addProduct(new BasicProduct("Item 1", 10.0));
        cart.addProduct(new DiscountedProduct("Item 2", 5.0, 0.2));
        System.out.println("Total Price: $" + cart.calculateTotal());
    }
}
```

In this example, we've adhered to the Open-Closed Principle by introducing the Product interface and creating two concrete implementations (BasicProduct and DiscountedProduct). We can now add new types of products without modifying the ShoppingCart class, making the Code open for extension but closed for modification.

# Using Inheritance

Let's say we have a simple application that calculates the area of different shapes. Initially, we have two shapes: rectangles and circles. Later, we want to add support for calculating the area of triangles without modifying the existing Code.

### Initial Implementation (Violation of OCP)

We have a Shape class with subclasses for Rectangle and Circle in the initial implementation.

```java
public class Shape {
    // Common properties or methods for all shapes
}

public class Rectangle extends Shape {
    private double width;
    private double height;
    // Constructor and methods for calculating area
}

public class Circle extends Shape {
    private double radius;
    // Constructor and methods for calculating area
}

public class AreaCalculator {
    public double calculateArea(Shape shape) {
        if (shape instanceof Rectangle) {
            Rectangle rectangle = (Rectangle) shape;
            return rectangle.getWidth() * rectangle.getHeight();
        } else if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI  circle.getRadius()  circle.getRadius();
        } else {
            throw new IllegalArgumentException("Unsupported shape type");
        }
    }
}
```

In this Code, if we want to add support for triangles, we need to modify the AreaCalculator class, which violates the Open-Closed Principle.

### Adherence to the Open-Closed Principle using Inheritance

We can use Inheritance to create a more extensible design to adhere to the Open-Closed Principle. We'll introduce a new abstract class, Shape, with concrete subclasses for Rectangles, Circles, and Triangles. This way, we can add new shapes without modifying existing Code.

```java
public abstract class Shape {
    // Common properties or methods for all shapes
    public abstract double calculateArea();
}

public class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return width * height;
    }
}

public class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI  radius  radius;
    }
}

public class Triangle extends Shape {
    private double base;
    private double height;

    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return 0.5  base  height;
    }
}
```

With this design, we've extended the application to include the Triangle class without modifying the AreaCalculator class. Now, you can create instances of Triangle and calculate their areas without changing existing codes.

Here's how you can use these classes:

```java
public class Main {
    public static void main(String[] args) {
        Shape rectangle = new Rectangle(5, 4);
        Shape circle = new Circle(3);
        Shape triangle = new Triangle(6, 8);
        AreaCalculator calculator = new AreaCalculator();

        System.out.println("Rectangle Area: " + calculator.calculateArea(rectangle));
        System.out.println("Circle Area: " + calculator.calculateArea(circle));
        System.out.println("Triangle Area: " + calculator.calculateArea(triangle));
    }
}
```

This adherence to the Open-Closed Principle using Inheritance allows us to easily add new shapes (e.g., Triangle) without modifying existing Code, making the code open for extension but closed for modification.

# **Liskov Substitution Principle - LSP**

The Liskov Substitution Principle says that you should be able to use a subclass object wherever you use a superclass object without causing problems in your program. So, if you have a class S that is a subclass of class T, you should be able to replace an object of class T with an object of class S without changing how the program works.

**Violation of Liskov Substitution Principle:**

Here's a class hierarchy where the Liskov Substitution Principle is violated:

```java
class Shape {
    public double getArea() {
        return 0;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double getArea() {
        return width * height;
    }
}

class Circle extends Shape {
    private double radius;

    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI  radius  radius;
    }
}
```

In this example, we have a base class Shape with a getArea method. The Rectangle and Circle classes override this method to calculate their respective areas. However, this design violates the Liskov Substitution Principle because it assumes all shapes can have an area. For shapes like circles, this works fine, but for shapes like a line or a point, it doesn't make sense.

**Adherence to the Liskov Substitution Principle:**

To adhere to the Liskov Substitution Principle, we should have a more general and flexible design:

```java
abstract class Shape {
    public abstract double getArea();
}

class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public double getArea() {
        return width * height;
    }
}

class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI  radius  radius;
    }
}

class Line extends Shape {
    private double length;

    public Line(double length) {
        this.length = length;
    }

    @Override
    public double getArea() {
        return 0; // A line has no area
    }
}
```

In this improved design, we've made Shape an abstract class with an abstract getArea method. This allows us to create shapes like Lines that have no area. Now, we adhere to the Liskov Substitution Principle because we can use any shape interchangeably, and the behavior is consistent with the shape's nature.

**How to Run the Example:**

Here's a simple program to test the adherence to the Liskov Substitution Principle:

```java
public class Main {
    public static void main(String[] args) {
        Shape rectangle = new Rectangle(5, 10);
        Shape circle = new Circle(7);
        Shape line = new Line(15);

        System.out.println("Rectangle Area: " + rectangle.getArea());
        System.out.println("Circle Area: " + circle.getArea());
        System.out.println("Line Area: " + line.getArea());
    }
}
```

When you run this program, you'll see that each shape behaves appropriately, with no unexpected exceptions or issues. The Line shape, in particular, returns an area of 0, consistent with its nature as a non-dimensional shape, adhering to the Liskov Substitution Principle.

# **Interface Segregation Principle - ISP**

The Interface Segregation Principle (ISP) is one of the SOLID principles of object-oriented design. It states:

"**A client should not be forced to depend on interfaces it does not use.**"

In essence, this principle emphasizes that interfaces should be specific to the needs of the clients (classes that implement them). Clients should not be burdened with methods they don't need or use. Violating the ISP can lead to overly large and complex interfaces that force clients to implement methods they don't care about, resulting in tight coupling and Code that is difficult to maintain and understand.

Let's illustrate the ISP with both a violation example and an adherence example in Java:

### Violation Example:

Suppose we have an interface called Worker that represents a worker's actions, and it contains methods for both working and eating:

```java
public interface Worker {
    void work();
    void eat();
}
```

Now, let's say we have two classes, Engineer and Waiter, that implement this interface:

```java
public class Engineer implements Worker {
    @Override
    public void work() {
        // Engineer-specific work implementation
    }

    @Override
    public void eat() {
        // Engineer-specific eat implementation
    }
}

public class Waiter implements Worker {
    @Override
    public void work() {
        // Waiter-specific work implementation
    }

    @Override
    public void eat() {
        // Waiter-specific eat implementation
    }
}
```

In this example, the Engineer and Waiter must implement the eat() method even though it's irrelevant to their roles. This violates the ISP because clients (in this case, Engineer and Waiter) must depend on methods they don't need.

### Adherence Example:

To align with the Interface Segregation Principle, we can divide the Worker interface into more specialized interfaces:

// Separate interfaces for working and eating

```java
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}
```

Now, we can ensure that the Engineer and Waiter classes implement only the interfaces pertinent to their respective roles:

```java
public class Engineer implements Workable {
    @Override
    public void work() {
        // Engineer-specific work implementation
    }
}

public class Waiter implements Workable, Eatable {
    @Override
    public void work() {
        // Waiter-specific work implementation
    }

    @Override
    public void eat() {
        // Waiter-specific eat implementation
    }
}
```

By adhering to the ISP and providing more specific interfaces, we ensure that each class only depends on the methods it needs. This results in more flexible and maintainable Code, as clients are not burdened with unnecessary methods from the interface.

## Interface Pollution

Interface pollution is a term used in software development to describe a situation where an interface (in the context of programming interfaces, not user interfaces) becomes overly complex or bloated with too many methods, many of which may not be relevant to all classes that implement the interface. This concept is closely related to the Interface Segregation Principle (ISP) from the SOLID principles, which emphasizes that interfaces should be specific to the needs of the clients (classes that implement them).

## Signs of Interface Pollution

* Classes have empty method implementations
    
* Method Implementations throw UnsupportedOperationException or Similar
    
* Method Implementations return null or default/dummy values
    

# **Dependency Inversion Principle - DIP**

The Dependency Inversion Principle (DIP) is one of the SOLID principles in object-oriented design, and it focuses on decoupling high-level modules from low-level modules, promoting more flexible and maintainable Code. The principle consists of two key points:

1. **High-level modules should be independent of low-level modules. Both should depend on abstractions.**
    
2. **Abstractions should be independent of details. Details should depend on abstractions.**
    

Using Java, let's illustrate the Dependency Inversion Principle with both a violation and an adherence example.

### Dependency Inversion Principle Violation Example:

Consider a simple example of a newsletter system without adhering to DIP:

```java
// Low-level module
class Email {
    public void sendEmail(String message) {
        //Code to send an email
    }
}

// High-level module
class NewsletterService {
    private Email email;

    public NewsletterService() {
        this.email = new Email(); // Dependency on a concrete class
    }

    public void sendNewsletter(String content) {
        // Compose the newsletter content
        String newsletterContent = "Newsletter: " + content;
        // Send the Email
        email.sendEmail(newsletterContent); // High-level module depends on a low-level module
    }
}
```

In this instance, the NewsletterService serves as a high-level module with a direct dependency on the Email class, a low-level module. This contradicts the Dependency Inversion Principle, as high-level modules ideally should not directly depend on low-level modules. Such tight coupling compromises code flexibility and increases the difficulty of maintenance.

### Dependency Inversion Principle Adherence Example:

To comply with the Dependency Inversion Principle, we introduce an abstraction, which can be either an interface or an abstract class. Both the high-level and low-level modules then depend on this abstraction:

```java
//MessageSender (interface)
interface MessageSender {
    void sendMessage(String message);
}

// Low-level module
class Email implements MessageSender {
    @Override
    public void sendMessage(String message) {
        //Code to send an email
    }
}

// High-level module

class NewsletterService {
    private MessageSender sender;

    public NewsletterService(MessageSender sender) {
        this.sender = sender; // Dependency on an abstraction
    }

    public void sendNewsletter(String content) {
        // Compose the newsletter content
        String newsletterContent = "Newsletter: " + content;

        // Send the message (via the Abstraction)
        sender.sendMessage(newsletterContent); // High-level module depends on an abstraction
    }
}
```

In this refactored Code, we introduced the MessageSender interface as an abstraction. This Abstraction depends on the Email class (low-level module) and the NewsletterService class (high-level module). This adheres to the Dependency Inversion Principle because:

1. **High-level modules (NewsletterService) are directly independent of low-level modules (Email). They depend on the common Abstraction (MessageSender).**
    
2. **Abstractions (MessageSender) do not depend on details (concrete implementations like Email). Instead, details depend on abstractions.**
    

Now, the Code is more flexible, and you can easily extend it by adding new implementations of the MessageSender interface without modifying the NewsletterService class, demonstrating adherence to the Dependency Inversion Principle.

```java
public class DependencyInversionTest {
    public static void main(String[] args) {
        // Create an instance of the low-level module (Email)
        MessageSender emailSender = new Email();
        // Create an instance of the high-level module (NewsletterService) using the abstraction
        NewsletterService newsletterService = new NewsletterService(emailSender);
        // Send a newsletter
        newsletterService.sendNewsletter("This is a test newsletter.");
    }
}
```

# Conclusion

[James Clear once said, "Understanding the first principles of your field is a smart use of your time."](https://jamesclear.com/first-principles) SOLID principles aren't just coding guidelines; they're part of the backbone and foundation of resilient software. Going back to these basics isn't a nostalgic gesture but a strategic decision to fortify your Code against the challenges of time.