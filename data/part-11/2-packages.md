---
path: '/part-11/2-packages'
title: 'Packages'
hidden: false
---




<text-box variant='learningObjectives' name='Learning Objectives'>

- You know what packages are and can place classes in them

- You know what the `import` statement used in Java is composed of

</text-box>



As the number of classes implemented for the program grows, remembering all the functionality and methods becomes more difficult. What helps is naming the classes in a sensible manner and planning them so that each class has one clear responsibility. In addition to these measures, it's wise to divide the classes into packages. Classes in one package might share funcionality, purpose, or some other logical property.



Packages are practically directories in which the source code files are organised.



IDEs offer existing tools for package management. Up until this point, we have only created classes and interfaces in the default package of the Source Packages folder of the project. You can create a new package in NetBeans by right-clicking on the Source Packages section (which contains the project's packages), and then selecting *New -&gt; Java Package...*.



You can create classes inside a package in the same way you can in the default package. Below we create the class `Program` in the newly created package `library`.



The package of a class (the package in which the class is stored) is noted at the beginning of the source code file with the statement `package *name-of-package*;`. Below, the class Program is in the package `library`.



```java
package library;

public class Program {

    public static void main(String[] args) {
        System.out.println("Hello packageworld!");
    }
}
```



Every package, including the default package, may contain other packages. For instance, in the package definition `package library.domain` the package `domain` is inside the package `library`. The word `domain` is often used to refer to the storage space of the classes that represent the concepts of the problem domain. For example, the class `Book` could be inside the package `library.domain`, since it represents a concept in the library application.




```java
package library.domain;

public class Book {
    private String name;

    public Book(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }
}
```



A class can access classes inside a package by using the `import` statement. The class `Book` in the package `library.domain` is made available for use with the statement `import library.domain.Book;`. The import statements that are used to import classes are placed in the source code file after the package definition.



```java
package library;

import library.domain.Book;

public class Program {

    public static void main(String[] args) {
        Book book = new Book("the ABCs of packages!");
        System.out.println("Hello packageworld: " + book.getName());
    }
}
```



<sample-output>

Hello packageworld: the ABCs of packages!

</sample-output>



From this point on, *nearly all* of the exercises will use packages. Let's begin by creating our first very own packages.




<programming-exercise name='First packages (3 parts)' tmcname='part11-Part11_08.FirstPackages'>

<h2>UserInterface</h2>



There is a package called `mooc` included in the exercise. We will create the functionality of the program inside this package. Add the package `ui` inside the package `mooc` (after which the package `mooc.ui` should be available), and add an interface called `UserInterface` in it.



The interface `UserInterface` defines the method `void update()`.




<h2>Text user interface</h2>



In the same package, create the class `TextInterface` that implements the `UserInterface` interface. Implement the method `public void update()`, required by the interface `UserInterface`, so that the only thing it does is print the string `"Updating UI"` by calling the method `System.out.println`.




<h2>Application logic</h2>



Then create the package `mooc.logic`. Create the class `ApplicationLogic` in it. The functionality that the application logic offers should be as follows:



- `public ApplicationLogic(UserInterface ui)`<br/>The constructor of the ApplicationLogic class. It receives as a parameter a class that implements the UserInterface interface. NB: For application logic to see the interface, it must be "imported". Add the line `import mooc.ui.UserInterface` at the beginning of the file.




- `public void execute(int times)`<br/>Prints the string "Application logic is working" the number of times that is indicated by the variable `times`. After each print, the method should call the `update()` method of the object that was received as a constructor parameter (which implements the `UserInterface` interface).




You can test the program with the following main program class.




```java
import mooc.logic.ApplicationLogic;
import mooc.ui.UserInterface;
import mooc.ui.TextInterface;

public class Main {

    public static void main(String[] args) {
        UserInterface ui = new TextInterface();
        new ApplicationLogic(ui).execute(3);
    }
}
```



<sample-output>

Application logic is working
Updating UI
Application logic is working
Updating UI
Application logic is working
Updating UI

</sample-output>

</programming-exercise>




<programming-exercise name='Three packages' tmcname='part11-Part11_09.TheThreePackages'>



Within the exercise base, create three packages: `a`, `b`, and `c`. Create class `A` inside the package `a`, class `B` inside the package `b`, and class `C` inside the package `c`. The classes don't need object variables, constructors, or methods.

</programming-exercise>





## Directory structure in a file system




<p>Every project you see in NetBeans is in your computer's <a href="https://en.wikipedia.org/wiki/File_system" target="_blank">file system</a> or on a centralized server.</p>



The project directory `src/main/java` contains the source code files of the program. If the package of a class is library, that class is stored inside the `src/main/java/libary` folder of the source code directory. You can also check the concrete project structure in NetBeans in the **Files** tab, which is normally next to the **Project** tab. If you cannot see the **Files** tab, you can make it appear by choosing the option **Files** from the dropdown menu **Window**.



Application development is normally done in the **Projects** tab, where NetBeans hides unimportant files from the programmer.



## Packages and access modifiers



Until now, we've used two access modifiers. The modifier `private` is used to define variables (and methods) that are only visible inside the class where they are defined. They cannot be used from outside that class. The methods and variables defined with `public`, on the other hand, are available for everyone to use.




```java
package library.ui;

public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {
        printTitle();

        // other functionality
    }

    private void printTitle() {
        System.out.println("***********");
        System.out.println("* LIBRARY *");
        System.out.println("***********");
    }
}
```



If you create an object of the `UserInterface` class above, its constructor and `start` method are callable from anywhere in the program. The method `printTitle` and the variable `scanner` are available only from within the class.



If the access modifier is missing, the methods and variables are only visible inside the same package. We call this the default or package modifier. Let's change the example above so that the method `printTitle` has package access modifier.



```java
package library.ui;

public class UserInterface {
    private Scanner scanner;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
    }

    public void start() {
        printTitle();

        // other functionality
    }

    void printTitle() {
        System.out.println("***********");
        System.out.println("* LIBRARY *");
        System.out.println("***********");
    }
}
```



Now the classes inside the same package -- i.e., the classes inside the package `library.ui` -- can use the method `printTitle`.



```java
package library.ui;

import java.util.Scanner;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        UserInterface ui = new UserInterface(scanner);

        ui.printTitle(); // works!
    }
}
```



If a class is in a different package, the method `printTitle` cannot be called. In the example below, the class `Main` is in the package `library`. As the `printTitle` method is in the package `library.ui` and has the package access modifier, it cannot be used.



```java
package library;

import java.util.Scanner;
import library.ui.UserInterface;

public class Main {

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        UserInterface ui = new UserInterface(scanner);

        ui.printTitle(); // doesn't work!
    }
}
```




## A larger example: flight control



Let's take a look at a program that offers a text UI for adding and examining airplanes and flights. The user interface of the program looks like this.



<sample-output>

Airport Asset Control
--------------------

Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **1**
Give the airplane id: **HA-LOL**
Give the airplane capacity: **42**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **1**
Give the airplane id: **G-OWAC**
Give the airplane capacity: **101**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **2**
Give the airplane id: **HA-LOL**
Give the departure airport id: **HEL**
Give the target airport id: **BAL**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **2**
Give the airplane id: **G-OWAC**
Give the departure airport id: **JFK**
Give the target airport id: **BAL**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **2**
Give the airplane id: **HA-LOL**
Give the departure airport id: **BAL**
Give the target airport id: **HEL**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **x**

Flight Control
------------

Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **1**
G-OWAC (101 capacity)
HA-LOL (42 capacity)
Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **2**
HA-LOL (42 passengers) (HEL-BAL)
HA-LOL (42 passengers) (BAL-HEL)
G-OWAC (101 passengers) (JFK-BAL)

Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **3**
Give the airplane id: **G-OWAC**
G-OWAC (101 capacity)

Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **x**

</sample-output>



There are many concepts of the problem domain in the program, and the essential ones are `Airplane` and `Flight`. Each flight also involves a `Place` (places of departure and destination). In addition to the concepts that represent the problem domain, the program also contains a text UI and a class through which the text UI uses the concepts.



The package structure of the program could look like the following (for example):



- `flightControl` - includes the main program class that is needed to start the program



- `flightControl.domain` - includes the classes that represent concepts of the problem domain: `Airplane`, `Flight`, and `Place`



- `flightControl.logic` - includes the functionality that is used to control the application



- `flightControl.ui` - includes the textual user interface




In the next subchapter, we list one possible organization for the operation of the program (excluding the main program class).




## Classes that represent concepts of the problem domain



The classes that represent concepts of the problem domain are often placed inside a package called `domain`. Since the entirety of the application is inside the package `flightControl`, let's place the package `domain` inside the package `flightControl`. Concepts of the problem domain are represented by the classes `Place`, `Airplane`, and `Flight`.



```java
package flightControl.domain;

public class Place {

    private String ID;

    public Place(String ID) {
        this.ID = ID;
    }

    @Override
    public String toString() {
        return this.ID;
    }
}
```



```java
package flightControl.domain;

public class Airplane {

    private String id;
    private int capacity;

    public Airplane(String id, int capacity) {
        this.id = id;
        this.capacity = capacity;
    }

    public String getID() {
        return this.id;
    }

    public int getCapacity() {
        return this.capacity;
    }

    @Override
    public String toString() {
        return this.id + " (" + this.capacity + " capacity)";
    }
}
```



```java
package flightControl.domain;
public class Flight {

    private Airplane airplane;
    private Place departureAirport;
    private Place targetAirport;

    public Flight(Airplane airplane, Place departureAirport, Place targetAirport) {
        this.airplane = airplane;
        this.departureAirport = departureAirport;
        this.targetAirport = targetAirport;
    }

    public Airplane getAirplane() {
        return this.airplane;
    }

    public Place getDeparturePlace() {
        return this.departureAirport;
    }

    public Place getTargetPlace() {
        return this.targetAirport;
    }

    @Override
    public String toString() {
        return this.airplane.toString() + " (" + this.departureAirport + "-" + this.targetAirport + ")";
    }
}

```




### Application logic



The application logic is typically kept separate from the classes that represents concepts of the problem domain. In our example, the application logic is stored in the package `logic`. Application logic includes the functionality to add airplanes and flights, and to list them.



```java
package flightControl.logic;

import java.util.Collection;
import flightControl.domain.Flight;
import flightControl.domain.Airplane;
import java.util.HashMap;
import java.util.Map;
import flightControl.domain.Place;

public class FlightControl {

    private HashMap<String, Airplane> airplanes = new HashMap<>();
    private HashMap<String, Flight> flights = new HashMap<>();
    private Map<String, Place> places;

    public FlightControl() {
        this.flights = new HashMap<>();
        this.airplanes = new HashMap<>();
        this.places = new HashMap<>();
    }

    public void addAirplane(String ID, int capacity) {
        Airplane plane = new Airplane(ID, capacity);
        this.airplanes.put(ID, plane);
    }

    public void addFlight(Airplane plane, String departureID, String destinationID) {
        this.places.putIfAbsent(departureID, new Place(departureID));
        this.places.putIfAbsent(destinationID, new Place(destinationID));

        Flight flight = new Flight(plane, this.places.get(departureID), this.places.get(destinationID));
        this.flights.put(flight.toString(), flight);
    }

    public Collection<Airplane> getAirplanes() {
        return this.airplanes.values();
    }

    public Collection<Flight> getFlights() {
        return this.flights.values();
    }

    public Airplane getAirplane(String ID) {
        return this.airplanes.get(ID);
    }
}

```

<quiz id="23c44e8c-1588-5cf5-9917-e666dbccefee"></quiz>




### Text user interface



The user interface is separate from the application logic and the classes that represent the problem domain. In this example, the user interface is stored in the package `ui`.



```java
package flightControl.ui;

import flightControl.domain.Flight;
import flightControl.domain.Airplane;
import java.util.Scanner;
import flightControl.logic.FlightControl;

public class TextUI {

    private FlightControl flightControl;
    private Scanner scanner;

    public TextUI(FlightControl flightControl, Scanner scanner) {
        this.flightControl = flightControl;
        this.scanner = scanner;
    }

    public void start() {
        // let's start in two parts -- first start the asset control,
        // then the flight control
        startAssetControl();
        System.out.println();
        startFlightControl();
        System.out.println();
    }

    private void startAssetControl() {
        System.out.println("Airport Asset Control");
        System.out.println("--------------------");
        System.out.println();

        while (true) {
            System.out.println("Choose an action:");
            System.out.println("[1] Add an airplane");
            System.out.println("[2] Add a flight");
            System.out.println("[x] Exit Airport Asset Control");

            System.out.print("> ");
            String answer = scanner.nextLine();

            if (answer.equals("1")) {
                addAirplane(scanner);
            } else if (answer.equals("2")) {
                addFlight(scanner);
            } else if (answer.equals("x")) {
                break;
            }
        }
    }

    private void addAirplane() {
        System.out.print("Give the airplane id: ");
        String id = scanner.nextLine();
        System.out.print("Give the airplane capacity: ");
        int capacity = Integer.parseInt(scanner.nextLine());

        this.flightControl.addAirplane(id, capacity);
    }

    private void addFlight() {
        System.out.print("Give the airplane id: ");
        Airplane airplane = askForAirplane(scanner);
        System.out.print("Give the departure airport id: ");
        String departureID = scanner.nextLine();
        System.out.print("Give the target airport id: ");
        String destinationID = scanner.nextLine();

        this.flightControl.addFlight(airplane, departureID, destinationID);
    }

    private void startFlightControl() {
        System.out.println("Flight Control");
        System.out.println("------------");
        System.out.println();

        while (true) {
            System.out.println("Choose an action:");
            System.out.println("[1] Print airplanes");
            System.out.println("[2] Print flights");
            System.out.println("[3] Print airplane details");
            System.out.println("[x] Quit");

            System.out.print("> ");
            String answer = scanner.nextLine();
            if (answer.equals("1")) {
                printAirplanes();
            } else if (answer.equals("2")) {
                printFlights();
            } else if (answer.equals("3")) {
                printAirplaneDetails();
            } else if (answer.equals("x")) {
                break;
            }
        }
    }

    private void printAirplanes() {
        for (Airplane plane : flightControl.getAirplanes()) {
            System.out.println(plane);
        }
    }

    private void printFlights() {
        for (Flight flight : flightControl.getFlights()) {
            System.out.println(flight);
            System.out.println("");
        }
    }

    private void printAirplaneDetails() {
        System.out.print("Give the airplane id: ");
        Airplane plane = askForAirplane();
        System.out.println(plane);
        System.out.println();
    }

    private Airplane askForAirplane() {
        Airplane airplane = null;
        while (airplane == null) {
            String id = scanner.nextLine();
            airplane = flightControl.getAirplane(id);

            if (airplane == null) {
                System.out.println("No airplane with the id " + id + ".");
            }
        }

        return airplane;
    }
}
```



<programming-exercise name='FlightControl (2 parts)' tmcname='part11-Part11_10.FlightControl'>



In this exercise, you will implement the application that was described above. You are free to design the structure as you wish, or you can follow the structure sketched out above. The appearance of the user interface and the required commands are predefined. This exercise is worth two normal exercise points.



**NB: for the tests to work, you can only create one Scanner object in your program to read user input.**



<p>In this exercise, you will implement a flight control application. It is used to control the airplanes and their flight routes. The system always knows the identifier and the capacity of an airplance. The flight information consists of the used airplane, the departure airport id (e.g. <a href="http://en.wikipedia.org/wiki/Helsinki_Airport" target="_blank" rel="noopener">HEL</a>), and the destination airport ID (e.g. <a href="http://en.wikipedia.org/wiki/Batman_Airport" target="_blank" rel="noopener">BAL</a>).</p>



There can be multiple airplanes and flights. The same airplane can be used to make several flights.



The application should operate in two parts: first, the user enters information about airplanes and flights in the airport asset control, after which the program offers the flight information service for the user. There are three operations in this latter flight control -- printing the airplanes, printing the flights, and printing the information of a single airplane. In addition, the user may exit the program by choosing the option `x`. If the user enters an invalid command, the program asks for a command again.



**The program should start when the main method of the Main class inside the package `FlightControl` is executed**



Example output of the program is presented below:



<sample-output>

Airport Asset Control
--------------------
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **1**
Give the airplane id: **HA-LOL**
Give the airplane capacity: **42**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **1**
Give the airplane id: **G-OWAC**
Give the airplane capacity: **101**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **2**
Give the airplane id: **HA-LOL**
Give the departure airport id: **HEL**
Give the target airport id: **BAL**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **2**
Give the airplane id: **G-OWAC**
Give the departure airport id: **JFK**
Give the target airport id: **BAL**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **2**
Give the airplane id: **HA-LOL**
Give the departure airport id: **BAL**
Give the target airport id: **HEL**
Choose an action:
[1] Add an airplane
[2] Add a flight
[x] Exit Airport Asset Control
&gt; **x**

Flight Control
------------
Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **1**
G-OWAC (101 capacity)
HA-LOL (42 capacity)
Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **2**
HA-LOL (42 capacity) (HEL-BAL)
HA-LOL (42 capacity) (BAL-HEL)
G-OWAC (101 capacity) (JFK-BAL)

Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **3**
Give the airplane id: **G-OWAC**
G-OWAC (101 capacity)

Choose an action:
[1] Print airplanes
[2] Print flights
[3] Print airplane details
[x] Quit
&gt; **x**

</sample-output>




**NB** For the purposes of the test it is essential that the *user interface* work **exactly** as described above. You should probably copy the options printed by the program from here to your code. The tests won't assume that your program is prepared to handle improper input.






</programming-exercise>
