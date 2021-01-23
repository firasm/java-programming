---
path: '/part-2/1-problems-and-patterns'
title: 'Recurring problems and patterns to solve them'
hidden: false
---

<text-box variant='learningObjectives' name='Learning Objectives'>



- You recognize that certain sub-problems, such as reading input or calculations, recur in programs.
- You're aware of solution models to certain sub-problems.
- You practice combining solution patterns used on sub-problems to solve broader ones.

</text-box>



The same small problems, or "sub-problems", reappear in programs time after time: "Read input from the user", "Calculate the sum of values", and so forth.



Let's look at a few sub-problems and patterns for solving them.




## Reading User Input



The solution pattern for programming tasks involving reading user input is straightforward. If the program needs to read from the user, a Scanner helper tool is created for the task. The Scanner is created in the main method after the line `public static void main(String[] args) {`. To use the Scanner, it needs to be made available in the program through the statement `import java.util.Scanner;`, which comes before the class definition (`public class ...`). Importing the Scanner tool makes it available for the program.



```java
// Making the scanner available in the program
import java.util.Scanner;

public class Program {
    public static void main(String[] main) {
        // Creating the scanner
        Scanner reader = new Scanner(System.in);

        // Examples of reading different types of user input
        String text = reader.nextLine();
        int number = Integer.valueOf(reader.nextLine());
        double numberWithDecimals = Double.valueOf(reader.nextLine());
        boolean trueOrFalse = Boolean.valueOf(reader.nextLine());

    }
}
```

<quiz id="b7705d46-9213-4ec2-b144-f27d14cd7287"></quiz>




## Calculating



We quite often need to calculate something in a program, such as an average or a sum. The solution pattern to solve such problems is as follows.



1. Define the inputs required for the calculation and declare variables for them. Input refers to the values used in the calculation. You can typically identify the type of inputs from the problem description.
2. Identify the operation needed, and declare a variable for the result of the calculation. Perform the calculation using the inputs, and assign the result to the variable that was reserved for it. The type of the result can also usually be identified from the problem description.
3. Once the calculation is done, do something with its result. This can mean printing the result of a computation, or, for example, using it in calculating an average by dividing a sum of the inputs by their count.



For example, the solution pattern for the problem _Create a program to calculate the sum of two integers_ is the following.



```java
// Identifying the input values and declaring the variables for them
int first = 1;
int second = 2;

// Identifying the operation and declaring a variable for the result
int sum = first + second;

// printing the result of the calculation
System.out.println("The sum of " + first + " and " + second + " is " + sum);
```



A program that both reads and calculates combines both of these patterns. One that calculates the product of two integers provided by the user looks like this:



```java
// Making the scanner available in the program
import java.util.Scanner;

public class Program {
    public static void main(String[] main) {
        // Creating the scanner
        Scanner reader = new Scanner(System.in);

        // Identifying the input values and declaring the variables for them
        int first = 1;
        int second = 2;

        // Assigning the user input to the variables
        first = Integer.valueOf(reader.nextLine());
        second = Integer.valueOf(reader.nextLine());

        // Identifying the operation and declaring a variable for the result
        int product = first * second;

        // Printing the result of the operation
        System.out.println("The product of " + first + " and " + second + " is " + product);

    }
}
```



In the example above, the program has been implemented so that the variables are declared first after which values are read into them. Variable declaration and the reading of values into them can also be combined into one.



```java
// Making the Scanner available to the program
import java.util.Scanner;

public class Program {
    public static void main(String[] main) {
        // Creating the Scanner
        Scanner reader = new Scanner(System.in);

        // Declaring the variables and assigning user input to them
        int first = Integer.valueOf(reader.nextLine());
        int second = Integer.valueOf(reader.nextLine());

        // Identifying the operation and declaring a variable for the result
        int product = first * second;

        // Printing the result of the operation
        System.out.println("The product of " + first + " and " + second + " is " + product);

    }
}
```

<programming-exercise name='Squared' tmcname='part02-Part02_01.Squared'>



Write a program that reads an integer from the user and prints the square of the given integer, i.e. the integer multiplied by itself.

<sample-output>

**4**
16

</sample-output>

<sample-output>

**-3**
9

</sample-output>

<sample-output>

**5**
25

</sample-output>

</programming-exercise>


<programming-exercise name='Square root of sum' tmcname='part02-Part02_02.SquareRootOfSum'>


Write a program that reads two integers from the user and prints the square root of the sum of these integers. The program does not need to work with negative values.


You can calculate the square root of an integer with the command `Math.sqrt` like this:



```java
int number = 42;
double squareRoot = Math.sqrt(number);
System.out.println(squareRoot);
```


Here are a few examples:

<sample-output>

**1**
**0**
1

</sample-output>

<sample-output>

**5**
**4**
3

</sample-output>

<sample-output>

**1**
**35**
6

</sample-output>


</programming-exercise>




## Conditional Logic



Problems often contain some conditional logic. In these instances we use conditional statements. A conditional statement starts with an `if` command followed by an expression in parentheses. The expression evaluates to either true or false. If it evaluates true, the following block delimited by curly brackets gets executed.



```java
// if the value is greater than five
if (value > 5) {
    // then...
}
```



A program that prints "ok" if the value of the variable is greater than `42`, and otherwise prints "not ok" looks like this:



```java
int value = 15;
if (value > 42) {
    System.out.println("ok");
} else {
    System.out.println("not ok")
}
```



You can also chain together multiple conditions. In such a case, the problem takes the form "if a, then b; else if c, then d; else if e, then f; otherwise g". The chain consists of an `if`-statement followed by `else if`-statements, each containing its own expression and a block.



```java
// if the value is greater than five
if (value > 5) {
    // functionality when value is greater than five
} else if (value < 0) { //
    // functionality when value is less than zero
    // and the value IS NOT larger than five
} else {  // otherwise
    // functionality otherwise
}
```

<text-box variant="hint" name="System.out.print">



The quiz below uses `System.out.print` for printing. It works exactly like `System.out.println`, but it doesn't add a line break to the end of the output.

</text-box>

<quiz id="cf7bfa61-dc45-56f2-8913-fa8671dbfc4d"></quiz>




Conditional logic can be combined with other patterns used for problem solving. Let's look into a problem "Read two integers from the user. If the sum of the integers is over 100, print `too much`. If the sum is less than 0, print `too little`. Otherwise, print `ok`. The program below combines reading, calculating and conditional functionality.



```java
// Bringing Scanner to the program's use
import java.util.Scanner;

public class Program {
    public static void main(String[] main) {
        // Creating the scanner
        Scanner reader = new Scanner(System.in);

        // Declaring the variables and assigning user input to them
        int first = Integer.valueOf(reader.nextLine());
        int second = Integer.valueOf(reader.nextLine());

        // Identifying the operation and declaring variable for the result
        int sum = first + second;

        // Doing something with the result. In this case
        // the result is used in the conditional operations.

        if (sum > 100) { // if the sum is over 100
            System.out.println("too much");
        } else if (sum < 0) { // if the sum is less than 0
            System.out.println("too little");
        } else { // otherwise
            System.out.println("ok");
        }
    }
}
```



<programming-exercise name='Absolute Value' tmcname='part02-Part02_03.AbsoluteValue'>



Write a program that reads an integer from the user. If the number is less than 0, the program prints the given integer multiplied by -1. In all other cases, the program prints the number itself. A few examples of how the program's expected to function are shown below:

<sample-output>

**-3**
3

</sample-output>

<sample-output>

**2**
2

</sample-output>

<sample-output>

**-5**
5

</sample-output>

</programming-exercise>


<programming-exercise name='Comparing Numbers' tmcname='part02-Part02_04.ComparingNumbers'>



Write a program that reads two integers from the user. If the first number is greater than the second, the program prints "(first) is greater than (second)." If the first number is less than the second, the program prints "(first) is smaller than (second)." Otherwise, the program prints "(first) is equal to (second)." The (first) and (second) should always be replaced with the actual numbers that were provided by the user.



A few examples of the expected behaviour:

<sample-output>

**8**
**4**
8 is greater than 4.

</sample-output>

<sample-output>

**-3**
**5**
-3 is smaller than 5.

</sample-output>

<sample-output>

**1**
**1**
1 is equal to 1.

</sample-output>

</programming-exercise>
