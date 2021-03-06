---
path: '/part-11/3-exceptions'
title: 'Exceptions'
hidden: false
---



<text-box variant='learningObjectives' name='Learning Objectives'>




 - Know what exceptions are and how to handle them
 - Can throw exceptions
 - Know that some exceptions have to be handled and that some exceptions do not have to be handled.

</text-box>


When program execution ends with an error, an exception is thrown. For example, a program might call a method with a *null* reference and throw a `NullPointerException`, or the program might try to refer to an element outside an array and result in an `IndexOutOfBoundsException`, and so on.


Some exceptions we have to always prepare for, such as errors when reading from a file or errors related to problems with a network connection. We do not have to prepare for runtime exceptions, such as the NullPointerException, beforehand. Java will always let you know if your code has a statement or an expression which can throw an error you have to prepare for.


## Handling exceptions


We use the `try {} catch (Exception e) {}` block structure to handle exceptions. The keyword `try` starts a block containing the code which *might* throw an exception. the block starting with the keyword `catch` defines what happens if an exception is thrown in the `try` block. The keyword `catch` is followed by the type of the exception handled by that block, for example "all exceptions" `catch (Exception e)`.


```java
try {
    // code which possibly throws an exception
} catch (Exception e) {
    // code block executed if an exception is thrown
}
```


We use the keyword `catch`, because causing an exception is referred to as `throw`ing an exception.


As mentioned above, we do not have to prepare for runtime exceptions such as the NullPointerException. We do not have to handle these kinds of exceptions, so the program execution stops if an error causes the exception to be thrown.
Next, we will look at one such situation, parsing strings to integers.



We have used the <a href="http://docs.oracle.com/javase/8/docs/api/java/lang/Integer.html#parseInt-java.lang.String-" target="_blank" rel="noopener">parseInt</a> method of the `Integer` class before.
The method throws a `NumberFormatException` if the string it has been given cannot be parsed into an integer.

<br/>

```java
Scanner reader = new Scanner(System.in);
System.out.print("Give a number: ");

int readNumber = Integer.parseInt(reader.nextLine());
```

<sample-output>

Give a number: **dinosaur**

  **Exception in thread "..." java.lang.NumberFormatException: For input string: "dinosaur"**

</sample-output>



The above program throws an error if the user input is not a valid number. The exception will cause the program execution to stop.


Let's handle the exception. We wrap the call that might throw an exception into a `try` block, and the code executed if the exception is thrown into a `catch` block.

```java
Scanner reader = new Scanner(System.in);

System.out.print("Give a number: ");
int readNumber = -1;

try {
    readNumber = Integer.parseInt(reader.nextLine());
} catch (Exception e) {
    System.out.println("User input was not a number.");
}
```

<sample-output>

Give a number: **5**

</sample-output>

<sample-output>

Give a number: **no!**
User input was not a number.

</sample-output>



The code in the `catch` block is executed immediately if the code in the `try` block throws an exception.
We can demonstrate this by adding a print statement below the line calling the `Integer.parseInt` method in the `try` block.

```java
Scanner reader = new Scanner(System.in);

System.out.print("Give a number: ");
int readNumber = -1;

try {
    readNumber = Integer.parseInt(reader.nextLine());
    System.out.println("Good job!");
} catch (Exception e) {
    System.out.println("User input was not a numer.");
}
```

<sample-output>

Give a number: **5**
Good job!

</sample-output>

<sample-output>

Give a number: **no!**
User input was not a number.

</sample-output>



The user input, in this case the string `no!`, is given to the `Integer.parseInt` method as a parameter.
The method throws an error if the string cannot be parsed into an integer.
Note, that the code within the `catch` block is executed *only* if an exception is thrown.



Let's make our integer parser a bit more useful.
We'll turn it into a method which prompts the user for a number until they give a valid number.
The execution stops only when the user gives a valid number.

```java
public int readNumber(Scanner reader) {
    while (true) {
        System.out.print("Give a number: ");

        try {
            int readNumber = Integer.parseInt(reader.nextLine());
            return readNumber;
        } catch (Exception e) {
            System.out.println("User input was not a number.");
        }
    }
}
```

<sample-output>

Give a number: **no!**
User input was not a number.
Give a number: **Matt has moss in his head**
User input was not a number.
Give a number: **43**

</sample-output>


## Exceptions and resources


There is separate exception handling for reading operating system resources such as files.
With so called try-with-resources exception handling, the resource to be opened is added to a non-compulsory part of the try block declaration in brackets.


The code below reads all lines from "file.txt" and adds them to an ArrayList.
Reading a file might throw an exception, so it requires a try-catch block.

```java
ArrayList<String> lines =  new ArrayList<>();

// create a Scanner object for reading files
try (Scanner reader = new Scanner(new File("file.txt"))) {

    // read all lines from a file
    while (reader.hasNextLine()) {
        lines.add(reader.nextLine());
    }
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}

// do something with the lines
```


The try-with-resources approach is useful for handling resources, because the program closes the used resources automatically.
Now references to files can "disappear", because we do not need them anymore.
If the resources are not closed, the operating system sees them as being in use until the program is closed.

## Shifting the responsibility



Methods and constructors can throw exceptions. There are roughly two categories of exceptions. There are exceptions we have to handle, and exceptions we do not have to handle.
We can handle exceptions by wrapping the code into a `try-catch` block or *throwing them out of the method*.


The code below reads the file given to it as a parameter line by line.
Reading a file can throw an exception -- for example, the file might not exist or the program does not have read rights to the file.
This kind of exception has to be handled.
We handle the exception by wrapping the code into a `try-catch` block.
In this example we do not really care about the exception, but we do print a message to the user about it.

```java
public List<String> readLines(String fileName){
    List<String> lines =  new ArrayList<>();

    try {
        Files.lines(Paths.get("file.txt")).forEach(line -> lines.add(line));
    } catch (Exception e) {
        System.out.println("Error: " + e.getMessage());
    }

    return lines;
}
```


A programmer can also leave the exception unhandled and shift the responsibility for handling it to whomever calls the method.
We can shift the responsibility of handling an exception forward by throwing the exception out of a method, and adding notice of this to the declaration of the method.
Notice on throwing an exception forward `throws *ExceptionType*` is added before the opening bracket of a method.

```java
public List<String> readLines(String fileName) throws Exception {
    ArrayList<String> lines =  new ArrayList<>();
    Files.lines(Paths.get(fileName)).forEach(line -> lines.add(line));
    return lines;
}
```


Now the method calling the `readLines` method has to either handle the exception in a `try-catch` block or shift the responsibility yet forward.
Sometimes the responsibility of handling exceptions is avoided until the end, and even the `main` method can throw an exception to the caller:

```java
public class MainProgram {
   public static void main(String[] args) throws Exception {
       // ...
   }
}
```


Now the exception is thrown to the program executor, or the Java virtual machine. It stops the program execution when an error causing an exception to be thrown occurs.

## Throwing exceptions


The `throw` command throws an exception.
For example a `NumberFormatException` can be done with command `throw new NumberFormatException()`.
The following code always throws an exception.

```java
public class Program {

    public static void main(String[] args) throws Exception {
        throw new NumberFormatException(); // Program throws an exception
    }
}
```


One exception that the user does not have to prepare for is `IllegalArgumentException`.
The `IllegalArgumentException` tells the user that the values given to a method or a constructor as parameters are *wrong*.
It can be used when we want to ensure certain parameter values.


Lets create class `Grade`. It gets a integer representing a grade as a constructor parameter.

```java
public class Grade {
    private int grade;

    public Grade(int grade) {
        this.grade = grade;
    }

    public int getGrade() {
        return this.grade;
    }
}
```


We want that the grade fills certain criteria. The grade has to be an integer between 0 and 5. If it is something else, we want to *throw an exception*.
Let's add a conditional statement to the constructor, which checks if the grade fills the criteria.
If it does not, we throw the `IllegalArgumentException` with `throw new IllegalArgumentException("Grade must be between 0 and 5.");`.

```java
public class Grade {
    private int grade;

    public Grade(int grade) {
        if (grade < 0 || grade > 5) {
            throw new IllegalArgumentException("Grade must be between 0 and 5.");
        }

        this.grade = grade;
    }

    public int getGrade() {
        return this.grade;
    }
}
```

```java
Grade grade = new Grade(3);
System.out.println(grade.getGrade());

Grade illegalGrade = new Grade(22);
// exception happens, execution will not continue from here
```

<sample-output>

3
Exception in thread "..." java.lang.IllegalArgumentException: Grade must be between 0 and 5.

</sample-output>



If an exception is a runtime exception, e.g. IllegalArgumentException, we do not have to warn about throwing it on the method declaration.



<programming-exercise name='Validating parameters (2 parts)' tmcname='part11-Part11_11.ValidatingParameters'>



Let's practise a little parameter validation with the `IllegalArgumentException` exception. There are two classes included with the exercise base: `Person` and `Calculator`. Modify the classes in the following manner:



<h2>Validating a person</h2>



The constructor of the class `Person` should ensure that the name given as the parameter is not null, empty, or over 40 characters in length. The age should between 0 and 120. If some of these conditions are not met, the constructor should throw an `IllegalArgumentException`.




<h2>Validating the calculator</h2>




The methods of the `Calculator` class should be as follows: The method `factorial` should only work if the parameter is a non-negative number (0 or greater). The method `binomialCoefficient` should only work when the parameters are non-negative and the subset size does not exceed the set size. If these methods receive invalid parameters when they are called, they should throw an `IllegalArgumentException`

</programming-exercise>


<text-box variant='hint' name='Types of exceptions'>


We said "*there are roughly two categories of exceptions: exceptions which must be handled, and exceptions which do not have to be handled.*"


Exceptions which must be handled are exceptions which are checked for during compilation.
Due to this, some exceptions have to be prepared for with a `try-catch` block or by throwing them out of a method with a `throws` attribute in a method declaration.
For example, exceptions related to handling files, including `IOException` and `FileNotFoundException`, are this kind of exception.


Some exceptions are not checked for during compilation. They can be thrown during execution.
These kinds of exceptions do not have to be handled with a `try-catch` block. For example, `IllegalArgumentException` and `NullPointerException` are this kind of exception.

</text-box>


## Exceptions and Interfaces


An Interface can have methods which throw an exception.
For example the classes implementing the following `FileServer` interface *might* throw an exception from the methods `load` or `save`.

```java
public interface FileServer {
    String load(String fileName) throws Exception;
    void save(String fileName, String textToSave) throws Exception;
}
```


If an interface declares a `throws Exception` attribute to a method, so that these methods might throw an exception, the class implementing this interface must also have this attribute.
However the class does not have to throw an error, as we can see below.

```java
public class TextServer implements FileServer {

    private Map<String, String> data;

    public TextServer() {
        this.data = new HashMap<>();
    }

    @Override
    public String load(String fileName) throws Exception {
        return this.data.get(fileName);
    }

    @Override
    public void save(String fileName, String textToSave) throws Exception {
        this.data.put(fileName, textToSave);
    }
}
```

## Details of the exception



A `catch` block defines which exception to prepare for with `catch (Exception e)`.
The details of the exception are saved to the `e` variable.

```java
try {
    // program code which might throw an exception
} catch (Exception e) {
    // details of the exception are stored in the variable e
}
```


The `Exception` class has some useful methods. For example `printStackTrace()` prints the *stack trace*, which shows how we ended up with an exception.
Below is a stack trace printed by the `printStackTrace()` method.

<sample-output>

Exception in thread "main" java.lang.NullPointerException
  at package.Class.print(Class.java:43)
  at package.Class.main(Class.java:29)

</sample-output>



We read a stack trace from the bottom up. At the bottom is the first call, so the execution of the program has begun from the `main()` method of the `Class` class.
Line 29 of the main method of the `Class` class calls the `print()` method.
Line 43 of the `print` method has thrown a `NullPointerException` exception.
The details of an exception are very useful when trying to pinpoint where an error happens.


<quiz id="96e2f6ba-4efd-54c0-8b60-a93c8de397c0"></quiz>




<programming-exercise name='Sensors and temperature (4 parts)' tmcname='part11-Part11_12.SensorsAndTemperature'>




All the classes should be placed inside the `application` package.



We have the following interface at our disposal:



```java
public interface Sensor {
    boolean isOn();    // returns true if the sensor is on
    void setOn();      // sets the sensor on
    void setOff();     // sets the sensor off
    int read();        // returns the value of the sensor if it's on
                       // if the sensor is not on, throw an IllegalStateException
}
```



<h2>Standard sensor</h2>



Create a class called `StandardSensor` that implements the interface `Sensor`.



A standard sensor is always on. Calling the methods setOn and setOff have no effect. The StandardSensor must have a constructor that takes one integer parameter. The method call `read` returns the number that was given to the constructor.



An example:



```java
public static void main(String[] args) {
    StandardSensor ten = new StandardSensor(10);
    StandardSensor minusFive = new StandardSensor(-5);

    System.out.println(ten.read());
    System.out.println(minusFive.read());

    System.out.println(ten.isOn());
    ten.setOff();
    System.out.println(ten.isOn());
}
```

<sample-output>

10
-5
true
true

</sample-output>




<h2>TemperatureSensor</h2>



Create a class `TemperatureSensor` that implements the `Sensor` interface.



At first a temperature sensor is off. When the method `read` is called and the sensor is on, the sensor randomly chooses an integer in the range -30...30 and returns it. If the sensor is off, the method `read` throws an `IllegalStateException`.



Use the ready-made Java class <a href="https://docs.oracle.com/javase/8/docs/api/java/util/Random.html" target="_blank" rel="noopener">Random</a> to choose the random number. You'll get an integer in the range 0...60 by calling `new Random().nextInt(61);` -- to get a random integer in the range -30...30 you'll have to subtract a suitable number from the random number in the range 0...60.

<br/>




<h2>Average sensor</h2>



Create a class called `AverageSensor` that implements the interface `Sensor`.



An average sensor includes multiple sensors. In addition to the methods defined in the `Sensor` interface, the AverageSensor has the method `public void addSensor(Sensor toAdd)` that can be used to add a new sensor for the average sensor to control.



An AverageSensor is on when *all* its sensors are on. When `setOn` is called, all the sensors must be set on. When `setOff` is called, at least one of the sensors must be set off. It's also acceptable to set off all the sensors.



The method `read` of AverageSensor returns the average of the `read` methods of its sensors (since the return value is `int`, the number is rounded down as is the case with integer division). If this method is called while the AverageSensor is off, or if no sensors have been added to it, the method should throw an `IllegalStateException`.



Here follows an example program that uses the sensors (NB: the constructors of both TemperatureSensor and AverageSensor are non-parameterized):




```java
public static void main(String[] args) {
    Sensor kumpula = new TemperatureSensor();
    kumpula.setOn();
    System.out.println("temperature in Kumpula " + kumpula.read() + " degrees Celsius");

    Sensor kaisaniemi = new TemperatureSensor();
    Sensor helsinkiVantaaAirport = new TemperatureSensor();

    AverageSensor helsinkiRegion = new AverageSensor();
    helsinkiRegion.addSensor(kumpula);
    helsinkiRegion.addSensor(kaisaniemi);
    helsinkiRegion.addSensor(helsinkiVantaaAirport);

    helsinkiRegion.setOn();
    System.out.println("temperature in Helsinki region " + helsinkiRegion.read() + " degrees Celsius");
}
```



The print statements below depend on which temperatures are randomly returned:




<sample-output>

temperature in Kumpula 11 degrees Celsius
temperature in Helsinki region 8 degrees Celsius

</sample-output>




<h2>All readings</h2>



Add to the class AverageSensor the method `public List<Integer> readings()`. The method should return the results of all the executed readings that the average sensor has done as a list. Here is an example:



```java
public static void main(String[] args) {
    Sensor kumpula = new TemperatureSensor();
    Sensor kaisaniemi = new TemperatureSensor();
    Sensor helsinkiVantaaAirport = new TemperatureSensor();

    AverageSensor helsinkiRegion = new AverageSensor();
    helsinkiRegion.addSensor(kumpula);
    helsinkiRegion.addSensor(kaisaniemi);
    helsinkiRegion.addSensor(helsinkiVantaaAirport);

    helsinkiRegion.setOn();
    System.out.println("temperature in Helsinki region " + helsinkiRegion.read() + " degrees Celsius");
    System.out.println("temperature in Helsinki region " + helsinkiRegion.read() + " degrees Celsius");
    System.out.println("temperature in Helsinki region " + helsinkiRegion.read() + " degrees Celsius");

    System.out.println("readings: " + helsinkiRegion.readings());
}
```



Again, the exact print statements depend on the randomized readings:

<sample-output>

temperature in Helsinki region -10 degrees Celsius
temperature in Helsinki region -4 degrees Celsius
temperature in Helsinki region 5 degrees Celsius

readings: [-10, -4, 5]

</sample-output>

</programming-exercise>
