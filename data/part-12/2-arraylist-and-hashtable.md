---
path: '/part-12/2-arraylist-and-hashtable'
title: 'ArrayList and hash table'
hidden: false
---



<text-box variant='learningObjectives' name='Learning Objectives'>



- You know how to implement a generic list that has changing size.



- You know of one possible method to implement a class like Java's ArrayList.



- You know how to implement a hash table.



- You are aware of one possible method to implement a class like Java's HashMap.

</text-box>



ArrayList and HashMap are commonly used data structures in programming. We are now going to take a look at their actual implementation. First we'll remind ourselves of how to use an array, after which we're going to build a data structure called `List`, imitating ArrayList. Then we'll make use of the List to implement the data structure `HashTable`.



## A brief recap of arrays



An array is an object that contains a limited number of places for values. The length (or size) of an array is the number of places in it; in other words, how many values can be stored in the array. The size of an array is always predetermined: it is chosen when the array is created, and cannot be changed later.



The array type is defined with square brackets preceded by the type of the elements in the array (typeOfElements[]). An array is created with the `new` call, followed by the type of the elements in that array, square brackets, and the number of elements in the array places inside the square brackets.

```java
int[] numbers = new int[3];
String[] strings = new String[5];
```



The elements of the array are referred to by the indexes. Below we create an integer array of size three, after which we set values to indexes 0 and 2. Then we print those values.

```java
int[] numbers = new int[3];
numbers[0] = 2;
numbers[2] = 5;

System.out.println(numbers[0]);
System.out.println(numbers[2]);
```

<sample-output>

2
5

</sample-output>




Setting a single value to a certain position is done similarly to setting a value to a regular variable, just that when placing the value in an array, you use the index to indicate the position.



To discover the size of an array you can use the public object variable `length` that arrays have. Examining the elements one by one can be accomplished with a for loop, for instance.



```java
int[] numbers = new int[4];
numbers[0] = 42;
numbers[1] = 13;
numbers[2] = 12;
numbers[3] = 7;

System.out.println("There are " + numbers.length + " elements in the array.");

for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}
```



<sample-output>

There are 4 elements in the array.
42
13
12
7

</sample-output>

<quiz id='aecece4d-a170-564c-881b-ef70ccdef697'></quiz>


<programming-exercise name='Sum these for me' tmcname='part12-Part12_03.SumTheseForMe'>


In the class `Program` implement a class method `public static int sum(int[] array, int fromWhere, int toWhere, int smallest, int largest)`. The method must calculate the sum of the elements in the array between the lower and the upper limits. Only numbers smaller or equal to the int largest and larger or equal to the int smallest are added to the sum.



The method must also check that the lower and the upper limit are valid indexes in the array. If the parameter `fromWhere` is smaller than 0, the lower limit becomes 0 instead. Accordingly, if the parameter `toWhere` is larger than the size of the array, the upper limit becomes the last index of the array instead.

```java
int[] numbers = {3, -1, 8, 4};

System.out.println(sum(numbers, 0, 0, 0, 0));
System.out.println(sum(numbers, 0, 0, 0, 10));
System.out.println(sum(numbers, 0, 1, 0, 10));
System.out.println(sum(numbers, 0, 1, -10, 10));
System.out.println(sum(numbers, -1, 999, -10, 10));
```

<sample-output>

0
0
3
3
14

</sample-output>

</programming-exercise>




Arrays can be used in the exact same manner as other variables, so they can be object variables, method parameters, return values of methods, and so on.



A significant portion of generally used data structures use arrays in their internal implementation.

## Lists




Let's examine one way to implement the Java ArrayList data structure. Java ArrayList uses an array. The type of the elements in the array is defined by the type parameter given to the ArrayList. Due to this we can add nearly any type of data to a list. Java List offers multiple methods, but right now `add`, `contains`, `remove` and `get` are most relevant for us.


```java
ArrayList<String> strings = new ArrayList<>();
System.out.println(strings.contains("Hello!"));
strings.add("Hello!");
System.out.println(strings.contains("Hello!"));
strings.remove("Hello!");
System.out.println(strings.contains("Hello!"));
```

<sample-output>

false
true
false

</sample-output>


### Creating a new list



Let's create class `List`. The List has a generic array -- the type of the elements in the array is defined on run time using type parameters.
Let's set the size of the array to `10`. The array is created as type object, and changed to type generic with `(A[]) new Object[10];` -- this is done because Java does not support the call `new A[10];` for now.


```java
public class List<Type> {
    private Type[] values;

    public List() {
        this.values = (Type[]) new Object[10];
    }
}
```


List encapsulates an array. In the beginning, every element in the array contains a `null`-reference.


### Adding values to the list


Let's add the method `public void add(A value)`, which enables adding values to the list. We have to add an int variable to keep track of the first empty index in the array.


```java
public class List<Type> {

    private Type[] values;
    private int firstFreeIndex;

    public List() {
        this.values = (Type[]) new Object[10];
        this.firstFreeIndex = 0;
    }

    public void add(Type value) {
        this.values[this.firstFreeIndex] = value;
        this.firstFreeIndex++; // same as this.firstFreeIndex = this.firstFreeIndex + 1;
    }
}
```


Now we can add values to the list -- or at least we can create a list and call the add method. We cannot test if the values are actually saved to the list yet.

```java
List<String> myList = new List<>();
myList.add("hello");
myList.add("world");
```

### Adding values to a list part 2


There is a small problem with the `add` method. The problem occurs when the following code is run:


```java
List<String> myList = new List<>();
for (int i = 0; i < 11; i++) {
    myList.add("hello");
}
```

<sample-output>
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: 10
    at dataStructures.List.add(List.java:14)
    at dataStructures.Program.main(Program.java:8)
</sample-output>


The size of the list does not grow. One of the benefits of the `ArrayList` class is, that it grows as needed -- programmers do not have to worry about the list getting full.


Let's add the functionality for increasing the size of the List. The size of the List increases if user tries to add a value to a full list. The size of the List is increased by creating a new, larger, array to which the values from the old array are copied to. After this the old array is abandoned and the List starts to use the new array.



The size of the array is determined in Java with the formula `oldSize + oldSize / 2`. Let's use the same formula in our implementation. We'll create a new method `grow` for increasing the size of the array. The method is available only for other methods in the class (it is `private`).

```java
private void grow() {
    int newSize = this.values.length + this.values.length / 2;
    Type[] newValues = (Type[]) new Object[newSize];
    for (int i = 0; i < this.values.length; i++) {
        newValues[i] = this.values[i];
    }

    this.values = newValues;
}
```



The implementation creates a new array whose size is 1.5 times the size of the old array. After this all the elements of the old array are copied into the new one, and finally the value of the object variable `values` is set to the new array. The automatic Java garbage collector removes the old array at some point, now that there are no longer any references to it.



Let's modify the `add` method so that the size of the array grows when needed.

```java
public void add(Type value) {
    if(this.firstFreeIndex == this.values.length) {
        grow();
    }

    this.values[this.firstFreeIndex] = value;
    this.firstFreeIndex++;
}
```


Now we can add almost unlimited amount of elements to the List.

<text-box variant='hint' name='On the effectiveness of this method'>


The method described above copies every element from the old array to the new array. If we would have for example two million elements in an array, we must go through two million elements while copying them.


We will discuss the effectiveness of this method -- and ways to make it more effective -- in the courses Datastructures and Algorithms and Design and analysis of algorithms.

</text-box>


### Checking the existence of a value


Next we'll create the method `public boolean contains(Type value)`, which we use to check whether the List contains a value or not. We will make use of the fact that each Java object -- no matter its type -- inherits the Object class (or is type Object). Due to this, each object has the method `public boolean equals(Object object)`, which we can use to check equality.


The variable `firstFreeIndex` contains the number of elements in the array. We can implement the `contains` method so, that it only checks the indexes in the array which contain a value.

```java
public boolean contains(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (this.values[i].equals(value)) {
            return true;
        }
    }

    return false;
}
```


We can now inspect the elements in the List.

```java
List<String> myList = new List<>();
System.out.println(myList.contains("hello"));
myList.add("hello");
System.out.println(myList.contains("hello"));
```

<sample-output>

false
true

</sample-output>


The method above assumes, that the user will not add a `null` reference to the list, and that the equals method checks that the value given to it as a parameter is not null.

### Removing a value


We can now add values to the List, and check if the List contains a value. Now we will implement the functionality for removing a value from the List.
Let's implement the method `public void remove(Type value)`, which removes *one* value type `value`.


Simple implementation would be as follows:

```java
public void remove(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (value == this.values[i] || this.values[i].equals(value)) {
            this.values[i] = null;
            this.firstFreeIndex--;
            return;
        }
    }
}
```


The above implementation is however problematic, because it leaves "empty" slots to the List, which would lead to the contains method not working.



```java
public void remove(T value) {
    boolean found = false;
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (found) {
            this.values[i - 1] = this.values[i];
        } else if (value == this.values[i] || this.values[i].equals(value)) {
            this.firstFreeIndex--;
            found = true;
        }
    }
}
```


We are not really satisfied with the solution above, because it does too many things at the same time. The method looks for an element and moves elements around. We will split the functionality into two methods: `private int indexOfValue(Type value)`, which searches for the index of the value given to it as a parameter, and `private void moveToTheLeft(int fromIndex)`, which moves the elements above the given index to the left.


First let's implement the method `private int indexOfValue(Type value)`, which searches for the index of the given value. The method returns -1 if the value is not found.

```java
private int indexOfValue(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (this.values[i].equals(value)) {
            return i;
        }
    }

    return -1;
}
```


Then we will implement the method `private void moveToTheLeft(int fromIndex)`, which moves values from the given index one place to the left.

```java
private void moveToTheLeft(int fromIndex) {
    for (int i = fromIndex; i < this.firstFreeIndex - 1; i++) {
        this.values[i] = this.values[i + 1];
    }
}
```


Now we can implement the method `remove` using these two methods.

```java
public void remove(Type value) {
    int indexOfValue = indexOfValue(value);
    if (indexOfValue < 0) {
        return; // not found
    }

    moveToTheLeft(indexOfValue);
    this.firstFreeIndex--;
}
```


<text-box variant='hint' name='On the effectiveness of the method'>


The method describes above copies each element after the removed element one place to the left. Think about the effectiveness of this method when the List is used as a queue.


We will discuss the effectiveness of this method -- and ways to make it more effective -- in the courses Datastructures and algorithms and Design and analysis of algorithms.

</text-box>



The class List now contains some repeated code. The method `contains` is very similiar to the method `indexOfValue`. Let's modify the method `contains` so that it uses the method `indexOfValue`.


```java
public boolean contains(Type value) {
    return indexOfValue(value) >= 0;
}
```


Now we have a List, which has the methods `add`, `contains`,  and `remove`. The List also grows in size when needed. The implementation of the List could of course be improved by for example adding functionality for decreasing the size of the List if the number of values in it decreases.

```java
List<String> myList = new List<>();
System.out.println(myList.contains("hello"));
myList.add("hello");
System.out.println(myList.contains("hello"));
myList.remove("hello");
System.out.println(myList.contains("hello"));
```

<sample-output>

false
true
false

</sample-output>


### Searching from an index


Let's add method `public Type value(int index)`, which returns the value in the given index of the List. If the user searches for a value in an index outside of the Array, `IndexOutOfBoundsException` is thrown.

```java
public Type value(int index) {
    if (index < 0 || index >= this.firstFreeIndex) {
        throw new ArrayIndexOutOfBoundsException("Index " + index + " outside of [0, " + this.firstFreeIndex + "]");
    }

    return this.values[index];
}
```


This method would be easier to use, if the user had information about the indexes of the values. Let's modify the method `indexOfValue(Type value)` so it can be used by everyone, so it is `public` instead of `private`.

```java
public int indexOfValue(Type value) {
    for (int i = 0; i < this.firstFreeIndex; i++) {
        if (this.values[i].equals(value)) {
            return i;
        }
    }

    return -1;
}
```

```java
List<String> myList = new List<>();
System.out.println(myList.contains("hello"));
myList.add("hello");
System.out.println(myList.contains("hello"));
int index = myList.indexOfValue("hello");
System.out.println(index);
System.out.println(myList.value(index));
myList.remove("hello");
System.out.println(myList.contains("hello"));
```

<sample-output>

false
true
0
hello
false

</sample-output>


### Size of the List


Lastly we will add a method for checking the size of the List. The size of the list can be determined by the variable `firstFreeIndex`.


```java
public int size() {
    return this.firstFreeIndex;
}
```


Now we can use a for-loop to go through the elements of the list.


```java
List<String> myList = new List<>();
myList.add("hello");
myList.add("world");

for(int i = 0; i < myList.size(); i++) {
    System.out.println(myList.value(i));
}
```

<sample-output>

hello
world

</sample-output>


<programming-exercise name='List (2 parts)' tmcname='part12-Part12_04.List' nocoins='1'>


Following the example above, implement a class `List`. This exercise has no tests -- test your class using the examples in the material and your own tests.
This exercise is worth 2 points.

</programming-exercise>




## Hash map



Hash map is implemented as an array, in which every element includes a list. The lists contain (key, value) pairs. The user can search from the hash map based on the key, and they can also add new key-value pairs into it. Each key can appear at most once in the hash map.



The functioning of the hash map is based on the hash value of the key. When a new (key, value) pair is stored in a hash map, we calculate a hash value based on the key to be stored. The hash value decides the index of the internal array that will be used for storing. The (key, value) pair is stored in the list that can be found at that index.



Let's sketch out how a hash map functions.


### Key-value pair



Let's start by creating the class `Pair` that represents a key-value pair. We want to make the hash map as general as possible, so the types of the key and the value are determined at run-time. The Pair class contains a key and a value, as well as the related get methods. The generic types K and V are named so after the words 'key' and 'value'.




```java
public class Pair<K, V> {

    private K key;
    private V value;

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey() {
        return key;
    }

    public V getValue() {
        return value;
    }

    public void setValue(V value) {
        this.value = value;
    }
}
```



Creating key-value pairs is straightforward.



```java
Pair<String, Integer> pair = new Pair<>("one", 1);
System.out.println(pair.getKey() + " -> " + pair.getValue());
```



<sample-output>

one -> 1

</sample-output>




### Creating a hash map



A hash map contains an array of lists. Each value on the list is a pair (described in the previous section) that contains a key and a value. A hash map also knows the number of the values. Here we have at our disposal the previously created class `List`.




```java
public class HashMap<K, V> {

    private List<Pair<K, V>>[] values;
    private int firstFreeIndex;

    public HashMap() {
        this.values = new List[32];
        this.firstFreeIndex = 0;
    }
}
```




### Retrieving a value



Let's implement a method called `public V get(K key)`. It can be used to search for a value based on a key.



The method begins by calculating a hash value for the key, and using it to figure out which is the relevant index of the internal array of the hash map. The hash value is calculated with the `hashCode` method that each object has. Then modulo (remainder of division operation) is used for ensuring that the index stays within the size boundaries of the internal array.



If there is no list in the calculated index, no key-value pairs have been added to that index. This means that there are no key-value pairs with this key that have been stored. In this case we'll return the `null` reference. Otherwise, the program goes through the list at the index, and we compare the parameter key to the key of every key-value pair on that list. If some of the keys matches the parameter key, the method returns the value of that key-value pair. Otherwise we cannot find a suitable key (and related value), so the method returns the value null.




```java
public V get(K key) {
    int hashValue = Math.abs(key.hashCode() % this.values.length);
    if (this.values[hashValue] == null) {
        return null;
    }

    List<Pair<K, V>> valuesAtIndex = this.values[hashValue];

    for (int i = 0; i < valuesAtIndex.size(); i++) {
        if (valuesAtIndex.value(i).getKey().equals(key)) {
            return valuesAtIndex.value(i).getValue();
        }
    }

    return null;
}
```


<text-box variant='hint' name='Why not implement hash map as a list?'>



The main principle of the hash map is that the key-value pairs are divided into small sets with the help of hash values. In this case a search based on key demands only going through a very small number of key-value pairs -- assuming that the hash values are calculated in a sensible manner.



If the hash value is always the same -- e.g. 1 -- the internal implementation of a hash map is similar to a list -- all the values are on the same list. If the hash values are sufficiently random, the different values are as evenly distributed to the different lists as possible.



Hash maps also grow the size of their internal array if the number of values becomes large enough (typically 75% of the size of the array). Typically a hash map that contains millions of key-value pairs only contains a few key-value pairs in each index. The practical consequence is that discovering if a key-value pair exists, we only need to calculate the hash value and examine a few objects -- this is very significantly faster than going through a single list that contains the entirety of stored values.

</text-box>




### Adding to hash map



Let's implement the first version of the method `public void add(K key, V value)`, which is used to add values to the hash map. In this version we are not going to increase the size of the internal array when new values are added to the hash map.



The method first calculates the hash value for the key, and uses it to determine the suitable index in the internal array. If there is no value in that index, we create a list into that index. After this the method goes through the list at the index, and looks for a key-value pair whose key matches the key of the key-value pair to be added. If the matching key is found, the value related to it is updated to match the new value. Otherwise the method adds a new key-value pair in the list -- in which case the number of stored values is also incremented by one.




```java
public void add(K key, V value) {
    int hashValue = Math.abs(key.hashCode() % values.length);
    if (values[hashValue] == null) {
        values[hashValue] = new List<>();
    }

    List<Pari<K, V>> valuesAtIndex = values[hashValue];

    int index = -1;
    for (int i = 0; i < valuesAtIndex.size(); i++) {
        if (valuesAtIndex.value(i).getKey().equals(key)) {
            index = i;
            break;
        }
    }

    if (index < 0) {
        valuesAtIndex.add(new Pair<>(key, value));
        this.firstFreeIndex++;
    } else {
        valuesAtIndex.value(index).setValue(value);
    }
}
```



The method is quite complex, so let's divide it into smaller parts. The first part is responsible for finding the list related to the key, and the second part is responsible for finding the key on that list.




```java
private List<Pair<K, V>> getListBasedOnKey(K key) {
    int hashValue = Math.abs(key.hashCode() % values.length);
    if (values[hashValue] == null) {
        values[hashValue] = new List<>();
    }

    return values[hashValue];
}

private int getIndexOfKey(List<Pair<K, V>> myList, K key) {
    for (int i = 0; i < myList.size(); i++) {
        if (myList.value(i).getKey().equals(key)) {
            return i;
        }
    }

    return -1;
}
```



Now we can write a somewhat clearer implementation of the method `public void add(K key, V value)`




```java
public void add(K key, V value) {
    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
    int index = getIndexOfKey(valuesAtIndex, key);

    if (index < 0) {
        valuesAtIndex.add(new Pair<>(key, value));
        this.firstFreeIndex++;
    } else {
        valuesAtIndex.value(index).setValue(value);
    }
}
```




### Adding to hash table, part 2



The way of adding to a hash table that was described above works partly. The greatest fault in the functionality is that the size of the internal array is not increased when the number of values grows too large. Let's add a growing functionality to the program that doubles the size of the internal array of the hash map. The growing operation should also place each value in the hash map into the newly created bigger array.



Let's sketch the beginning of the growing functionality. The responsible method should create a new array whose size is double that of the old array. After this it goes through the old array, index by index. The encountered key-value pairs are copied into the new array. Finally, the old array is replaced with the new one.



Below there is a first version of how the method should work. We haven't implemented the copying yet.



```java
private void grow() {
    // crete a new array
    List<Pair<K, V>>[] newValues = new List[this.values.length * 2];

    for (int i = 0; i < this.values.length; i++) {
        // copy the values of the old array into the new one

    }

    // replace the old array with the new one
    this.values = newValues;
}
```



Then let's begin to create a method that copies the list of values at one index of the old array into the new one. When copying, the location of each key-value pair is recalculated for the new array -- this is done because the size of the internal array grows, and we want to distribute all the key-value pairs in that array as evenly as possible.




```java
private void copy(List<Pair<K, V>>[] newArray, int fromIdx) {
    for (int i = 0; i < this.values[fromIdx].size(); i++) {
        Pair<K, V> value = this.values[fromIdx].value(i);

        int hashValue = Math.abs(value.getKey().hashCode() % newArray.length);
        if(newArray[hashValue] == null) {
            newArray[hashValue] = new List<>();
        }

        newArray[hashValue].add(value);
    }
}
```

<quiz id='bf26ed5d-6f6b-5b04-a253-a0c14de7615d'></quiz>



Now you can call the copy method from the grow method




```java
private void grow() {
    // create a new array
    List<Pair<K, V>>[] newArray = new List[this.values.length * 2];

    for (int i = 0; i < this.values.length; i++) {
        // copy the values of the old array into the new one
        copy(newArray, i);
    }

    // replace the old array with the new
    this.values = newArray;
}
```



Finally, let's add the growing functionality to be a part of the `add` method. We want to grow the size of the hash map if the number of key-value pairs in it is greater than 75% of the size of the internal array.




```java
public void add(K key, V value) {
    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
    int index = getIndexOfKey(valuesAtIndex, key);

    if (index < 0) {
        valuesAtIndex.add(new Pair<>(key, value));
        this.firstFreeIndex++;
    } else {
        valuesAtIndex.value(index).setValue(value);
    }

    if (1.0 * this.firstFreeIndex / this.values.length > 0.75) {
        grow();
    }
}
```




### Remove




Let's give the hash map the functionality to remove a key-value pair based on key. The removal functionality returns null if the value cannot be found, and otherwise it will remove the value that is paired with the key to be removed.



We can take advantage of the method we've already implemented in the removing method. Explain to yourself (out loud) how the method described below concretely works.



```java
public V remove(K key) {
    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
    if (valuesAtIndex.size() == 0) {
        return null;
    }

    int index = getIndexOfKey(valuesAtIndex, key);
    if (index < 0) {
        return null;
    }

    Pair<K, V> pair = valuesAtIndex.value(index);
    valuesAtIndex.remove(pair);
    return pair.getValue();
}
```

<programming-exercise name='Hash map (3 parts)' tmcname='part12-Part12_05.HashMap' nocoins='1'>



Implement the class HashMap in the exercise base, following along the lines of the previous example. Unlike the example, implement the class so that it uses the ready-made Java class ArrayList in its internal implementation. There are no tests in the exercise base -- test its functionality with the examples in the material, and with your own experimentation. This exercise is worth three points.

</programming-exercise>




## On search performance



Let's compare the performance of searching from a list or a hash map. To evaluate performance we can use the `System.nanotime()` method and the value it returns, which represents the time as nanoseconds. The program first creates a hash map and a list, each containing a million elements, after which a thousand randomly chosen values are chosen from both. Roughly 50 % of the values are found with both structures.



```java
List<String> myList = new List<>();
HashMap<String, String> hashMap = new HashMap<>();

for (int i = 0; i < 1000000; i++) {
    myList.add("" + i);
    hashMap.add("" + i, "" + i);
}

List<String> elements = new List<>();
Random randomizer = new Random();
for (int i = 0; i < 1000; i++) {
    elements.add("" + randomizer.nextInt(2000000));
}

long listSearchStartTime = System.nanoTime();
for (int i = 0; i < elements.size(); i++) {
    myList.contains(elements.value(i));
}
long listSearchEndTime = System.nanoTime();

long hashMapSearchStartTime = System.nanoTime();
for (int i = 0; i < elements.size(); i++) {
    hashMap.hae(elements.value(i));
}
long hashMapSearchEndTime = System.nanoTime();


long listSearch = listSearchEndTime - listSearchStartTime;
System.out.println("List: the search took about " + listSearch / 1000000 + " milliseconds (" +
    listSearch + " nanoseconds.)");

long hashMapSearch = hashMapSearchEndTime - hashMapSearchStartTime;
System.out.println("Hash map: the search took about " + hashMapSearch / 1000000 +
    " milliseconds (" + hashMapSearch + " nanoseconds.)");
```

<sample-output>

List: the search took about 6284 milliseconds (6284420580 nanoseconds.)
Hash map: the search took about 0 milliseconds (805106 nanoseconds.)

</sample-output>



*The list and hash map that are described in this chapter do have some differences from the readymade tools we use elsewhere in the course. The data structures offered by the programming language have more different kinds of optimizations -- other courses go more in detail with these specifics. For the purposes of this course it's enough to know how to use the data structures and to have some idea of the performance differences and when they are suitable to use.*
