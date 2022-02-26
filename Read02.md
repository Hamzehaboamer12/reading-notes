# Reading:02 - Arrays, Loops, Imports 

<hr>

## Arrays:

<hr>

Arrays are used to store multiple values in a single variable, instead of declaring separate variables for each value.
To declare an array, define the variable type with square brackets:
String[] cars;

We have now declared a variable that holds an array of strings. To insert values to it, we can use an array literal - place the values in a comma-separated list, inside curly braces:
`String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};`

To create an array of integers, you could write:

`int[] myNum = {10, 20, 30, 40};`

Access the Elements of an Array
You access an array element by referring to the index number.
This statement accesses the value of the first element in cars:

---

`String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};
System.out.println(cars[0]);
// Outputs Volvo`

`Note:
Array indexes start with 0: [0] is the first element. [1] is the second element, etc.
`

Array Length
To find out how many elements an array has, use the length property:

`String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};`

**System.out.println(cars.length);
// Outputs 4**


## Loop Through an Array

You can loop through the array elements with the for loop, and use the length property to specify how many times the loop should run.
The following example outputs all elements in the cars array:

`String[] cars = {"Volvo", "BMW", "Ford", "Mazda"};`

`for (int i = 0; i < cars.length; i++)<br>
{
 System.out.println(cars[i]);
}`

## Multidimensional Arrays 2D

A multidimensional array is an array of arrays.
To create a two-dimensional array, add each array within its own set of curly braces:

`int[][] myNumbers = { {1, 2, 3, 4}, {5, 6, 7} };`

---


# Loop :

---

When you know exactly how many times you want to loop through a block of code, use the for loop instead of a while loop:

`for (statement 1; statement 2; statement 3) {
// code block to be executed
}`

* Statement 1 is executed (one time) before the execution of the code block.

* Statement 2 defines the condition for executing the code block.

* Statement 3 is executed (every time) after the code block has been executed.


**The example below will print the numbers 0 to 4:**

`for (int i = 0; i < 5; i++) {
System.out.println(i);
}`

Example explained

* Statement 1 sets a variable before the loop starts (int i = 0).

* Statement 2 defines the condition for the loop to run (i must be less than 5). If the condition is true, the loop will start over again, if it is false, the loop will end.

* Statement 3 increases a value (i++) each time the code block in the loop has been executed.

# Packages and Import:

---

* Packages - Java classes can be grouped together in packages. Package name same as directory name containing the .java files.

* Package declaration - you can have import statements following the declaration, which will allow you to specify classes from other packages.

* Default package - a package that has omitted the package declaration.

* Package declaration syntax - statement order

  * Package statment 
  * Imports
  * Class or interface definitions

* Imports: three options

* JOptionPane class - is in the swing package, which is located in the javax package.

* Wildcard character (*) - specifies that all classes with that package are available to your program.

* Classes can be specified explicitly on import instead of using the wildcard character.

* Can use class name without an import

* Common imports

* import java.awt.*; - Common GUI elements

* import java.awt.event.*; - The most common GUI event listeners

* import javax.swing.*; - More common GUI elements. Note “javax”

* import java.util.*; - Data structures (Collections), time, Scanner, etc classes

* import java.io.*; - Input-output classes

* import java.text.; - Some formatting classes. import java.util.regex.; - Regular expression classes