---
path: '/part-13/1-graphical-user-interfaces'
title: 'Graphical user interfaces'
hidden: false
---


<text-box variant='learningObjectives' name='Learning Objectives'>


- You know what user interfaces (UIs) consist of and know how to launch a graphical user interfaces (GUIs).

</text-box>




We'll now take a look at creating graphical user interfaces (GUIs). When creating graphical user interfaces, we mostly make use of user-interface libraries that provide us with ready-made components, such as buttons and text areas. These user-interface libraries take care of the drawing the components for us, meaning that we don't have to draw every single component in our program, only add them to it.

Whereas text interfaces have the functionality coupled to a particular form of input, in GUIs it's added to the user-interface components. A programmer can, for instance, add a method to a button that lives on the interface, which handles an event associated with that button.

We'll be using Java's [JavaFx] (https://en.wikipedia.org/wiki/JavaFX) user-interface library to create our graphical user interfaces. The applications we develop are desktop applications.
<br/>


<text-box variant='hint' name='Graphical User Interfaces and Required Libraries'>


A library called JavaFX is used to create graphical user interfaces. On Linux machines, you may have to install the openjfx library -- depending on how Java's installed. You can install this in Ubuntu (from the command line) with the command:


```bash
user@computer:~$ sudo apt-get install openjfx
```


A test library called [TestFX] (https://github.com/TestFX/TestFX/wiki) is used in the exercise templates to test JavaFx programs. This library is included in the templates.

</text-box>


<text-box variant='hint' name='Required Rights on macOS for Exercise Tests'>



Some of the tests in this section assume that tmcbeans is able to freely move the cursor around the screen. On macOS (Apple computers), tmcbeans has to be granted this right explicitly. Here are instructions on how to grant this right: [macOS help] (/macos help)

</text-box>


We can create a simple window using JavaFX with the following program.


```java
package application;

import javafx.application.Application;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        window.setTitle("Hello World!");
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```



When the program is launched, it looks as follows.


<img src="../img/material/gui-helloworld.png" alt="Empty window with a 'Hello World' title"/>

When the launch method is called, the method of the Application class creates a new object from the given class (here JavaFxApplication) and calls its init method. The init method is defined in the Application class and is used, for instance, to initialize objects of a program. After calling the init method, the program calls the start method, which gets a [Stage] (https://docs.oracle.com/javase/8/javafx/api/javafx/stage/Stage.html) object as its parameter, which describes the window. In the implementation of the start method above, the setTitle method sets the title of the Stage-type window object obtained as a parameter. The method show is then called, which leads to the window being displayed. The program then stays in a state where it continuously listens to events on user interface, such as closing the window that causes the application to shut down.

<programming-exercise name='My first application' tmcname='part13-Part13_01.MyFirstApplication'>



Create a GUI app with the title "My first application". The app should start when the main method is executed.

</programming-exercise>



## Structure of a User Interface



Graphical user interfaces consist of three essential parts. The Stage object behaves as the program's window. A [Scene] (https://docs.oracle.com/javase/8/javafx/api/javafx/scene/Scene.html) is set for a Stage object that represents a scene within the window. The Scene object, on the other hand, contains an object responsible for arranging the components belonging to the scene (such as FlowPane), which contains the actual user interface components.

The program below creates an interface with a single button.


```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.FlowPane;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        Button button = new Button("This is a button");

        FlowPane componentGroup = new FlowPane();
        componentGroup.getChildren().add(button);

        Scene scene = new Scene(componentGroup);

        window.setScene(scene);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```


The application looks like this.


<img src="../img/material/gui-nappi.png" alt="A window, which has a button. The button contains the text 'This is a button'."/>




UI components are added as "children" to the object responsible for setting them -- FlowPane. This has to do with a JavaFx design decision, whereby each object responsible for UI components may contain other objects responsible for UI components as well as actual UI components. This enables GUIs where the layout of the UI components depends on their location on the user interface. For example, menu items located at the top of a UI are usually placed side by side, while list items are placed one below the other.


To briefly summarize, the UI structure is as follows. The window contains a Scene object. The Scene object contains the object responsible for the layout of the user-interface components. The object responsible for the component layout can contain both UI components and objects responsible for UI component layouts.
