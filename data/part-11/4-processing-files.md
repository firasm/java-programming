---
path: '/part-11/4-processing-files'
title: 'Processing files'
hidden: false
---




<text-box variant='learningObjectives' name='Learning Objectives'>



- You will refresh your memories of reading from files.



- You will be able to write to a file.

</text-box>



We have already learned some strategies to read text files. If your memories of the subject are hazy, take a look at the relevant parts of Part 4 of the course material.



Next, let's take a look at writing data to files. The [PrintWriter](https://docs.oracle.com/javase/8/docs/api/java/io/PrintWriter.html) class offers the functionality to write to files. The constructor of the `PrintWriter` class receives as its parameter a string that represents the location of the target file.




```java
PrintWriter writer = new PrintWriter("file.txt");
writer.println("Hello file!"); //writes the string "Hello file!" and line change to the file
writer.println("More text");
writer.print("And a little extra"); // writes the string "And a little extra" to the file without a line change
writer.close(); //closes the file and ensures that the written text is saved to the file
```



In the example above, we write to the file "file.txt" the string "Hello file!", followed by a line change and some additional text. Take notice that when writing to the file the method `print` does not add line changes, and you have to add them yourself. In contrast, the method `println` adds a new line change at the end of the parameter string it receives.



The constructor of the `PrintWriter` class might throw an exception that must be either handled or thrown so that it is the responsibility of the calling method. Here is what a method that receives as its parameters a file name and the text contents to write into it could look like.




```java
public class Storer {

    public void writeToFile(String fileName, String text) throws Exception {
        PrintWriter writer = new PrintWriter(fileName);
        writer.println(text);
        writer.close();
    }
}
```



In the `writeToFile` method above we begin by creating a `PrintWriter` object. It writes data the the file that is located at the path that the string `fileName` indicates. After this, we write the text to the file by calling the `println` method. The possible exception that the constructor throws has to be handled with a `try-catch` block or the handling responsibility has to be transferred elsewhere. In the `writeToFile` method, the responsibility to handle the exception is placed on the method that calls `writeToFile`.



Let's create a `main` method that calls the `writeToFile` of a `Storer` object. There is nothing to force the `main` method to handle the exception -- it, too, can state that it might throw an exception by adding `throws Exception` to the method definition.




```java
public static void main(String[] args) throws Exception {
    Storer storer = new Storer();
    storer.writeToFile("diary.txt", "Dear diary, today was a good day.");
}
```



By calling the method above we create a file called "diary.txt" and write the text "Dear diary, today was a good day." into it. If the file already exists, the earlier contents are erased when we store the new text.



It is also possible to handle files in a way that adds the new text after the existing content. In that case, you might want to use the [FileWriter](https://docs.oracle.com/javase/8/docs/api/java/io/FileWriter.html) class.

<br/>

<quiz id="9cd37f37-26c8-52ab-bf84-1f025eee8588"></quiz>


<programming-exercise name='Saveable Dictionary (4 parts)' nocoins='true' tmcname='part11-Part11_13.SaveableDictionary'>



In this exercise we will extend the dictionary, so that it can read words from a file and write words to a file.
The dictionary must also be able to translate both from Finnish to another language and from another language to Finnish (in this exercise we ignore the fact that some words might be written the same in Finnish and in other languages). Your mission is to create a dictionary in the class `SaveableDictionary`. Implement the class in the package `dictionary`.



<h2>The basic functionality</h2>


For the dictionary, implement a constructor which takes no parameters, and the following methods:




 - `public void add(String words, String translation)` adds a word to the dictionary. Every word has just one translation, and if the same word is added for the second time, nothing happens.

 - `public String translate(String word)` returns the translation for the given word. If the word is not in the dictionary, returns null.



In this phase, the dictionary should work as follows:


```java
SaveableDictionary dictionary = new SaveableDictionary();
dictionary.add("apina", "monkey");
dictionary.add("banaani", "banana");
dictionary.add("apina", "apfe");

System.out.println(dictionary.translate("apina"));
System.out.println(dictionary.translate("monkey"));
System.out.println(dictionary.translate("ohjelmointi"));
System.out.println(dictionary.translate("banana"));
```



Prints

<sample-output>

monkey
apina
null
banaani

</sample-output>


As can be seen from the output, after a translation has been added to the dictionary, it can translate the word to and from Finnish.



<b>NB:</b> the methods `add` and `translate` do not read from a file or write to a file! The constructor does not use a file either.



<h2>Deleting words</h2>


Add the dictionary method `public void delete(String word)` which deletes the given word and its translation from the dictionary.


It might be worth reviewing the material concerning deleting objects from an ArrayList from previous weeks.



<b>NB:</b> the method `delete` does not write to a file.


In this phase, the dictionary should work as follows:

```java
SaveableDictionary dictionary = new SaveableDictionary();
dictionary.add("apina", "monkey");
dictionary.add("banaani", "banana");
dictionary.add("ohjelmointi", "programming");
dictionary.delete("apina");
dictionary.delete("banana");

System.out.println(dictionary.translate("apina"));
System.out.println(dictionary.translate("monkey"));
System.out.println(dictionary.translate("banana"));
System.out.println(dictionary.translate("banaani"));
System.out.println(dictionary.translate("ohjelmointi"));
```


Prints

<sample-output>

null
null
null
null
programming

</sample-output>



Deleting also works both ways: Both the word and its translation are removed if either the word or the translation are deleted.



<h2>Reading from file</h2>



Make a constructor `public SaveableDictionary(String file)` and method `public boolean load()`, which loads the dictionary from the file given to the constructor as a parameter. If the program is unable to open the file or read from it, the method returns false, otherwise it returns true.


<b>NB:</b> the constructor only tells the dictionary the name of the file to load the dictionary from. The constructor does not read the file. Only the method `load` reads the file.


In the dictionary file, one line contains a word and its translation separated by ":".
The exercise template contains a file `words.txt` which contains the following:

<sample-output>

apina:monkey
alla oleva:below
olut:beer

</sample-output>


Read the dictionary file line by line with the method `nextLine`. You can split a line using the String method `split` like so:




```java
Scanner fileReader = new ...
while (fileReader.hasNextLine()) {
    String line = fileReader.nextLine();
    String[] parts = line.split(":");   // split the line based on the ':' character

    System.out.println(parts[0]);     // part of line before :
    System.out.println(parts[1]);     // part of line after :
}
```


The dictionary can be used as follows:

```java
SaveableDictionary dictionary = new SaveableDictionary("words.txt");
boolean wasSuccessful = dictionary.load();

if (wasSuccessful) {
    System.out.println("Successfully loaded the dictionary from file");
}

System.out.println(dictionary.translate("apina"));
System.out.println(dictionary.translate("ohjelmointi"));
System.out.println(dictionary.translate("alla oleva"));
```

Prints

<sample-output>

Successfully loaded the dictionary from file
monkey
null
below

</sample-output>



<h2>Saving to a file</h2>


Create the method `public boolean save()`, which saves the dictionary to the file given to the dictionary as a parameter to the constructor. If the program cannot save to the file, the method returns false. Otherwise it returns true. The dictionary files have to be saved in the form described above, so the program can read the files it has written.


<b>NB:</b> Only the method `save` writes to the file.


**NB:** Even though the dictionary can translate both ways, the dictionary file should only contain one way. So if the dictionary, for example, knows that *computer = tietokone*, the file should contain:


<sample-output>

tietokone:computer

</sample-output>

or

<sample-output>

computer:tietokone

</sample-output>


but not both!


It is best to handle the saving to, such that the whole dictionary is written on top of the previously saved version, so it might not be the best to use the `append` method described in the material.


The final version of the dictionary works as follows:

```java
SaveableDictionary dictionary = new SaveableDictionary("words.txt");
dictionary.load();

// use the dictionary

dictionary.save();
```


In the beginning, the dictionary is loaded from a file, and, in the end, it is saved back to the file, so that changes made to the dictionary are kept for the next time the dictionary is used.

</programming-exercise>
