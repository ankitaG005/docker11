/*Q1. You are given an AudioPlayer class that only plays MP3 files. Extend its functionality to
support playing both MP4 and VLC files by using the Adapter Pattern.*/

package Audio.com;

interface MediaPlayer {
    void play(String audioType, String fileName);
}

class AudioPlayer implements MediaPlayer {

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            // Play MP3 file
        } else {
            System.out.println("Unsupported audio type");
        }
    }
}

class MP4Player implements MediaPlayer {

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp4")) {
            // Play MP4 file
        } else {
            System.out.println("Unsupported audio type");
        }
    }
}

class VLCPlayer implements MediaPlayer {

    @Override
    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            // Play VLC file
        } else {
            System.out.println("Unsupported audio type");
        }
    }
}

class MediaAdapter implements MediaPlayer {

    private MediaPlayer mediaPlayer;

    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("mp4")) {
            mediaPlayer = new MP4Player();
        } else if (audioType.equalsIgnoreCase("vlc")) {
            mediaPlayer = new VLCPlayer();
        }
    }

    @Override
    public void play(String audioType, String fileName) {
        mediaPlayer.play(audioType, fileName);
    }
}

public class AdapterPatternDemo {

    public static void main(String[] args) {
        AudioPlayer audioPlayer = new AudioPlayer();

        // Play MP3 file
        audioPlayer.play("mp3", "beyond_the_horizon.mp3");

        // Play MP4 file using MediaAdapter
        audioPlayer.play("mp4", "memories.mp4");

        // Play VLC file using MediaAdapter
        audioPlayer.play("vlc", "nature.vlc");
    }
}


/*Q2. Implement a simple system where you have Employee objects. An employee can either be
an individual worker or a manager who supervises a team of employees. Use the Composite
Pattern to treat both individual employees and managers uniformly in terms of their reporting
structure.*/

package Employee.com;
import java.util.*;

interface Employee {
    void report();
}

class IndividualEmployee implements Employee {
    private String name;

    public IndividualEmployee(String name) {
        this.name = name;
    }

    @Override
    public void report() {
        System.out.println("Individual Employee: " + name);
    }
}

class Manager implements Employee {
    private String name;
    private List<Employee> employees;

    public Manager(String name) {
        this.name = name;
        this.employees = new ArrayList<>();
    }

    public void addEmployee(Employee employee) {
        employees.add(employee);
    }

    public void removeEmployee(Employee employee) {
        employees.remove(employee);
    }

    @Override
    public void report() {
        System.out.println("Manager: " + name);
        for (Employee employee : employees) {
            employee.report();
        }
    }
}

public class CompositePatternDemo {
    public static void main(String[] args) {
        IndividualEmployee employee1 = new IndividualEmployee("John Doe");
        IndividualEmployee employee2 = new IndividualEmployee("Jane Smith");
        IndividualEmployee employee3 = new IndividualEmployee("Mike Johnson");

        Manager manager1 = new Manager("Alice Williams");
        manager1.addEmployee(employee1);
        manager1.addEmployee(employee2);

        Manager manager2 = new Manager("Bob Brown");
        manager2.addEmployee(employee3);

        manager1.addEmployee(manager2);

        // Report structure
        manager1.report();
    }
}


/*Q3. Develop a Java application where you have different types of shapes (e.g., Circle,
Rectangle) and draw implementations (e.g., DrawingAPI1, DrawingAPI2). Use the Bridge
pattern to separate shape logic from drawing logic.*/

package DrawingAPT.com;

interface Shape {
    void draw();
}

interface DrawingAPI {
    void drawCircle(int radius);
    void drawRectangle(int x, int y, int width, int height);
}

class DrawingAPI1 implements DrawingAPI {
    @Override
    public void drawCircle(int radius) {
        System.out.println("Drawing API 1: Circle with radius " + radius);
    }

    @Override
    public void drawRectangle(int x, int y, int width, int height) {
        System.out.println("Drawing API 1: Rectangle at (" + x + ", " + y + ") with width " + width + " and height " + height);
    }
}

class DrawingAPI2 implements DrawingAPI {
    @Override
    public void drawCircle(int radius) {
        System.out.println("Drawing API 2: Circle with radius " + radius);
    }

    @Override
    public void drawRectangle(int x, int y, int width, int height) {
        System.out.println("Drawing API 2: Rectangle at (" + x + ", " + y + ") with width " + width + " and height " + height);
    }
}

class Circle implements Shape {
    private int radius;
    private DrawingAPI drawingAPI;

    public Circle(int radius, DrawingAPI drawingAPI) {
        this.radius = radius;
        this.drawingAPI = drawingAPI;
    }

    @Override
    public void draw() {
        drawingAPI.drawCircle(radius);
    }
}

class Rectangle implements Shape {
    private int x, y, width, height;
    private DrawingAPI drawingAPI;

    public Rectangle(int x, int y, int width, int height, DrawingAPI drawingAPI) {
        this.x = x;
        this.y = y;
        this.width = width;
        this.height = height;
        this.drawingAPI = drawingAPI;
    }

    @Override
    public void draw() {
        drawingAPI.drawRectangle(x, y, width, height);
    }
}

public class BridgePatternDemo {
    public static void main(String[] args) {
        DrawingAPI drawingAPI1 = new DrawingAPI1();
        DrawingAPI drawingAPI2 = new DrawingAPI2();

        Shape circle1 = new Circle(5, drawingAPI1);
        Shape circle2 = new Circle(7, drawingAPI2);
        Shape rectangle1 = new Rectangle(10, 20, 50, 30, drawingAPI1);
        Shape rectangle2 = new Rectangle(30, 40, 70, 60, drawingAPI2);

        circle1.draw();
        circle2.draw();
        rectangle1.draw();
        rectangle2.draw();
    }
}

/*Q4. Create a basic Pizza class in Java and then implement decorators to add different toppings
(like cheese, olives, and mushrooms) to the pizza. Each topping should be an additional class
that extends a base decorator.
 */



package Pizza.com;


abstract class Pizza {
    public abstract double getCost();
    public abstract String getDescription();
}

class PlainPizza extends Pizza {
    @Override
    public double getCost() {
        return 10.0;
    }

    @Override
    public String getDescription() {
        return "Plain Pizza";
    }
}

abstract class ToppingDecorator extends Pizza {
    protected Pizza pizza;

    public ToppingDecorator(Pizza pizza) {
        this.pizza = pizza;
    }
}

class CheeseTopping extends ToppingDecorator {
    public CheeseTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public double getCost() {
        return pizza.getCost() + 2.0;
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + ", Cheese";
    }
}

class OliveTopping extends ToppingDecorator {
    public OliveTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public double getCost() {
        return pizza.getCost() + 1.5;
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + ", Olive";
    }
}

class MushroomTopping extends ToppingDecorator {
    public MushroomTopping(Pizza pizza) {
        super(pizza);
    }

    @Override
    public double getCost() {
        return pizza.getCost() + 2.5;
    }

    @Override
    public String getDescription() {
        return pizza.getDescription() + ", Mushroom";
    }
}

public class PizzaDecorator {
    public static void main(String[] args) {
        Pizza plainPizza = new PlainPizza();
        Pizza cheesePizza = new CheeseTopping(plainPizza);
        Pizza cheeseOlivePizza = new OliveTopping(cheesePizza);
        Pizza cheeseOliveMushroomPizza = new MushroomTopping(cheeseOlivePizza);

        System.out.println(cheeseOliveMushroomPizza.getDescription());
        System.out.println(cheeseOliveMushroomPizza.getCost());
    }
}




/*
  2.	Implement a VehicleFactory class in Java using the Factory Method design pattern.
  The factory should be able to create objects of Car and Bike classes based on the input provided.

*/

// Vehicle interface
interface Vehicle {
    void drive();
}

// Car class implementing Vehicle interface
class Car implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a car...");
    }
}

// Bike class implementing Vehicle interface
class Bike implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Riding a bike...");
    }
}

// VehicleFactory class
class VehicleFactory {
    // Factory method to create a Vehicle object
    public Vehicle createVehicle(String type) {
        if ("car".equalsIgnoreCase(type)) {
            return new Car();
        } else if ("bike".equalsIgnoreCase(type)) {
            return new Bike();
        }
        return null; // Return null for unknown types
    }
}

// Main class to demonstrate the factory
public class Q2 {
    public static void main(String[] args) {
        VehicleFactory factory = new VehicleFactory();

        // Create a car
        Vehicle car = factory.createVehicle("car");
        if (car != null) car.drive();

        // Create a bike
        Vehicle bike = factory.createVehicle("bike");
        if (bike != null) bike.drive();

        // Try to create an unknown vehicle
        Vehicle unknown = factory.createVehicle("plane");
        if (unknown == null) {
            System.out.println("Unknown vehicle type.");
        }
    }
}


/*
3.	Implement a Shape interface with a method clone().
Create concrete classes Circle and Rectangle that implement this interface.
Use the Prototype pattern to create a new instance of Circle or Rectangle by cloning the existing objects.

*/

// Shape interface with clone() method
interface Shape {
    Shape clone();
    void draw();
}

// Circle class implementing Shape interface
class Circle implements Shape {
    private int radius;

    public Circle(int radius) {
        this.radius = radius;
    }

    @Override
    public Shape clone() {
        return new Circle(this.radius);
    }

    @Override
    public void draw() {
        System.out.println("Drawing a Circle with radius: " + radius);
    }
}

// Rectangle class implementing Shape interface
class Rectangle implements Shape {
    private int width, height;

    public Rectangle(int width, int height) {
        this.width = width;
        this.height = height;
    }

    @Override
    public Shape clone() {
        return new Rectangle(this.width, this.height);
    }

    @Override
    public void draw() {
        System.out.println("Drawing a Rectangle with width: " + width + ", height: " + height);
    }
}

// Main class to demonstrate the Prototype pattern
public class Q3
{
    public static void main(String[] args) {
        // Create original Circle and Rectangle
        Shape originalCircle = new Circle(5);
        Shape originalRectangle = new Rectangle(4, 6);

        // Clone the Circle and Rectangle
        Shape clonedCircle = originalCircle.clone();
        Shape clonedRectangle = originalRectangle.clone();

        // Draw the original and cloned shapes
        System.out.println("Original Shapes:");
        originalCircle.draw();
        originalRectangle.draw();

        System.out.println("Cloned Shapes:");
        clonedCircle.draw();
        clonedRectangle.draw();
    }
}
