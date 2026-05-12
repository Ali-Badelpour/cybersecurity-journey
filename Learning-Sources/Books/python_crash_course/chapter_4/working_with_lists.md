
In the previous chapter, we learned some basic commands and information about lists. In this chapter, we will learn to work with lists more efficiently and loop through them. When we loop through a list, we can perform an action (or set of actions) on each item of the list. This allows us to work with lists more efficiently.

## Looping Through an Entire List

Sometimes we want to perform an action on every element of a list. For example, we have a list of numbers and we want to do a calculation on each one. If we write the code one by one, it will take a long time. But with fewer lines of code, we can loop that action through the entire list. In Python, this looping is done with the `for` loop.

Let's say we have a list of magicians and we want to print their names. If we stick to the old method of printing each element individually, we may encounter two problems. First, it is repetitive. Second, if the number of names in the list changes, we need to modify our code. To avoid these (and other) problems, we can use a `for` loop like this:

![magicians.py loop](pictures/01-magicians-loop.png)

The `for` line tells Python to pull one element from the list and assign it to the variable `magician`. Then it prints that magician's name. Python does this for each element in the list.

### A Closer Look at Looping

Looping is very important in programming because it automates repetitive tasks. Python starts by assigning the first element and printing it. Because there are still elements left, Python goes back to the `for` line, assigns the next element, and prints it. This continues until the last element, and then the loop finishes. Python repeats the process for each element – even if we have a million items in the list.

For the variable name in our loop, we can use anything we like. However, it is recommended to use a **meaningful name** (not `i`, `t`, or other single letters), although those work too.

### Doing More Work Within a `for` Loop

We can do anything with the items in the list. For example, we can create a sentence for each magician, saying they performed a great trick:
![more work in loop](pictures/02-loop-more-work.png)

We can write as many lines of code as we want inside the loop. The only thing we must pay attention to is **indentation**! We indent each line inside the loop by pressing `TAB` (or the IDE does it automatically).
![adding another line to loop](pictures/03-loop-more-lines.png)

### Doing Something After a `for` Loop

Any lines written after the `for` loop (without indentation) will be executed **once** after the loop finishes. Let's add a line and run the program:
![after loop line](pictures/04-loop-after.png)

Remember: after finishing a loop, it is often a good idea to print a summary or result of that loop.

## Avoiding Indentation Errors

One reason Python is easy to understand is its use of **indentation**. Python forces us to indent using whitespace (spaces or tabs) and write code in a clean, structured way. In a longer Python file, this indentation helps us get a good view of different parts of the program.

One of the most common errors is the **indentation error**. People forget to indent lines that need indentation, or they indent lines that don't need to be indented.

Let's look at some examples of this error:
![indentation error](pictures/05-indentation-error.png)

### Forgetting to Indent Additional Lines

Sometimes we run our loop and don't get an error, but we also don't get the expected result. In this case, we may have forgotten to indent some lines. For example:
![forgetting to indent](pictures/06-forgot-indent.png)

Because Python finds at least one line that is indented, it will not give us an error. But the program does not work as expected. These errors, which don't break the syntax but produce incorrect results, are called **logical errors**.

### Indenting Unnecessarily

If we indent a line that should not be indented, Python will give us an error:
![unnecessary indent](pictures/07-unnecessary-indent.png)

### Indenting Unnecessarily After the Loop

If we indent a line at the end of the program that is not part of the loop, Python may report a syntax error, or we may get a logical error:

![indent after loop](pictures/08-indent-after-loop.png)

### Forgetting the Colon

The colon (`:`) after the `for` loop tells Python to start the loop block after that line. If you forget the colon, you'll get a syntax error:

![missing colon](pictures/09-missing-colon.png)

Solving some errors is easy; others are hard. That's okay!

---

## Try It Yourself

**4-1. Pizzas:** Think of at least three kinds of your favorite pizza. Store these pizza names in a list, and then use a `for` loop to print the name of each pizza.
- Modify your `for` loop to print a sentence using the name of the pizza, instead of just printing the name. For each pizza, you should have one line of output containing a simple statement like `I like pepperoni pizza.`
- Add a line at the end of your program, outside the `for` loop, that states how much you like pizza. The output should consist of three or more lines about the kinds of pizza you like, and then an additional sentence such as `I really love pizza!`
![exercise 4-1](pictures/10-exercise-4-1.png)

**4-2. Animals:** Think of at least three different animals that have a common characteristic. Store the names of these animals in a list, and then use a `for` loop to print out the name of each animal.
- Modify your program to print a statement about each animal, such as `A dog would make a great pet.`
- Add a line at the end of your program stating what these animals have in common. You could print a sentence such as `Any of these animals would make a great pet!`

![exercise 4-2](pictures/11-exercise-4-2.png)

---

## Making Numerical Lists

Using numbers in programs is very important and common. We use numbers for many reasons: temperatures, distances, sizes, etc. Lists are very powerful for storing numerical data.

### Using the `range()` Function

We can generate a series of numbers using the `range()` function:

![range function](pictures/12-range-function.png)

When we use `range(1, 5)`, it prints the numbers 1 through 4 – it stops **before** the second number. This is called an **off-by-one** behavior in programming. It starts counting from the first number and stops when it reaches the second number.

We can also use `range()` with only one argument. For example, `range(10)` produces numbers 0 through 9.

### Using `range()` to Make a List of Numbers

We have a function called `list()` that can convert the output of `range()` into a list:

![list with range](pictures/13-list-range.png)

Another useful feature of `range()` is the **step size** (third argument). If we provide a third argument, Python starts with the first value and then adds the step value repeatedly until it reaches the second value:

![range step](pictures/14-range-step.png)

In this example, Python starts at 0, adds 2 to get 2, adds 2 to get 4, and continues until the second argument (20).

We can perform many operations on the values from `range()`. For example, we can calculate the squares of numbers from 1 to 20:

![squares with range](pictures/15-squares-loop.png)

We can write the program more efficiently (list comprehension – more on that later):

![squares efficient](pictures/16-squares-efficient.png)

**Note:** First write code that you understand; then look for ways to improve efficiency.

### Simple Statistics with a List of Numbers

Python provides helpful functions for working with numerical lists: `min()`, `max()`, and `sum()`. They are easy to use:

![statistics functions](pictures/17-statistics-functions.png)

### List Comprehensions

A **list comprehension** combines a `for` loop and list creation into one line. Look at this example:

![list comprehension squares](pictures/18-list-comprehension.png)

It works like this: for each number in the given range, do something (here, square it) and automatically create a list of the results.

Remember: in a list comprehension, the `for` statement comes **after** the action we want to perform. List comprehensions are very useful!

---

## Try It Yourself

**4-3. Counting to Twenty:** Use a `for` loop to print the numbers from 1 to 20, inclusive.

![exercise 4-3](pictures/19-exercise-4-3.png)

**4-4. One Million:** Make a list of the numbers from 1 to 1,000,000, and then use a `for` loop to print the numbers. (If the output takes too long, stop it by pressing `CTRL+C` or closing the output window.)

![exercise 4-4](pictures/20-exercise-4-4.png)

**4-5. Summing a Million:** Make a list of the numbers from 1 to 1,000,000. Use `min()` and `max()` to make sure your list actually starts at 1 and ends at 1,000,000. Also, use the `sum()` function to see how quickly Python can add a million numbers.

![exercise 4-5](pictures/21-exercise-4-5.png)

**4-6. Odd Numbers:** Use the third argument of `range()` to make a list of the odd numbers from 1 to 20. Use a `for` loop to print each number.

![exercise 4-6](pictures/22-exercise-4-6.png)

**4-7. Threes:** Make a list of the multiples of 3, from 3 to 30. Use a `for` loop to print the numbers in your list.

![exercise 4-7](pictures/23-exercise-4-7.png)

**4-8. Cubes:** A number raised to the third power is called a cube. For example, the cube of 2 is written as `2**3` in Python. Make a list of the first 10 cubes (the cube of each integer from 1 through 10), and use a `for` loop to print out the value of each cube.

![exercise 4-8](pictures/24-exercise-4-8.png)

**4-9. Cube Comprehension:** Use a list comprehension to generate a list of the first 10 cubes.

![exercise 4-9](pictures/25-exercise-4-9.png)

---

## Working with Part of a List

In this section, we will learn how to work with different parts of a list – a technique called **slicing**.

### Slicing a List

To slice a list, we use square brackets `[ ]` with a colon `:` inside. For example, `[0:3]` gives us items at indices 0, 1, and 2 (but not 3). Remember: the number after the colon is **not included**. This is another off-by-one example.

Look at this example:

![slicing basic](pictures/26-slicing-basic.png)

If we omit the first number (like `[:3]`), Python starts at the beginning of the list (assumes `0`):

![slicing omit first](pictures/27-slicing-omit-first.png)

If we omit the second number (like `[2:]`), Python starts at index 2 and goes to the end of the list:

![slicing omit last](pictures/28-slicing-omit-last.png)

If we use negative indices, we get different outputs:

- `[-3:]` → Give me the last 3 items from the end of the list:

![negative slice 1](pictures/29-negative-slice-1.png)

- `[:-3]` → From the end of the list, skip the last 3 elements and print everything else:

![negative slice 2](pictures/30-negative-slice-2.png)

- `[1:-1]` → Start from index 1 (include it) and skip the last element; give me everything in between:

![negative slice 3](pictures/31-negative-slice-3.png)

**Note:** When counting from the beginning, start at 0. When counting from the end, start at 1 (i.e., `-1` is the last element, `-2` is second‑last, etc.).

We can also add a **step** as a third argument inside the slice. For example, `[1:8:2]` means: start at index 1, stop before index 8, and take every 2nd element:

![slice with step](pictures/32-slice-step.png)

### Looping Through a Slice

We can loop through a slice just like we loop through a whole list:

![loop slice](pictures/33-loop-slice.png)

Slicing is very powerful when working with data, especially numerical data. For example, we can store a player's game results in a list, then choose the last three games and display statistics.

### Copying a List

To copy a list, we make a slice from the beginning to the end (`[:]`) and assign it to a new variable:

![copy list](pictures/34-copy-list.png)

**Note:** If you forget the `[:]` and just write `new_list = old_list`, you are not copying – you are just creating another name that refers to the **same** list. Changes to one will affect the other.

---

## Try It Yourself

**4-10. Slices:** Using one of the programs you wrote in this chapter, add several lines to the end of the program that do the following:
- Print the message `The first three items in the list are:` then use a slice to print the first three items from that program's list.
- Print the message `Three items from the middle of the list are:` then use a slice to print three items from the middle.
- Print the message `The last three items in the list are:` then use a slice to print the last three items.

![exercise 4-10](pictures/35-exercise-4-10.png)

**4-11. My Pizzas, Your Pizzas:** Start with your program from Exercise 4-1. Make a copy of the list of pizzas and call it `friend_pizzas`. Then:
- Add a new pizza to the original list.
- Add a different pizza to the `friend_pizzas` list.
- Prove that you have two separate lists. Print the message `My favorite pizzas are:` and then use a `for` loop to print the first list. Print `My friend's favorite pizzas are:` and then use a `for` loop to print the second list. Make sure each new pizza is stored in the appropriate list.

![exercise 4-11a](pictures/36-exercise-4-11a.png)

![exercise 4-11b](pictures/37-exercise-4-11b.png)

![exercise 4-11c](pictures/38-exercise-4-11c.png)

**4-12. More Loops:** All versions of `foods.py` in this section have avoided using `for` loops when printing, to save space. Choose a version of `foods.py`, and write two `for` loops to print each list of foods.

![exercise 4-12a](pictures/39-exercise-4-12a.png)
![exercise 4-12b](pictures/40-exercise-4-12b.png)

---

## Tuples

Lists in Python are **mutable** – we can change, add, or remove items. But Python also has **tuples**, which are like lists but **immutable** (cannot be changed after creation).

### Defining a Tuple

To create a tuple, use parentheses `( )` instead of square brackets `[ ]`. You can access elements by index, just like with lists, but you cannot modify them:
![tuple definition](pictures/41-tuple-definition.png)

If you try to replace an element, you will get an error:

![tuple error](pictures/42-tuple-error.png)

**Note:** When creating a tuple with only one item, you must add a final comma:

```python
not_a_tuple = (42)   # This is just an integer
actual_tuple = (42,) # This is a tuple with one element
```

### Looping Through All Values in a Tuple

Looping through a tuple is exactly the same as looping through a list:
![loop tuple](pictures/43-loop-tuple.png)

### Writing Over a Tuple

You cannot modify individual items in a tuple, but you can **reassign** the entire tuple (overwrite it with a new tuple):
![overwrite tuple](pictures/44-overwrite-tuple.png)

---

## Try It Yourself

**4-13. Buffet:** A buffet-style restaurant offers only five basic foods. Think of five simple foods and store them in a tuple.
- Use a `for` loop to print each food the restaurant offers.
- Try to modify one of the items, and make sure Python rejects the change.
- The restaurant changes its menu, replacing two of the items with different foods. Add a line that rewrites the tuple, and then use a `for` loop to print each item on the revised menu.

![exercise 4-13a](pictures/45-exercise-4-13a.png)

![exercise 4-13b](pictures/46-exercise-4-13b.png)

---

## Styling Your Code

Writing clean, well‑structured, easy‑to‑read code helps you and others understand it better. Python programmers have agreed on a set of style rules. To become a professional programmer, you should apply these principles immediately.

### The Style Guide (PEP 8)

**PEP** stands for **Python Enhancement Proposal**. One of the oldest and most important is **PEP 8**, which provides guidelines for writing Python code.

Remember: **Code is read more often than it is written.** So instead of writing code that is easiest to write, write code that is easiest to read.

### Indentation

PEP 8 recommends using **4 spaces** for each indentation level. Most code editors (IDEs) can be configured to insert 4 spaces when you press the `TAB` key. You should check that your editor does this (inserts spaces, not a tab character). Mixing tabs and spaces can cause hard‑to‑debug problems.

![indentation settings](pictures/47-indentation-settings.png)

### Line Length

Most programmers agree that a line of code should not exceed **79 characters**. In the past, terminals could not display more than 79 characters on one line. PEP 8 recommends 79 characters per line.

![line length guideline](pictures/48-line-length.png)

### Blank Lines

To organize your file, group related lines of code and separate groups with blank lines. However, don't overdo it. Blank lines do not affect how Python runs the code (Python ignores vertical spacing), but they improve readability.

### Other Style Guidelines

Some PEP 8 guidelines are for more complex Python files. As we learn more, we will learn more of these points.

---

## Try It Yourself

**4-14. PEP 8:** Look through the original PEP 8 style guide at [https://python.org/dev/peps/pep-0008](https://python.org/dev/peps/pep-0008). You won't use much of it now, but it might be interesting to skim through it.


**4-15. Code Review:** Choose three of the programs you've written in this chapter and modify each one to comply with PEP 8.
- Use four spaces for each indentation level. Set your text editor to insert four spaces every time you press the `TAB` key (see Appendix B for instructions).
- Use fewer than 80 characters on each line. Set your editor to show a vertical guideline at the 80th character position.
- Don't use blank lines excessively in your program files.

![exercise 4-15a](pictures/49-exercise-4-15a.png)

![exercise 4-15b](pictures/50-exercise-4-15b.png)

![exercise 4-15c](pictures/51-exercise-4-15c.png)

![exercise 4-15d](pictures/52-exercise-4-15d.png)

![exercise 4-15e](pictures/53-exercise-4-15e.png)
![exercise 4-15f](pictures/54-exercise-4-15f.png)
