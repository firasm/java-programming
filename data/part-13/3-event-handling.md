---
path: '/part-13/3-event-handling'
title: 'Event handling'
hidden: false
---


<text-box variant='learningObjectives' name='Learning Objectives'>


- You're familiar with the concept of an event handler and are able to handle user interface events.

</text-box>




The user interfaces we've previously implemented have not been able to react to events in the user interface. This inability to react is not due to the components of the interface themselves, but due to the fact that we've yet to added any functionality that handles component events.



Button presses are handled using a class that implements the [EventHandler](https://docs.oracle.com/javase/8/javafx/api/javafx/event/EventHandler.html) interface. The type of the event in these cases is  [ActionEvent](https://docs.oracle.com/javase/8/javafx/api/javafx/event/ActionEvent.html). The interface implementation specifies *what is done* when a user presses a button.



```java
Button button = new Button("This is a button");
button.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent event) {
        System.out.println("Pressed!");
    }
});
```


If desired, the explicit implementation of the interface can be replaced by a Lambda expression.




```java
Button button = new Button("This is a button");
button.setOnAction((event) -> {
    System.out.println("Pressed!");
});
```


When the button is pressed, the program prints the text "Pressed!" to the console.




**Event handlers** attached to user interface components, such as the EventHandler used previously, are always connected to specific user interface components. Whenever an action is performed on a user interface component, e.g., a button is pressed, each of the event handlers attached to that particular component is called and the functionality written for them is executed.



We often want an event handler to change the state of some object. To get hold of an object, the event handler needs a reference to it. Let's think about the following user interface which has two text fields and a button.



```java
@Override
public void start(Stage window) {
    TextField leftText = new TextField();
    TextField rightText = new TextField();
    Button button = new Button("Copy");

    HBox componentGroup = new HBox();
    componentGroup.setSpacing(20);
    componentGroup.getChildren().addAll(leftText, button, rightText);

    Scene viewport = new Scene(componentGroup);

    window.setScene(viewport);
    window.show();
}
```


There is a text field on both the left and right hand sides of the user interface. In addition to these, there's a button in the middle with the text "Copy".


<img src="../img/material/gui-kopioija.png" alt="Two text fields and a button with the text 'Copy'."/>




We'd like to have an application where the content of the left text field is copied over to become the content of the right text field when the user presses the button. This can be done with the help of an object implementing the EventHandler interface.



```java
@Override
public void start(Stage window) {
    TextField leftText = new TextField();
    TextField rightText = new TextField();
    Button button = new Button("Copy");

    button.setOnAction((event) -> {
        rightText.setText(leftText.getText());
    });

    HBox componentGroup = new HBox();
    componentGroup.setSpacing(20);
    componentGroup.getChildren().addAll(leftText, button, rightText);

    Scene scene = new Scene(componentGroup);

    window.setScene(scene);
    window.show();
}
```




Now pressing the button results in the content of the left text field being copied to the text field on the right.


<img src="../img/material/gui-kopioija-2.png" alt="Two text fields and a button with the text 'Copy'."/>




NB! The method implemented can use objects that were declared before the method definition, as long as the values of the objects being used are not reassigned using the equals operator, i.e., the references do not change.


<programming-exercise name='Notifier' tmcname='part13-Part13_06.Notifier'>




Create a user interface in the class included in the exercise template. It should contain three user interface components. The topmost is a text field (TextField), the middle one is a button (Button), and component on the bottom is a text component (Label). Lay out the components using the VBox class. The user interface should look kind of like the following.

<img src="../img/material/part13.3-gui-notifier-1-sm.png" alt="."/>




Then add functionality to the application, where pressing the button leads to the text in the text field being copied as the value of the text component. In the example below we have written the message "hello world!" into the text field, after which we have pressed the button.

<img src="../img/material/part13.3-gui-notifier-2-sm.png" alt="."/>


</programming-exercise>




The eventhandler being used depends on what kind of user interface component we attach it to. If we want to listen to changes made to a text field character by character, then we would use the interface [ChangeListener](https://docs.oracle.com/javafx/2/api/javafx/beans/value/ChangeListener.html). In the example below we have attached an object implementing the ChangeListener interface to text field on the left. This object prints the changes in the text field to the console as well as sets the new value into the text field on the right.



```java
leftText.textProperty().addListener(new ChangeListener<String>() {
    @Override
    public void changed(ObservableValue<? extends String> change,
            String oldValue, String newValue) {

        System.out.println(oldValue + " -> " + newValue);
        oikeaTeksti.setText(newValue);
    }
});
```




In the previous example the changes being observed are in the text of the text field. Beacause text is in string format we have provided string as the type for the handler interface. As before, we can also express this code in a more compact form.




```java
leftText.textProperty().addListener((change, oldValue, newValue) -> {
    System.out.println(oldValue + " -> " + newValue);
    rightText.setText(newValue);
});
```




The program can also do statistics. Calculating the values for the text fields in the previous exercise is quite straightforward. Following the example below, the values would be updated every time the user changes the content of the text field.



```java
leftText.textProperty().addListener((change, oldValue, newValue) -> {
    int characters = newValue.length();
    String[] parts = newValue.split(" ");
    int words = parts.length;
    String longest = Arrays.stream(parts)
        .sorted((s1, s2) -> s2.length() - s1.length())
        .findFirst()
        .get();

    // set values of text elements
});
```



<programming-exercise name='Text statistics, part II' tmcname='part13-Part13_07.TextStatisticsPart2'>




Copy the work you did in the Text statistics exercise into the class included in the exercise template and add to it the functionality from the example above for calculating the statistics. The end result should an application that calculates statistics based on text that is input, which are updated automatically and displayed in the application.



<img src="../img/material/part13.3-gui-stats.gif" alt="A example of the functionality of an application made for text statistics."/>

</programming-exercise>
