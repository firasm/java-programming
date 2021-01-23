---
path: '/part-12/1-type-parameters'
title: 'Type parameters'
hidden: false
---



<text-box variant='learningObjectives' name='Learning Objectives'>



- You know what is meant with the concept of a generic type parameter.



- You are familiar with already existing Java classes that make use of generic type parameters.



- You can create classes of your own that make use of generic type parameters.

</text-box>




Since we began using lists, we have given data structures the type of the values that we want them to store. For example, a list that stores strings has been defined as `ArrayList<String>`, and a hash map that stores keys and values as Strings has been defined as `HashMap<String, String>`. How on Earth can you implement a class that can contain objects of a given type?



*Generics* relates to how classes that store objects can store objects of a freely chosen type. The choice is based on the generic type parameter in the definition of the classes, which makes it possible to choose the type(s) *at the moment of the object's creation*. Using generics is done in the following manner: after the name of the class, follow it with a chosen number of type parameters. Each of them is  placed between the 'smaller than' and 'greater than' signs, like this: `public class Class<TypeParameter1, TypeParameter2, ...>`. The type parameters are usually defined with a single character.



Let's implement our own generic class `Locker` that can hold one object of any type.



```java
public class Locker<T> {
    private T element;

    public void setValue(T element) {
        this.element = element;
    }

    public T getValue() {
        return element;
    }
}
```




The definition `public class Locker<T>` indicates that the `Locker` class must be given a type parameter in its constructor. After the constructor call is executed, all the variables stored in that object are going to be of the type that was given with the constructor. Let's create a locker for storing strings.




```java
Locker<String> string = new Locker<>();
string.setValue(":)");

System.out.println(string.getValue());
```

<sample-output>

:)

</sample-output>




In the program above, the **runtime** implementation of the `Locker` object named `string` looks like the following.




```java
public class Locker<String> {
    private String element;

    public void setValue(String element) {
        this.element = element;
    }

    public String getValue() {
        return element;
    }
}
```



Changing the type parameter allows for creating lockers that store objects of other types. You could store an integer in the following manner.



```java
Locker<Integer> integer = new Locker<>();
integer.setValue(5);

System.out.println(integer.getValue());
```

<sample-output>

5

</sample-output>




Similarly, here is how to create a locker for storing a `Random` object.



```java
Locker<Random> random = new Locker<>();
random.setValue(new Random());

System.out.println(random.getValue().nextDouble());
```




There is no maximum on the number of type parameters, it's all dependent on the implementation. The programmer could implement the following `Pair` class that is able to store two objects of specified types.



```java
public class Pair<T, K> {
    private T first;
    private K second;

    public void setValues(T first, K second) {
        this.first = first;
        this.second = second;
    }

    public T getFirst() {
        return this.first;
    }

    public K getSecond() {
        return this.second;
    }
}
```

<quiz id="4e28cce5-9043-58be-ac53-5243976494bd"></quiz>




A significant portion of the Java data structures use type parameters, which enables them to handle different types of variables. ArrayList, for instance, receives a single type parameter, while HashMap receives two.




```java
List<String> strings = new ArrayList<>();
Map<String, String> keyValuePairs = new HashMap<>();
```




From here on out when you see the type `ArrayList<String>`, you know that its internal implementation uses a generic type parameter. The same principle holds true for the interface Comparable, for example.



Creating generic interfaces is very similar to creating generic classes. Below you can study the generic interface `List` (our own definition, which is not as extensive as the existing Java [List](https://docs.oracle.com/javase/8/docs/api/java/util/List.html)).




```java
public interface List<T> {
    void add(T value);
    T get(int index);
    T remove(int index);
}
```




There are two ways for a class to implement a generic interface. One is to decide the type parameter in the definition of the class, and the other is to define the implementing class with a type parameter as well. Below, the class `MovieList` defines the type parameter when it implements List. The MovieList is meant only for handling movies.




```java
public class MovieList implements List<Movie> {
    // object variables

    @Override
    public void add(Movie value) {
        // implementation
    }

    @Override
    public Movie get(int index) {
        // implementation
    }

    @Override
    public Movie remove(int index) {
        // implementation
    }
}
```



The alternative is to use a type parameter in the class defintion, in which case the parameter is passed along to the interface. Now this concrete implementation of the interface remains generic.



```java
public class GeneralList<T> implements List<T> {
    // object variables

    @Override
    public void add(T value) {
        // implementation
    }

    @Override
    public T get(int index) {
        // implementation
    }

    @Override
    public T remove(int index) {
        // implementation
    }
}
```



If you wanted, you could also use the existing ArrayList class to implement the GeneralList. The implementation would look roughly like this.



```java
import java.util.ArrayList;

public class GeneralList<T> implements List<T> {
    ArrayList<T> values;

    public GeneralList() {
        this.values = new ArrayList<>();
    }

    @Override
    public void add(T value) {
        this.values.add(value);
    }

    @Override
    public T get(int index) {
        retun this.values.get(index);
    }

    @Override
    public T remove(int index) {
        T value = this.values.get(index);
        this.values.remove(index);
        return value;
    }
}
```




<programming-exercise name='Hideout' tmcname='part12-Part12_01.Hideout'>




Implement a class called Hideout, which has a single generic type parameter. The object created from the class may only hide one object at a time. The class should provide a parameterless constructor as well as the following three methods:




- `public void putIntoHideout(T toHide)` puts an object with a type in accordance with the type parameter given to the the class into the hideout. In case an object is already in the hideout, it will disappear.



- `public T takeFromHideout()` takes out the object with a type in accordance with the type parameter given to the the class from the hideout. In case there is nothing in the hideout, we return `null`. Calling the method returns the object in the hideout and removes the object from the hideout.



- `public boolean isInHideout()` returns the value `true` if there is an object in the hideout. In case there isn't an object in the hideout, it should return the value `false`.



**There are no tests for the class in the exercise template**. Submit the exercise when the following examples work as intended.




```java
Hideout<String> den = new Hideout<>();
System.out.println(den.isInHideout());
den.putIntoHideout("peekaboo");
System.out.println(den.isInHideout());
System.out.println(den.takeFromHideout());
System.out.println(den.isInHideout());
den.putIntoHideout("toodaloo");
den.putIntoHideout("heelloo");
System.out.println(den.isInHideout());
System.out.println(den.takeFromHideout());
System.out.println(den.isInHideout());
```



<sample-output>

false
true
peekaboo
false
true
heelloo
false

</sample-output>



```java
Hideout<Integer> den = new Hideout<>();
System.out.println(den.isInHideout());
den.putIntoHideout(1);
System.out.println(den.isInHideout());
System.out.println(den.isInHideout());
System.out.println(den.takeFromHideout());
System.out.println(den.isInHideout());
```

<sample-output>

false
true
true
1
false

</sample-output>

</programming-exercise>




<programming-exercise name='Pipe' tmcname='part12-Part12_02.Pipe'>



Implement a class called Pipe, which has a single generic parameter type. The pipe works, such that we can add values into it and values can be taken out. Adding to the pipe happens from one end, and taking out from the other end. In other words, we always remove the value which has been inside the pipe the longest and add to it the newest value. The class should provide a parameterless constructor as well as the following three methods:



- `public void putIntoPipe(T value)` puts an object with a type in accordance with the type parameter given to the the class into the pipe.



- `public T takeFromPipe()` takes out the value, which has been in the pipe the longest. In case there is nothing in the pipe, return `null`. Calling the method returns the value, which has been in the pipe the longest, and removes it from the pipe.



- `public boolean isInPipe()` returns the value `true` if the pipe has values. In case the pipe is empty, it returns the value `false`.


Use ArrayList to implement the internal functionality of the class.


**There are no tests for the class in the exercise template**. Submit the exercise when the following examples work as intended.



```java
Pipe<String> pipe = new Pipe<>();
pipe.putIntoPipe("dibi");
pipe.putIntoPipe("dab");
pipe.putIntoPipe("dab");
pipe.putIntoPipe("daa");
while(pipe.isInPipe()) {
    System.out.println(pipe.takeFromPipe());
}
```



<sample-output>

dibi
dab
dab
daa

</sample-output>



```java
Pipe<Integer> numberPipe = new Pipe<>();
numberPipe.putIntoPipe(1);
numberPipe.putIntoPipe(2);
numberPipe.putIntoPipe(3);

int sum = 0;
while(numberPipe.isInPipe()) {
    sum = sum + numberPipe.takeFromPipe();
}
System.out.println(sum);
System.out.println(numberPipe.takeFromPipe());
```

<sample-output>

6
null

</sample-output>

</programming-exercise>


<quiz id='515dc1a2-e0b9-5f2d-b8b3-cff8ae61b8b1'></quiz>
