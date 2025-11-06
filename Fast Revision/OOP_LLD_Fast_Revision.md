# OOP & LLD Fast Revision 🏗️

## Why OOP + LLD is High Priority 🎯

**Interviewers love asking OOP questions these days. They want to see whether you can write object-oriented code really well or not. Structuring real world problems into code is something that all companies love to ask.**

### **Why This Round is Critical**:
- **Tests ability to structure real-world problems**
- **Essential for machine coding/LLD rounds**
- **Shows practical programming skills**
- **Demonstrates design thinking**

### **Common Interview Formats**:
- **Write C++/Java code with OOP concepts embedded**
- **Real-world scenario questions** (design parking lot)
- **Object-oriented modeling** of problems
- **System design with OOP principles**

---

## Core OOP Concepts 📋

### **1. Encapsulation** 📦

#### **Definition**:
**Bundling data (attributes) and methods (functions) that operate on the data into a single unit (class). Hiding internal details and exposing only necessary interfaces.**

#### **Example**:
```java
public class BankAccount {
    // Private data - hidden from outside
    private double balance;
    private String accountNumber;

    // Public interface - controlled access
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }

    public double getBalance() {
        return balance; // Read-only access
    }
}
```

#### **Interview Questions**:
- **Why use private variables?** (Data hiding, validation)
- **Difference between encapsulation and abstraction?**
- **How does encapsulation improve maintainability?**

### **2. Inheritance** 🧬

#### **Definition**:
**Mechanism where a new class derives properties and behaviors from an existing class. Promotes code reusability and establishes "is-a" relationships.**

#### **Types of Inheritance**:
```java
// Single Inheritance
class Vehicle {
    protected String brand;
    protected int year;

    public void start() {
        System.out.println("Vehicle starting");
    }
}

class Car extends Vehicle {
    private int numberOfDoors;

    public void honk() {
        System.out.println("Car honking");
    }
}

// Multilevel Inheritance
class SportsCar extends Car {
    private boolean turboEnabled;

    public void enableTurbo() {
        turboEnabled = true;
        System.out.println("Turbo enabled!");
    }
}

// Interface (Multiple Inheritance)
interface Electric {
    void charge();
    int getBatteryLevel();
}

interface Autonomous {
    void enableSelfDriving();
}

class Tesla extends Car implements Electric, Autonomous {
    private int batteryLevel;
    private boolean selfDrivingMode;

    public void charge() {
        batteryLevel = 100;
        System.out.println("Tesla charging");
    }

    public int getBatteryLevel() {
        return batteryLevel;
    }

    public void enableSelfDriving() {
        selfDrivingMode = true;
        System.out.println("Self-driving enabled");
    }
}
```

#### **Key Interview Points**:
- **"is-a" relationship** (Car is a Vehicle)
- **Code reusability** and maintainability
- **Method overriding** vs **method hiding**
- **Super keyword usage**
- **Constructor chaining**

### **3. Polymorphism** 🔄

#### **Definition**:
**Ability of objects to take different forms. Same interface, different implementations. Two types: Compile-time (method overloading) and Runtime (method overriding).**

#### **Compile-time Polymorphism (Method Overloading)**:
```java
class Calculator {
    // Method overloading - same name, different parameters
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }

    public int add(int a, int b, int c) {
        return a + b + c;
    }

    // Varargs example
    public int add(int... numbers) {
        int sum = 0;
        for (int num : numbers) {
            sum += num;
        }
        return sum;
    }
}
```

#### **Runtime Polymorphism (Method Overriding)**:
```java
abstract class Animal {
    abstract void makeSound();

    public void sleep() {
        System.out.println("Animal is sleeping");
    }
}

class Dog extends Animal {
    @Override
    void makeSound() {
        System.out.println("Woof! Woof!");
    }

    @Override
    public void sleep() {
        System.out.println("Dog is sleeping in its bed");
    }
}

class Cat extends Animal {
    @Override
    void makeSound() {
        System.out.println("Meow!");
    }
}

// Runtime polymorphism in action
public class PolymorphismDemo {
    public static void main(String[] args) {
        Animal myDog = new Dog(); // Upcasting
        Animal myCat = new Cat();

        myDog.makeSound(); // Outputs: Woof! Woof!
        myCat.makeSound(); // Outputs: Meow!

        // Dynamic method dispatch
        Animal[] animals = {new Dog(), new Cat(), new Dog()};
        for (Animal animal : animals) {
            animal.makeSound(); // Calls appropriate method at runtime
        }
    }
}
```

#### **Interview Questions**:
- **Difference between overloading and overriding?**
- **When to use which type of polymorphism?**
- **How does JVM achieve runtime polymorphism?**
- **Can we override static methods?** (No!)

### **4. Abstraction** 🎭

#### **Definition**:
**Hiding complex implementation details while showing only essential features. Can be achieved through abstract classes and interfaces.**

#### **Abstract Class Example**:
```java
abstract class Shape {
    protected String color;

    public Shape(String color) {
        this.color = color;
    }

    // Abstract method - must be implemented by subclasses
    public abstract double calculateArea();

    // Concrete method - can be used as-is or overridden
    public void displayColor() {
        System.out.println("Shape color: " + color);
    }
}

class Circle extends Shape {
    private double radius;

    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }

    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

class Rectangle extends Shape {
    private double width;
    private double height;

    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }

    @Override
    public double calculateArea() {
        return width * height;
    }
}
```

#### **Interface Example**:
```java
interface Flyable {
    void fly();
    default void takeOff() {
        System.out.println("Taking off...");
    }
}

interface Swimmable {
    void swim();
}

class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }

    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

---

## SOLID Principles 🏛️

### **1. Single Responsibility Principle (SRP)**

#### **Definition**:
**A class should have only one reason to change. Each class should have a single responsibility.**

#### **Before SRP (Violating)**:
```java
class Employee {
    private String name;
    private String email;
    private double salary;

    // Employee data management
    public void saveEmployee() { /* DB operations */ }
    public void calculateTax() { /* Tax calculations */ }
    public void generateReport() { /* Report generation */ }
    public void sendEmail() { /* Email operations */ }
}
```

#### **After SRP (Correct)**:
```java
class Employee {
    private String name;
    private String email;
    private double salary;

    // Only employee data
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
    // Getters and setters only
}

class EmployeeRepository {
    public void saveEmployee(Employee employee) { /* DB operations */ }
    public Employee getEmployee(int id) { /* DB retrieval */ }
}

class TaxCalculator {
    public double calculateTax(Employee employee) { /* Tax calculations */ }
}

class ReportGenerator {
    public void generateEmployeeReport(Employee employee) { /* Report generation */ }
}

class EmailService {
    public void sendEmail(String to, String subject, String body) { /* Email */ }
}
```

### **2. Open/Closed Principle (OCP)**

#### **Definition**:
**Software entities should be open for extension but closed for modification.**

#### **Example**:
```java
interface DiscountCalculator {
    double calculateDiscount(double amount);
}

class RegularCustomerDiscount implements DiscountCalculator {
    @Override
    public double calculateDiscount(double amount) {
        return amount * 0.05; // 5% discount
    }
}

class PremiumCustomerDiscount implements DiscountCalculator {
    @Override
    public double calculateDiscount(double amount) {
        return amount * 0.15; // 15% discount
    }
}

class VIPCustomerDiscount implements DiscountCalculator {
    @Override
    public double calculateDiscount(double amount) {
        return amount * 0.25; // 25% discount
    }
}

// No need to modify this class when adding new discount types
class OrderProcessor {
    private DiscountCalculator discountCalculator;

    public OrderProcessor(DiscountCalculator discountCalculator) {
        this.discountCalculator = discountCalculator;
    }

    public double processOrder(double amount) {
        double discount = discountCalculator.calculateDiscount(amount);
        return amount - discount;
    }
}
```

### **3. Liskov Substitution Principle (LSP)**

#### **Definition**:
**Objects of a superclass should be replaceable with objects of its subclasses without breaking the application.**

#### **Correct Implementation**:
```java
class Rectangle {
    protected int width;
    protected int height;

    public void setWidth(int width) {
        this.width = width;
    }

    public void setHeight(int height) {
        this.height = height;
    }

    public int getWidth() {
        return width;
    }

    public int getHeight() {
        return height;
    }

    public int getArea() {
        return width * height;
    }
}

// Square IS-A Rectangle but violates LSP if behavior changes
class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width; // Square maintains equal sides
    }

    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height; // Square maintains equal sides
    }
}

// Better approach - use common interface
interface Shape {
    int getArea();
}

class Rectangle2 implements Shape {
    private int width, height;

    public Rectangle2(int width, int height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public int getArea() {
        return width * height;
    }
}

class Square2 implements Shape {
    private int side;

    public Square2(int side) {
        this.side = side;
    }

    @Override
    public int getArea() {
        return side * side;
    }
}
```

### **4. Interface Segregation Principle (ISP)**

#### **Definition**:
**Clients should not be forced to depend on interfaces they don't use.**

#### **Before ISP (Fat Interface)**:
```java
interface Worker {
    void work();
    void eat();
    void sleep();
    void attendMeeting();
}

class Robot implements Worker {
    @Override
    public void work() { /* Robot can work */ }

    @Override
    public void eat() { /* Robot doesn't eat! */ }

    @Override
    public void sleep() { /* Robot doesn't sleep! */ }

    @Override
    public void attendMeeting() { /* Robot can attend */ }
}
```

#### **After ISP (Segregated Interfaces)**:
```java
interface Workable {
    void work();
}

interface Eatable {
    void eat();
}

interface Sleepable {
    void sleep();
}

interface Attendee {
    void attendMeeting();
}

class Human implements Workable, Eatable, Sleepable, Attendee {
    @Override
    public void work() { /* Human can work */ }

    @Override
    public void eat() { /* Human can eat */ }

    @Override
    public void sleep() { /* Human can sleep */ }

    @Override
    public void attendMeeting() { /* Human can attend */ }
}

class Robot implements Workable, Attendee {
    @Override
    public void work() { /* Robot can work */ }

    @Override
    public void attendMeeting() { /* Robot can attend */ }
}
```

### **5. Dependency Inversion Principle (DIP)**

#### **Definition**:
**High-level modules should not depend on low-level modules. Both should depend on abstractions.**

#### **Before DIP**:
```java
class LightBulb {
    public void turnOn() {
        System.out.println("LightBulb turned on");
    }

    public void turnOff() {
        System.out.println("LightBulb turned off");
    }
}

class Switch {
    private LightBulb bulb;

    public Switch() {
        this.bulb = new LightBulb(); // Direct dependency
    }

    public void flip() {
        bulb.turnOn();
    }
}
```

#### **After DIP**:
```java
interface Switchable {
    void turnOn();
    void turnOff();
}

class LightBulb implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("LightBulb turned on");
    }

    @Override
    public void turnOff() {
        System.out.println("LightBulb turned off");
    }
}

class Fan implements Switchable {
    @Override
    public void turnOn() {
        System.out.println("Fan turned on");
    }

    @Override
    public void turnOff() {
        System.out.println("Fan turned off");
    }
}

class Switch {
    private Switchable device;

    public Switch(Switchable device) {
        this.device = device; // Dependency injection
    }

    public void flip() {
        device.turnOn();
    }
}
```

---

## Low-Level Design (LLD) & System Design 🎯

### **Common LLD Problems** 🏗️

#### **1. Parking Lot System** 🚗

**Requirements**:
- **Multiple vehicle types** (Car, Motorcycle, Truck)
- **Different parking spot sizes**
- **Payment system**
- **Entry/Exit gates**

**Class Design**:
```java
// Vehicle hierarchy
abstract class Vehicle {
    protected String licensePlate;
    protected VehicleType type;

    public abstract VehicleType getType();
}

class Car extends Vehicle {
    public Car(String licensePlate) {
        this.licensePlate = licensePlate;
        this.type = VehicleType.CAR;
    }

    @Override
    public VehicleType getType() {
        return VehicleType.CAR;
    }
}

class Motorcycle extends Vehicle {
    public Motorcycle(String licensePlate) {
        this.licensePlate = licensePlate;
        this.type = VehicleType.MOTORCYCLE;
    }

    @Override
    public VehicleType getType() {
        return VehicleType.MOTORCYCLE;
    }
}

// Parking spot
class ParkingSpot {
    private int spotNumber;
    private VehicleType spotType;
    private Vehicle parkedVehicle;
    private boolean isOccupied;

    public boolean isAvailable() {
        return !isOccupied;
    }

    public boolean parkVehicle(Vehicle vehicle) {
        if (!isOccupied && vehicle.getType() == spotType) {
            this.parkedVehicle = vehicle;
            this.isOccupied = true;
            return true;
        }
        return false;
    }

    public void removeVehicle() {
        this.parkedVehicle = null;
        this.isOccupied = false;
    }
}

// Parking lot
class ParkingLot {
    private Map<Integer, ParkingSpot> carSpots;
    private Map<Integer, ParkingSpot> motorcycleSpots;
    private Map<Integer, ParkingSpot> truckSpots;
    private EntryGate entryGate;
    private ExitGate exitGate;

    public ParkingTicket parkVehicle(Vehicle vehicle) {
        ParkingSpot spot = findAvailableSpot(vehicle.getType());
        if (spot != null) {
            spot.parkVehicle(vehicle);
            return entryGate.generateTicket(vehicle, spot);
        }
        return null;
    }

    public double exitVehicle(ParkingTicket ticket) {
        exitGate.processPayment(ticket);
        ParkingSpot spot = findSpotByNumber(ticket.getSpotNumber());
        spot.removeVehicle();
        return ticket.getAmount();
    }
}
```

#### **2. Elevator System** 🛗

**Requirements**:
- **Multiple elevators**
- **Floor requests**
- **Direction handling**
- **Priority scheduling**

**Class Design**:
```java
enum Direction {
    UP, DOWN, IDLE
}

class Request {
    private int floor;
    private Direction direction;
    private long timestamp;

    public Request(int floor, Direction direction) {
        this.floor = floor;
        this.direction = direction;
        this.timestamp = System.currentTimeMillis();
    }
}

class Elevator {
    private int id;
    private int currentFloor;
    private Direction currentDirection;
    private List<Integer> destinationFloors;
    private ElevatorState state;

    public void addRequest(int floor) {
        if (floor > currentFloor && currentDirection == Direction.UP) {
            destinationFloors.add(floor);
        } else if (floor < currentFloor && currentDirection == Direction.DOWN) {
            destinationFloors.add(floor);
        }
        sortDestinations();
    }

    public void move() {
        if (!destinationFloors.isEmpty()) {
            int nextFloor = destinationFloors.get(0);
            if (nextFloor > currentFloor) {
                currentDirection = Direction.UP;
                currentFloor++;
            } else if (nextFloor < currentFloor) {
                currentDirection = Direction.DOWN;
                currentFloor--;
            }

            if (currentFloor == nextFloor) {
                destinationFloors.remove(0);
                openDoors();
            }
        }
    }
}

class ElevatorController {
    private List<Elevator> elevators;
    private List<Request> pendingRequests;

    public void assignRequest(Request request) {
        Elevator bestElevator = findBestElevator(request);
        if (bestElevator != null) {
            bestElevator.addRequest(request.getFloor());
        } else {
            pendingRequests.add(request);
        }
    }

    private Elevator findBestElevator(Request request) {
        // Find elevator with minimum cost
        return elevators.stream()
                .min(Comparator.comparing(e -> calculateCost(e, request)))
                .orElse(null);
    }
}
```

#### **3. Book My Show (Seat Booking) 🎭

**Requirements**:
- **Multiple theaters/shows**
- **Seat selection and booking**
- **Payment processing**
- **Seat locking mechanism**

**Class Design**:
```java
class Seat {
    private String seatId;
    private SeatType type;
    private SeatStatus status;
    private User lockedBy;
    private LocalDateTime lockTime;

    public boolean lockSeat(User user) {
        if (status == SeatStatus.AVAILABLE) {
            this.status = SeatStatus.LOCKED;
            this.lockedBy = user;
            this.lockTime = LocalDateTime.now();
            return true;
        }
        return false;
    }

    public boolean bookSeat() {
        if (status == SeatStatus.LOCKED && !isLockExpired()) {
            this.status = SeatStatus.BOOKED;
            return true;
        }
        return false;
    }

    private boolean isLockExpired() {
        return lockTime.plusMinutes(10).isBefore(LocalDateTime.now());
    }
}

class Show {
    private String showId;
    private Movie movie;
    private Theater theater;
    private LocalDateTime showTime;
    private Map<String, Seat> seats;

    public List<Seat> getAvailableSeats() {
        return seats.values().stream()
                .filter(seat -> seat.getStatus() == SeatStatus.AVAILABLE)
                .collect(Collectors.toList());
    }
}

class BookingService {
    private Map<String, Show> shows;
    private PaymentService paymentService;

    public Booking lockSeats(String showId, List<String> seatIds, User user) {
        Show show = shows.get(showId);
        List<Seat> lockedSeats = new ArrayList<>();

        for (String seatId : seatIds) {
            Seat seat = show.getSeat(seatId);
            if (seat.lockSeat(user)) {
                lockedSeats.add(seat);
            } else {
                // Rollback already locked seats
                lockedSeats.forEach(s -> s.releaseLock());
                return null;
            }
        }

        return new Booking(show, lockedSeats, user);
    }

    public boolean confirmBooking(Booking booking, Payment payment) {
        if (paymentService.processPayment(payment)) {
            booking.getSeats().forEach(Seat::bookSeat);
            return true;
        }
        return false;
    }
}
```

---

## Design Patterns 🎨

### **1. Singleton Pattern**

#### **When to Use**:
- **Database connections**
- **Logger instances**
- **Configuration managers**

```java
public class DatabaseConnection {
    private static DatabaseConnection instance;
    private Connection connection;

    private DatabaseConnection() {
        // Private constructor
    }

    public static synchronized DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }

    public Connection getConnection() {
        if (connection == null) {
            // Initialize connection
        }
        return connection;
    }
}
```

### **2. Factory Pattern** 🏭

#### **When to Use**:
- **Object creation with complex logic**
- **Depending on configuration**
- **Runtime type determination**

```java
interface Vehicle {
    void drive();
}

class Car implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a car");
    }
}

class Motorcycle implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Riding a motorcycle");
    }
}

class VehicleFactory {
    public static Vehicle createVehicle(String type) {
        switch (type.toLowerCase()) {
            case "car":
                return new Car();
            case "motorcycle":
                return new Motorcycle();
            default:
                throw new IllegalArgumentException("Unknown vehicle type");
        }
    }
}
```

### **3. Observer Pattern** 👁️

#### **When to Use**:
- **Event handling systems**
- **UI updates**
- **Notification systems**

```java
interface Observer {
    void update(String message);
}

interface Subject {
    void addObserver(Observer observer);
    void removeObserver(Observer observer);
    void notifyObservers(String message);
}

class NewsAgency implements Subject {
    private List<Observer> observers = new ArrayList<>();

    @Override
    public void addObserver(Observer observer) {
        observers.add(observer);
    }

    @Override
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }

    @Override
    public void notifyObservers(String message) {
        for (Observer observer : observers) {
            observer.update(message);
        }
    }

    public void publishNews(String news) {
        System.out.println("Publishing: " + news);
        notifyObservers(news);
    }
}

class NewsChannel implements Observer {
    private String channelName;

    public NewsChannel(String channelName) {
        this.channelName = channelName;
    }

    @Override
    public void update(String message) {
        System.out.println(channelName + " reporting: " + message);
    }
}
```

---

## Interview Preparation Checklist ✅

### **Must-Know OOP Concepts**:
- [ ] **Four pillars**: Encapsulation, Inheritance, Polymorphism, Abstraction
- [ ] **Compile-time vs Runtime polymorphism**
- [ ] **Method overloading vs overriding**
- [ ] **Abstract classes vs interfaces**
- [ ] **Constructor chaining**
- [ ] **Super and this keywords**
- [ ] **Access modifiers** (public, private, protected, default)

### **SOLID Principles**:
- [ ] **SRP**: Single Responsibility
- [ ] **OCP**: Open/Closed
- [ ] **LSP**: Liskov Substitution
- [ ] **ISP**: Interface Segregation
- [ ] **DIP**: Dependency Inversion

### **Design Patterns**:
- [ ] **Singleton**, **Factory**, **Observer**
- [ ] **Strategy**, **Adapter**, **Decorator**
- [ ] **Builder**, **Prototype**

### **LLD Problems to Practice**:
- [ ] **Parking Lot System**
- [ ] **Elevator System**
- [ ] **Book My Show (Seat Booking)**
- [ ] **ATM System**
- [ ] **Hospital Management**
- [ ] **Library Management**
- [ ] **Social Media System**

### **Implementation Skills**:
- [ ] **Class diagram design**
- [ ] **Relationship modeling** (association, aggregation, composition)
- [ ] **Interface design**
- [ ] **Exception handling**
- [ ] **Input validation**
- [ ] **Thread safety considerations**

---

## Key Takeaways 💡

1. **Practice Implementation**: Cannot write OOP code directly in interviews
2. **Understand SOLID**: Shows deep understanding of design principles
3. **Master Common Problems**: Parking lot, elevator, booking systems
4. **Know Design Patterns**: Demonstrates design maturity
5. **Think Real-World**: Structure problems into object-oriented solutions
6. **Focus on Relationships**: Association, aggregation, composition
7. **Consider Edge Cases**: Error handling, validation, constraints
8. **Explain Your Design**: Justify your design decisions

**Remember**: LLD tests your ability to structure real-world problems into clean, maintainable code. Practice designing systems before the interview! 🎯

---

**Next Steps**: Practice implementing these designs and move on to Operating Systems concepts for comprehensive interview preparation.