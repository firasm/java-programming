---
path: '/part-11/1-class-diagrams'
title: 'Class diagrams'
hidden: false
---


<text-box variant='learningObjectives' name='Learning Objectives'>




- Know how to draw class diagrams and how to describe classes and their attributes, constructors, and methods
- Know how to describe connections between classes and describe inheritance and interface implementation
- Can implement a class based on a class diagram

</text-box>



A class diagram is a diagram used in designing and modeling software to describe classes and their relationships. Class diagrams enable us to model software in a high level of abstraction and without having to look at the source code.


Classes in a class diagram correspond with classes in the source code. The diagram shows the names and attributes of the classes, connections between the classes, and sometimes also the methods of the classes.


Next we will get familiar with creating and reading class diagrams using <a href="https://en.wikipedia.org/wiki/Unified_Modeling_Language" target="_blank" norel>UML</a>. Using a unified modeling language ensures that class diagrams drawn by different people can be read and understood by everyone familiar with the language.

<br/>

## Describing class and class attributes


First we will describe one class and its attributes. Below is the source code for a class called `Person` which has two class attributes name and age.


```java
public class Person {
    private String name;
    private int age;
}
```


In a class diagram, a class is represented by a rectangle with the name of the class written on top.
A line below the name of the class divides the name from the list of attributes (names and types of the class variables). The attributes are written one attribute per line.


In a class diagram, class attributes are written "attributeName: attributeType". A + before the attribute name means the attribute is public, and a - means the attribute is private.


<img src="../img/diagrams/part4.1-classdiagram-person-name-age.png" alt="[Person|-name:String;-age:int]">


## Describing class constructor


Below we have the source code for a constructor for our Person class. The constructor gets the name of the person as a parameter.

```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.name = initialName;
        this.age = 0;
    }
}
```


In a class diagram, we list the constructor (and all other methods) below the attributes. A line below the attributes list separates it from the method list.
Methods are written with +/- (depending on the visibility of the method), method name, parameters, and their types. The constructor above is written `+Person(initialName:String)`


The parameters are written the same way class attributes are -- "parameterName: parameterType".


<img src="../img/diagrams/part4.1-classdiagram-person-name-age-constructor.png" alt="[person|-name:String;-age:int|+Person(initialName:String)]">


## Describing class methods



Below we have added a method printPerson() which returns void to the Person class.

```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.name = initialName;
        this.age = 0;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " +   this.age + " years");
    }
}
```


In a class diagram, we list all class methods including the constructors; constructors are listed first and then all class methods.
We also write the return type of a method in the class diagram.


<img src="../img/diagrams/part4.1-classdiagram-person-name-age-constructor-print.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void]">


<text-box variant='hint' name='A class diagram describes classes, construcutors and methods'>


A class diagram describes classes and their attributes, constructors and methods as well as the connections between classes. However a class diagram tells us nothing about the implementation of the constructors or the methods. Therefore a class diagram describes the structure of an object but not its functionality.


For example the method `printPerson` uses the class attributes `name` and `age`, but this cannot be seen from the class diagram.

</text-box>



Below we have added method `getName` to the Person class which returns the name of the Person.


```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.name = initialName;
        this.age = 0;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " +   this.age + " years");
    }

    public String getName() {
        return this.name;
    }
}
```


<img src="../img/diagrams/part11.1-classdiagram-person-age-name-constructor-print-getName.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void;+getName():String]">




<programming-exercise name='Customer' tmcname='part11-Part11_01.Customer'>



The class diagram below shows the class Customer. Implement the class in the exercise.

<img src="../img/exercises/class-diagram-customer.svg" alt="[Customer|-name:String;-address:String;-email:String]">

</programming-exercise>

<programming-exercise name='Book and plane' tmcname='part11-Part11_02.ABookAndAPlane'>



The class diagram below depicts the classes Book and Plane. Implement the classes in the exercise.

<img src="../img/exercises/class-diagram-book.svg" alt="[Book|-name:String;-author:String;-pageCount:int]">

<img src="../img/exercises/class-diagram-plane.svg" alt="[Plane|-ID:String;-model:String;-yearOfIntroduction:int]">



</programming-exercise>


## Connections between classes


In a class diagram, the connections between classes are shown as arrows. The arrows also show the direction of the connection.

Below we have a class Book.


```java
public class Book {
    private String name;
    private String publisher;

    // constructors and methods
}
```


<img src="../img/diagrams/part11.1-classdiagram-book-name-and-publisher.png" alt="[Book|-name:String;-publisher:String]">



If we add a variable author, type of which is Person, in the source code the variable is declared like all other class variables.


```java
public class Book {
    private String name;
    private String publisher;
    private Person author;

    // constructors and methods
}
```


In a class diagram variables which refer to other objects are not written with the rest of the class attributes, but are shown as connections between the classes. In the class diagram below we have the classes Person and Book, and the connection between them.



<img src="../img/diagrams/part11.1-classdiagram-book-name-and-publisher-and-author.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void;+getName():String][Book|-name:String;-publisher:String][Book]-author->[Person]">


The arrow shows the direction of the connection. The connection above shows that a Book knows its author but a Person does not know about books they are the author of.
We can also add a label to the arrow to describe the connection. In the above diagram the arrow has an accompanying label telling us that a Book has an author.


If a book has multiple authors, the authors are saved to a list.

```java
public class Book {
    private String name;
    private String publisher;
    private ArrayList<Person> authors;

    // constructors and methods
}
```


In a class diagram, this situation is described by adding a star to the end of the arrow showing the connection between the classes. The star tells us that a book can have between 0 and unlimited number of authors. Below we have not amended the label to describe the multiplicity of the connection, but it would be a good idea for the sake of clarity.


<img src="../img/diagrams/part11.1-classdiagram-book-name-and-publisher-and-authors.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void;+getName():String][Book|-name:String;-publisher:String][Book]-*->[Person]">



Class methods are described just like we did before. Below we have added methods `getAuthors` and `addAuthor` to the Book class.


```java
public class Book {
    private String name;
    private String publisher;
    private ArrayList<Person> authors;

    // constructor

    public ArrayList<Person> getAuthors() {
        return this.authors;
    }

    public void addAuthor(Person author) {
        this.authors.add(author);
    }
}
```


<img src="../img/diagrams/part11.1-classdiagram-book-name-and-publisher-and-authors-and-methods.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void;+getName():String][Book|-name:String;-publisher:String|+getAuthors():ArrayList;+addAuthor(author:Person)][Book]-*->[Person]">



We could add the type of the elements in the ArrayList `ArrayList<Person>` and a label describing the connection "authors" to the class diagram above.


<programming-exercise name='Show and ticket' tmcname='part11-Part11_03.ShowAndTicket'>



In the class diagram below, the classes Show and Ticket and their connection are depicted. The star is at the Ticket end of the connection -- in this case the star gives some extra information of the connection; even though a show doesn't know about the tickets that have been sold to it, you can still sell many tickets to one show.



Implement the classes in the diagram in the exercise base.



<img src="../img/exercises/class-diagram-show-and-ticket.svg" alt="[Show|-movie:String;-time:String]<-*[Ticket|-seat:int;-code:int]">

</programming-exercise>



If there is no arrowhead in a connection, both classes know about each other. Below is an example where a book knows about its author and a person knows about a book they have written.




<img src="../img/diagrams/part11.1-classdiagram-book-person-bidirectional.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void;+getName():String][Book|-name:String;-publisher:String|+getAuthors():ArrayList;+addAuthor(author:Person)][Book]-*[Person]">


```java
public class Person {
    private String name;
    private int age;
    private Book book;

    // ...
}
```


```java
public class Book {
    private String name;
    private String publisher;
    private ArrayList<Person> authors;

    // ..
}
```


As you can see, by default -- if there is no star on the connection -- the connection is singular. The classes above are interesting, because a Person can only have one book.


If a person can have multiple books and a book can have multiple authors, we add a star to both ends of the connection:


<img src="../img/diagrams/part11.1-classdiagram-book-person-bidirectional-many-to-many.png" alt="[Person|-name:String;-age:int|+Person(initialName:String);+printPerson():void;+getName():String][Book|-name:String;-publisher:String|+getAuthors():ArrayList;+addAuthor(author:Person)][Book]*-*[Person]">


Now the person class would be as follows:

```java
import java.util.ArrayList;

public class Person {
    private String name;
    private int age;
    private ArrayList<Book> books;

    // ...
}
```




<programming-exercise name='StudentAndUniversity' tmcname='part11-Part11_04.StudentAndUniversity'>



Two classes, Student and University, are depicted below, as well as the connection between them. Implement these classes in the exercise base.

<img src="../img/exercises/class-diagram-student-and-university.svg" alt="[Student|-studentID:int;-name:String]*-[University|-name:String]">

</programming-exercise>


## Describing inheritance


In a class diagram inheritance is described by an arrow with a triangle head. The triangle points to the class being inherited from. In the below example the Engine inherits the class Part.


<img src="../img/diagrams/part11.1-classdiagram-engine-inherits-part.png" alt="[Part|-id:String;-manufacturer:String;-description:String][Engine|-type:String][Part]^-[Engine]" />



In the below example the class diagram describes the classes from the Product warehouse exercise. The ProductWarehouseWithHistory class inherits the ProductWarehouse class, which, in turn, inherits the Warehouse class.
ChangeHistory is a separate class connected to the ProductWarehouse. ProductWarehouseWithHistory knows about the ChangeHistory but the ChangeHistory does not know about the ProductWarehouseWithHistory.


<img src="../img/diagrams/part11.1-classdiagram-productWarehouseWithHistory.png" alt="[Warehouse|-capacity:double;-balance:double|+Warehouse(capacity:double);+getBalance():double;+getCapacity():double;+howMuchSpaceLeft():double;+addToWarehouse(amount:double):void;+takeFromWarehouse(amount:double):double;+toString():String][ProductWarehouse|-name:String|+ProductWarehouse(name:String، capacity:double);+getName():String;+setName(name:String):String;+toString():String][ChangeHistory|-states:ArrayList|+ChangeHistory();+add(status:double);+clear():void;...][ProductWarehouseWithHistory||+ProductWarehouseWithHistory(name:String، capacity:double، initialBalance:double);+history():String;+printAnalysis():void;+addToWarehouse(amount:double);+takeFromWarehouse(amount:double):double][Warehouse]^-[ProductWarehouse][ProductWarehouse]^-[ProductWarehouseWithHistory][ChangeHistory]<-[ProductWarehouseWithHistory]" />



Inheritance of abstract classes is described almost the same way as regular classes. However we add the description `<<abstract>>` above the name of the class. The name of the class and its abstract methods are also written in cursive.



<img src="../img/diagrams/part11.1-classdiagram-abstracts.png" />


<programming-exercise name='The Player And the Bot' nocoins='true' tmcname='part11-Part11_05.ThePlayerAndTheBot'>



The classes Player and Bot and the connection between them are depicted in the class diagram below. Implement these classes in the exercise.



<img src="../img/exercises/class-diagram-player-and-bot.svg" alt="[Player|-name:String|+play():void;+printName():void]^-[Bot||+play():void;+addMove(move:String):void]">



A model answer is not supplied for this exercise.

</programming-exercise>


## Describing interfaces


In class diagrams, interfaces are written `<<interface>>` NameOfTheInterface. Below we describe an interface Readable.


```java
public interface Readable {

}
```


<img src="../img/diagrams/part11.1-classdiagram-interface-readable.png" alt="[&lt;&lt;interface&gt;&gt; Readable]">


Methods are described just like they are for a class.


Implementing an interface is shown as a dashed arrow with a triangle arrowhead. Below, we describe a situation where Book implements interface Readable.



<img src="../img/diagrams/part11.1-classdiagram-book-implements-readable.png" alt="[<<interface>> Readable][Book]-.-^[<<interface>> Readable]">


<programming-exercise name='Saveable person' tmcname='part11-Part11_06.SaveablePerson'>



Below you'll see the interface Saveable and the class Person. Implement the contents of this class diagram in the exercise base.



<img src="../img/exercises/class-diagram-saveable-person.svg" alt="[<<interface>> Saveable ||+save():void;+delete():void;+load(address:String):void]^-.-[Person|-name:String;-address:String]">

</programming-exercise>


<text-box variant='hint' name='How these should be drawn?'>


Class diagrams are an excellent way to describe a problem and a problem-domain to others. They are particularily useful when a programmer is designing a program with multiple classes.



Often a class diagram is drawn on a whiteboard or a large sheet of paper during the design phase. Class diagrams should be thought of as helpful tools to build a program, which can be thrown away afterwards. You should not use too much energy to think about the correctness and details of the modeling language.
Class diagrams should also be drawn in a suitable level of abstraction. For example, if you have tens of classes, it might not be worth describing each attribute and each method of each class; getting a good overview of the program structure is the most important.




<p>The class diagrams here have been drawn using <a href="https://yuml.me/" target="_blank" norel>yUML</a>, <a href="https://creately.com" target="_blank" norel>Creately</a>, and <a href="https://www.draw.io/" target="_blank" norel>draw.io</a>.
NetBeans also has tools for drawing class diagrams; for example, <a href="http://plugins.netbeans.org/plugin/55435/easyuml" target="_blank" norel>easyUML</a> draws class diagrams from the source code.</p>

</text-box>



<programming-exercise name='Bigger class diagram' tmcname='part11-Part11_07.BiggerClassDiagram'>



Below you'll see a somewhat larger class diagram. In it are the classes A, B, C, D, and E, as well as the interfaces IA, IB, and IC. Create these classes and interfaces in the exercise.

<img  src="../img/exercises/luokkakaavio-iso-abstrakti.png" alt="[<<interface>>;IA][<<interface>>;IB][<<interface>>;IC][A]-.-^[<<interface>>;IA][B]-.-^[<<interface>>;IB][C]-.-^[<<interface>>;IC][D]->[<<interface>>;IA][E]*-*[C][C]-^[B][B]-^[A]">

</programming-exercise>


<quiz id="1748c548-bfb5-506c-b56d-81bac26e7fc8"></quiz>
