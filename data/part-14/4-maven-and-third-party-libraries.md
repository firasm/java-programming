---
path: '/part-14/4-maven-and-third-party-libraries'
title: 'Maven and third-party libraries'
hidden: false
---

<text-box variant='learningObjectives' name='Learning Objectives'>





 - Know the term library and know some third party libraries
 - Know where to search for libraries
 - Can implement an application which uses a third party library
 - Know that an application can be packaged and shared with others, and that applications shared this way do not require a development environment to work.

</text-box>



The programming exercises of this course have been done in the NetBeans development environment with a Test My Code plugin for downloading and submitting exercises.
However the exercise templates we have used are not dependent on NetBeans or Test My Code in any way,  and you can use them in other development environments as well.



The exercise templates use a tool called [Maven](https://maven.apache.org/), which is used for executing and managing programs.
Maven dictates the structure of our projects -- every project has a file called `pom.xml` located at its root, and their source code is located in a directory called `src`.

The `src` directory typically has two sub-directories: `main` containing the project source code and `test` containing the source code of the tests for the project.


One advantage of using Maven is, that it is useful for downloading libraries.
Libraries are source code  which has been packaged to be used by anyone.
For example we have used the JUnit library for unit tests.
There is a library for almost anything. The search engine at [https://mvnrepository.com/](https://mvnrepository.com/) finds over 10 million libraries, although many of them are different versions of the same library.


You can find for example libraries for using databases or creating telegram bots.
Next you will get to know some of those libraries a bit better.




## Using a database



Databases, or rather database management systems, are tools for managing data. You have a chance to familiarize yourself better with them on the course Introduction to Databases (TKT10004). Let's have a brief look at an application that uses a database.



There are several libraries for using databases at the address [https://mvnrepository.com/](https://mvnrepository.com/). We will show an example of how to begin using the database management system called [H2](http://www.h2database.com/html/main.html). This can be done by adding the H2 library to the file `pom.xml`, between the tags `<dependencies>` and `</dependencies>`, which is the area for the library dependencies. You can take a look at the example below.


```xml


<dependencies>


    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <version>1.4.197</version>
    </dependency>


</dependencies>


```



This has already been done in the exercise base. When the library dependency has been configured as part of the project, the classes included in that library are usable in the project. In the following exercise you are going to use the previously described dependency, and implement a program that uses a database to manage data.



<programming-exercise name='Database' tmcname='part14-Part14_10.Database'>




The exercise base contains an application that has the H2 database configured as a dependency. It also includes the following four classes:



- `Todo`: a class that represents a task that is to be done. Each todo has a numerical identifier (id), a name, a description, and the information abot whether it has been done.



- `TodoDao`: a class that is used to store todos to the database. The word "dao" comes from the phrase "data access object". The class offers methods for listing, adding, setting as completed, and removing todos. Removing or setting as done is done on the basis of the id. The class constructor receives the location of the database.



- `UserInterface`: a class that is used to ask the user for instructions for what to do. The constructor receives a Scanner object and a TodoDao object as its parameters. Calling the `start` method starts the user interface, after which the user controls the program with their keyboard input.



- `Program`: a class that serves as a starting point for the program.




In this exercise it is your task to modify the user interface so that the user of the program can add new todos, or list, mark as done, or remove existing ones. Don't do changes in the classes `Todo`, `TodoDao`, or `Program`.



The finished program is expected to behave in the following manner:



```

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 1
Listing the database contents

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 2
Adding a new todo
Enter name
code
Enter description
code a whole lot

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 2
Adding a new todo
Enter name
prepare food
Enter description
rice porridge

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 1
Listing the database contents
Todo{id=1, name=code, description=code a whole lot, done=false}
Todo{id=2, name=prepare food, description=rice porridge, done=false}

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 3

Which todo should be marked as done (give the id)?
2

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 1
Listing the database contents
Todo{id=1, name=code, description=code a whole lot, done=false}
Todo{id=2, name=prepare food, description=rice porridge, done=true}

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 4

Which todo should be removed (give the id)?
2

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 1
Listing the database contents
Todo{id=1, name=code, description=code a whole lot, done=false}

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 3

Which todo should be marked as done (give the id)?
1

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> 1
Listing the database contents
Todo{id=1, name=code, description=code a whole lot, done=true}

Enter command:
1) list
2) add
3) mark as done
4) remove
x) quit
> x
Thank you!

```



The text user interface in this exercise does not differ in any essential way from the earlier text UIs we have created. The difference is that the data is being stored in a database: *the stored data will be available for the program when it is started the next time.*

</programming-exercise>


## Telegram bot


As we mentioned above, you can use Maven to find a great number of libraries you can use as a part of your own applications.
By searching telegram from [https://mvnrepository.com/](https://mvnrepository.com/) you find e.g the [TelegramBots](https://github.com/rubenlagus/TelegramBots) library for making Telegram bots.


Telegram bots are programs which react to messages sent on Telegram, and for example tell jokes.
Creating these bots is out of the scope of this course, but the skills you have learned will help you to study different tutorials. .
If you want to learn to make a Telegram bot, read the guide at [https://github.com/rubenlagus/TelegramBots/wiki/Getting-Started](https://github.com/rubenlagus/TelegramBots/wiki/Getting-Started).
Remember to start implementing your bot bit by bit -- start by for example making a bit which joins a channel.



## Packaging applications


So far it has seemed like our applications have required a development environment to work. This is not strictly true however, because executing a program on a development environment is quite the same as executing a program outside of it.
We can define a class which contains the `public static void main` method for starting the program.



Oracle has a [javapackager](https://docs.oracle.com/javase/8/docs/technotes/tools/unix/javapackager.html) tool for packaging an application.
You can find information about using it from [https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/packager.html](https://docs.oracle.com/javase/8/docs/technotes/guides/deploy/packager.html).


Using the instructions on the website linked above you can create packaged versions of your applications you can share with others.
The library used in the instructions can be found packaged as a Maven plugin from [https://github.com/javafx-maven-plugin/javafx-maven-plugin](https://github.com/javafx-maven-plugin/javafx-maven-plugin).


There are other tools for packaging, for example the [JavaPackager](https://github.com/fvarrui/JavaPackager) Maven plugin.

## Other development environment


Java is one of the most used programming languages in the world, and it is used i.e in Android phones.
The techniques for creating graphical user interfaces practiced in this course can be used for mobile applications as well, altough JavaFX is primarily targeted for desktop applications.
If you want to use JavaFX for a mobile application, [JavaFXPorts](https://gluonhq.com/products/mobile/javafxports/) project is created for that.
The JavaFXPorts project can be used to create mobile applications using the JavaFX library.
You can find more information about it from [https://docs.gluonhq.com/javafxports/](https://docs.gluonhq.com/javafxports/).



If you are interested in developing mobile applications, it is worth it to study the Android's developer guide for app developers which can be found from [https://developer.android.com/guide/](https://developer.android.com/guide/).
This programming course has given you excellent basis for learning app development.
Correspondingly if you are interested in developing simple (mobile) games, we recommend learning to use the [FXGL](https://github.com/AlmasB/FXGL/wiki) library.
