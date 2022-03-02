# Inheritance and Interfaces 

---

## review Object and Class, read the rest

---

* Object : 

What Is an Object?

Objects are key to understanding object-oriented technology. Look around right now and you'll find many examples of real-world objects: your dog, your desk, your television set, your bicycle.

Real-world objects share two characteristics: They all have state and behavior. Dogs have state (name, color, breed, hungry) and behavior (barking, fetching, wagging tail). Bicycles also have state (current gear, current pedal cadence, current speed) and behavior (changing gear, changing pedal cadence, applying brakes). Identifying the state and behavior for real-world objects is a great way to begin thinking in terms of object-oriented programming.

* Bundling code into individual software objects provides a number of benefits, including:

Modularity: The source code for an object can be written and maintained independently of the source code for other objects. Once created, an object can be easily passed around inside the system.
Information-hiding: By interacting only with an object's methods, the details of its internal implementation remain hidden from the outside world.

Code re-use: If an object already exists (perhaps written by another software developer), you can use that object in your program. This allows specialists to implement/test/debug complex, task-specific objects, which you can then trust to run in your own code.
Pluggability and debugging ease: If a particular object turns out to be problematic, you can simply remove it from your application and plug in a different object as its replacement. This is analogous to fixing mechanical problems in the real world. If a bolt breaks, you replace it, not the entire machine.

* class 

What Is a Class?

In the real world, you'll often find many individual objects all of the same kind. There may be thousands of other bicycles in existence, all of the same make and model. Each bicycle was built from the same set of blueprints and therefore contains the same components. In object-oriented terms, we say that your bicycle is an instance of the class of objects known as bicycles. A class is the blueprint from which individual objects are created.


Ex : 

       class Bicycle {

            int cadence = 0;
            int speed = 0;
            int gear = 1;
        
            void changeCadence(int newValue) {
                 cadence = newValue;
            }
        
            void changeGear(int newValue) {
                 gear = newValue;
            }
        
            void speedUp(int increment) {
                 speed = speed + increment;   
            }
        
            void applyBrakes(int decrement) {
                 speed = speed - decrement;
            }
        
            void printStates() {
                 System.out.println("cadence:" +
                     cadence + " speed:" + 
                     speed + " gear:" + gear);
            }
        }

* The syntax of the Java programming language will look new to you, but the design of this class is based on the previous discussion of bicycle objects. The fields cadence, speed, and gear represent the object's state, and the methods (changeCadence, changeGear, speedUp etc.) define its interaction with the outside world.


---


# Interfaces 

---

There are a number of situations in software engineering when it is important for disparate groups of programmers to agree to a "contract" that spells out how their software interacts. Each group should be able to write their code without any knowledge of how the other group's code is written. Generally speaking, interfaces are such contracts.


**Interfaces in Java**


In the Java programming language, an interface is a reference type, similar to a class, that can contain only constants, method signatures, default methods, static methods, and nested types. Method bodies exist only for default methods and static methods. Interfaces cannot be instantiated—they can only be implemented by classes or extended by other interfaces. Extension is discussed later in this lesson.

**Defining an interface is similar to creating a new class:**

    public interface OperateCar {
    
    // constant declarations, if any
    
    // method signatures
    
    // An enum with values RIGHT, LEFT
    int turn(Direction direction,
    double radius,
    double startSpeed,
    double endSpeed);
    int changeLanes(Direction direction,
    double startSpeed,
    double endSpeed);
    int signalTurn(Direction direction,
    boolean signalOn);
    int getRadarFront(double distanceToCar,
    double speedOfCar);
    int getRadarRear(double distanceToCar,
    double speedOfCar);
    ......
    // more method signatures
    }   


* Note that the method signatures have no braces and are terminated with a semicolon


## Interfaces as APIs 

---

The robotic car example shows an interface being used as an industry standard Application Programming Interface (API). APIs are also common in commercial software products. Typically, a company sells a software package that contains complex methods that another company wants to use in its own software product. An example would be a package of digital image processing methods that are sold to companies making end-user graphics programs.



**Implementing an Interface**


To declare a class that implements an interface, you include an implements clause in the class declaration. Your class can implement more than one interface, so the `implements` keyword is followed by a comma-separated list of the interfaces implemented by the class. By convention, the implements clause follows the extends clause, if there is one.

Ex For Sample interface : 


        public interface Relatable {
        
            // this (object calling isLargerThan)
            // and other must be instances of 
            // the same class returns 1, 0, -1 
            // if this is greater than, 
            // equal to, or less than other
            public int isLargerThan(Relatable other);
        }


**Using an Interface as a Type**

When you define a new interface, you are defining a new reference data type. You can use interface names anywhere you can use any other data type name. If you define a reference variable whose type is an interface, any object you assign to it must be an instance of a class that implements the interface.


As an example, here is a method for finding the largest object in a pair of objects, for any objects that are instantiated from a class that implements Relatable:

    public Object findLargest(Object object1, Object object2) {
    Relatable obj1 = (Relatable)object1;
    Relatable obj2 = (Relatable)object2;
    if ((obj1).isLargerThan(obj2) > 0)
    return object1;
    else
    return object2;
    }

**By casting object1 to a Relatable type, it can invoke the isLargerThan method.**


* Default Methods

The section Interfaces describes an example that involves manufacturers of computer-controlled cars who publish industry-standard interfaces that describe which methods can be invoked to operate their cars. What if those computer-controlled car manufacturers add new functionality, such as flight, to their cars? These manufacturers would need to specify new methods to enable other companies (such as electronic guidance instrument manufacturers) to adapt their software to flying cars.

**Default methods enable you to add new functionality to the interfaces of your libraries and ensure binary compatibility with code written for older versions of those interfaces.**

* Extending Interfaces That Contain Default Methods

When you extend an interface that contains a default method, you can do the following:

Not mention the default method at all, which lets your extended interface inherit the default method.

Redeclare the default method, which makes it abstract.

Redefine the default method, which overrides it.

Suppose that you extend the interface TimeClient as follows:

    public interface AnotherTimeClient extends TimeClient { }
    Any class that implements the interface AnotherTimeClient will have the implementation specified by the default method TimeClient.getZonedDateTime.

Suppose that you extend the interface TimeClient as follows:

    public interface AbstractZoneTimeClient extends TimeClient {
    public ZonedDateTime getZonedDateTime(String zoneString);
    }


**Static Methods**

In addition to default methods, you can define static methods in interfaces. (A static method is a method that is associated with the class in which it is defined rather than with any object. Every instance of the class shares its static methods.) This makes it easier for you to organize helper methods in your libraries; you can keep static methods specific to an interface in the same interface rather than in a separate class. 


---



# Inheritance 

---

**Definitions** : A class that is derived from another class is called a subclass (also a derived class, extended class, or child class). The class from which the subclass is derived is called a superclass (also a base class or a parent class).

Excepting Object, which has no superclass, every class has one and only one direct superclass (single inheritance). In the absence of any other explicit superclass, every class is implicitly a subclass of Object.

Classes can be derived from classes that are derived from classes that are derived from classes, and so on, and ultimately derived from the topmost class, Object. Such a class is said to be descended from all the classes in the inheritance chain stretching back to Object.


Ex of Inheritance :


    public class Bicycle {
    
        // the Bicycle class has three fields
        public int cadence;
        public int gear;
        public int speed;
            
        // the Bicycle class has one constructor
        public Bicycle(int startCadence, int startSpeed, int startGear) {
            gear = startGear;
            cadence = startCadence;
            speed = startSpeed;
        }
            
        // the Bicycle class has four methods
        public void setCadence(int newValue) {
            cadence = newValue;
        }
            
        public void setGear(int newValue) {
            gear = newValue;
        }
            
        public void applyBrake(int decrement) {
            speed -= decrement;
        }
            
        public void speedUp(int increment) {
            speed += increment;
        }
    
    }

What You Can Do in a Subclass

A subclass inherits all of the public and protected members of its parent, no matter what package the subclass is in. If the subclass is in the same package as its parent, it also inherits the package-private members of the parent. You can use the inherited members as is, replace them, hide them, or supplement them with new members:

* The inherited fields can be used directly, just like any other fields.
* You can declare a field in the subclass with the same name as the one in the superclass, thus hiding it (not recommended).
* You can declare new fields in the subclass that are not in the superclass.
* The inherited methods can be used directly as they are.
* You can write a new instance method in the subclass that has the same signature as the one in the superclass, thus overriding it.
* You can write a new static method in the subclass that has the same signature as the one in the superclass, thus hiding it.
* You can declare new methods in the subclass that are not in the superclass.
* You can write a subclass constructor that invokes the constructor of the superclass, either implicitly or by using the keyword super.


**Private Members in a Superclass**

A subclass does not inherit the private members of its parent class. However, if the superclass has public or protected methods for accessing its private fields, these can also be used by the subclass.

A nested class has access to all the private members of its enclosing class—both fields and methods. Therefore, a public or protected nested class inherited by a subclass has indirect access to all of the private members of the superclass