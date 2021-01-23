---
path: "/part-1/1-starting-programming"
title: "Getting started with programming"
hidden: false
---

<text-box variant='learningObjectives' name='Learning Objectives'>



- Become familiar with the 'NetBeans with TMC' development environment used in this course.



- Learn to download and submit programming exercises.

</text-box>

<quiz id="ae09155d-d056-5662-9611-f322cd50959d"></quiz>



Modern programming is practically always done in an IDE (integrated development environment). An IDE contains a set of useful tools for the programmer. It does not create the program by itself, but it can give hints about common mistakes in the code, and help the programmer understand the structure of the program.



The IDE we use on this course is called [NetBeans](https://netbeans.apache.org), and we use it with Test My Code plugin.



To start the course you need (1) a MOOC.fi account, which you can create at the top of this course material, (2) Java (Java JDK), and (3) Netbeans with TMC development environment (in the future TMC).

All of these can be installed with the following instructions. **When following the instructions, select "MOOC" as your organization and "Java Programming I" as your course.** Installation instructions for Java and NetBeans can be found here: [Java and NetBeans installation guide](https://www.mooc.fi/en/installation/netbeans)



Once you have created the user account and installed Java and TMC, watch the video below. The video demonstrates what happens when 'Netbeans with TMC' is opened for the first time. We select the organization and course, and do the first programming exercise. In the video, the user selects the 2019 course, but you should remember to **select Java Programming I as your course**!

<youtube id="zvE8XA8D0gE"></youtube>



This is the first programming exercise of this course. In this exercise, you'll familiarize yourself with the development environment.

<programming-exercise name='Sandbox' tmcname='part01-Part01_01.Sandbox'>



The exercise template already contains the following template of code:

```java
public class Sandbox {
    public static void main(String[] args) {
        // Write your program here

    }
}
```



The line "// Write your program here" is a _line comment_, and the computer will ignore it when executing the program.



First, submit the exercise template to be checked on the server without changing anything. In this way, you practice submitting an exercise. You can submit an exercise by selecting the TMC menu within TMC, and then "Submit".



Once you have submitted the exercise, try following the video above to add a print command to the program and to execute the program (by pressing the play - button). When you've been able to get your program to print some text (this text can be anything), submit the exercise to the server once more.



You should use this sandbox exercise to try out different things later on. When you encounter something new in the material, try it out in the sandbox first, and then solve the exercises on the topic.

</programming-exercise>

## Programmers Write Source Code



Programming is designing and implementing software. The functionality is implemented based on the wishes and requirements of the users and the customers. Programs are typically implemented (i.e., written or "coded") in a programming language meant to be written and read by humans.



There are hundreds of programming languages out there, and this course focuses one of them. The language used in this course is [Java](<https://en.wikipedia.org/wiki/Java_(programming_language)>), which is one of the most commonly-used programming languages in the world. Learning Java also makes it easier to pick up other programming languages.



Programming languages, such as Java, have many commands built-in that a programmer uses when developing software. This makes programming easier as you don't need to implement everything from scratch. For example, programming languages typically have built-in options available for different menus and views used for making graphical user interfaces. Indeed, a large part of programming is making use of available functions and tools in solving problems -- this, however, requires programming experience, which you only gain by programming.



The "code" you write is called **source code**. Source code consists of statements and expressions, which are read line by line from top to bottom, and from left to right. For example, to print "Hello world", you can use the pre-defined Java command `System.out.println()`, to which you need to provide the text inside the parentheses that you'd like to be printed:

```java
System.out.println("Hello World");
```



The above statement is pre-built into Java, and it's used for printing a string. The statement tells the computer to output the string that's been provided to it, placed between the quotation marks. The statement ends with a semicolon `;`.



Java programs require a certain frame around the code to work. Don't worry if it appears intimidating, it'll become very familiar over this course. Although you might not be able to make sense of the surrounding frame of code, you could probably guess what this following program does based on the print statement described above.




You'll find the first quiz of the course below. You can answer the quizzes right here in the course material.

<quiz id="398ad482-25d6-5817-8d95-375c4d15452c"></quiz>
