---
path: '/part-8/4-grouping-data-using-hash-maps'
title: 'Grouping data using hash maps'
hidden: false
---


<text-box variant='learningObjectives' name='Learning Objectives'>

- You know how to use a list as a hash map's value
- You know how to categorize data using a hash map

</text-box>


A hash map has at most one value per each key. In the following example, we store the phone numbers of people into the hash map.



```java
HashMap<String, String> phoneNumbers = new HashMap<>();
phoneNumbers.put("Pekka", "040-12348765");

System.out.println("Pekka's Number " + phoneNumbers.get("Pekka"));

phoneNumbers.put("Pekka", "09-111333");

System.out.println("Pekka's Number " + phoneNumbers.get("Pekka"));
```


<sample-output>

Pekka's number: 040-12348765
Pekka's number: 09-111333

</sample-output>


What if we wanted to assign multiple values ​​to a single key, such as multiple phone numbers for a given person?

Since keys and values ​​in a hash map can be any variable, it is also possible to use lists as values in a hash map. You can add more values ​​to a single key by attaching a list to the key. Let's change the way the numbers are stored in the following way:


```java
HashMap<String, ArrayList<String>> phoneNumbers = new HashMap<>();
```


Each key of the hash map now has a list attached to it. Although the new command creates a hash map, the hash map initially does not contain a single list. They need to be created separately as needed.


```java
HashMap<String, ArrayList<String>> phoneNumbers = new HashMap<>();

// let's initially attatch an empty list to the name Pekka
phoneNumbers.put("Pekka", new ArrayList<>());

// and add a phone number to the list at Pekka
phoneNumbers.get("Pekka").add("040-12348765");
// and let's add another phone number
phoneNumbers.get("Pekka").add("09-111333");

System.out.println("Pekka's numbers: " + phoneNumbers.get("Pekka"));
```


<sample-output>

Pekka's number: [040-12348765, 09-111333]

</sample-output>


We define the type of the phone number as  `HashMap<String, ArrayList<String>>`. This refers to a hash map that uses a string as a key and a list containing strings as its value. As such, the values added to the hash map are concrete lists.


```java
// let's first add an empty ArrayList as the value of Pekka
phoneNumbers.put("Pekka", new  ArrayList<>());

// ...
```


We can implement, for instance, an exercise point tracking program in a similar way. The example below outlines the `TaskTracker` class, which involves user-specific tracking of points from tasks. The user is represented as a string and the points as integers.


```java
public class TaskTracker {
    private HashMap<String, ArrayList<Integer>> completedExercises;

    public TaskTracker() {
        this.completedExercises = new HashMap<>();
    }

    public void add(String user, int exercise) {
        // an empty list has to be added for a new user if one has not already been added
        this.completedExercises.putIfAbsent(user, new ArrayList<>());

        // let's first retrieve the list containing the exercises completed by the user and add to it
        ArrayList<Integer> completed = this.completedExercises.get(user);
        completed.add(exercise);

        // the previous would also work without the helper variable as follows
        // this.completedExercises.get(user).add(exercise);
    }

    public void print() {
        for (String name: completedExercises.keySet()) {
            System.out.println(name + ": " + completedExercises.get(name));
        }
    }
}
```


```java
TaskTracker tracker = new TaskTracker();
tracker.add("Ada", 3);
tracker.add("Ada", 4);
tracker.add("Ada", 3);
tracker.add("Ada", 3);

tracker.add("Pekka", 4);
tracker.add("Pekka", 4);

tracker.add("Matti", 1);
tracker.add("Matti", 2);

tracker.print();
```

<sample-output>

Matti: [1, 2]
Pekka: [4, 4]
Ada: [3, 4, 3, 3]

</sample-output>

<programming-exercise name='Dictionary of many translations' tmcname='part08Part08_14.DictionaryOfManyTranslations'>



Your assignment is to create the class `DictionaryOfManyTranslations`. In it can be stored one or more translations for each word. The class is to implement the following methods:



- `public void add(String word, String translation)` adds the translation for the word and preserves the old translations.



- `public ArrayList<String> translate(String word)` returns a list of the translations added for the word. If the word has no translations, the method should return an empty list.



- `public void remove(String word)` removes the word and all its translations from the dictionary.



It's probably best to add the translations to an object variable that is of the type `HashMap<String, ArrayList<String>>`



An example:



```java
DictionaryOfManyTranslations dictionary = new DictionaryOfManyTranslations();
dictionary.add("lie", "maata");
dictionary.add("lie", "valehdella");

dictionary.add("bow", "jousi");
dictionary.add("bow", "kumartaa");

System.out.println(dictionary.translate("lie"));
dictionary.remove("bow");
System.out.println(dictionary.translate("bow"));
```



<sample-output>

[maata, valehdella]
[]

</sample-output>

</programming-exercise>


<programming-exercise name='Storage facility (2 parts)' tmcname='part08-Part08_15.StorageFacility'>




<h2>Adding items and examining contents</h2>



Your task is creating a class called `StorageFacility` that can be used to keep track of storage units and their contents. The class is to implement the following methods:



- `public void add(String unit, String item)` adds the parameter item to the storage unit that is also given as a parameter.



- `public ArrayList<String> contents(String storageUnit)` returns a list that contains all the items in the storage unit indicated by the parameter. If there is no such storage unit or it contains no items, the method should return an empty list.



Here's an example:



```java
StorageFacility facility = new StorageFacility();
facility.add("a14", "ice skates");
facility.add("a14", "ice hockey stick");
facility.add("a14", "ice skates");

facility.add("f156", "rollerblades");
facility.add("f156", "rollerblades");

facility.add("g63", "six");
facility.add("g63", "pi");

System.out.println(facility.contents("a14"));
System.out.println(facility.contents("f156"));
```



<sample-output>

[ice skates, ice hockey stick, ice skates]
[rollerblades, rollerblades]

</sample-output>




<h2>Listing the units and removing from unit</h2>



- Now the class `StorageFacility` contains the functionality to add an item to a storage unit and to list the contents of a unit. Next add the possibilities to remove an item from a storage unit and to list all the units.



- `public void remove(String storageUnit, String item)` removes the given item from the given storage unit. NB! Only removes one item -- if there are several items with the same name, the rest still remain. If the storage unit would be empty after the removal, the method also removes the unit.



- `public ArrayList<String> storageUnits()` returns the names of the storage units as a list. NB! Only the units that contain items are listed.



An example:



```java
StorageFacility facility = new StorageFacility();
facility.add("a14", "ice skates");
facility.add("a14", "ice hockey stick");
facility.add("a14", "ice skates");

facility.add("f156", "rollerblades");
facility.add("f156", "rollerblades");

facility.add("g63", "six");
facility.add("g63", "pi");

facility.remove("f156", "rollerblades");

System.out.println(facility.contents("f156"));

facility.remove("f156", "rollerblades");

System.out.println(facility.storageUnits());
```



<sample-output>

[rollerblades]
[a14, g63]

</sample-output>



The order of the storage units in the output may be different from this example.

</programming-exercise>
