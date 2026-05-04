
In this chapter, we will learn different types of data and how to use variables to store that data.

### What Really Happens When We Run `hello_world.py`

When we have the code `print("Hello World")`, you might think it's a simple job for Python to output `Hello World`. But it's not that simple. Let's take a closer look.

Python reads all the words in the code and determines the course of action. For example, when it sees `print` followed by parentheses, it will print everything inside the parentheses on the screen. This is called **syntax highlighting**. The program understands that `print` is a code instruction, but everything inside the parentheses are just simple words, not executable code.

### Variables

Let's add a variable to `hello_world.py`. Add this line to the beginning of our code:

```python
message = "Hello World"
print(message)
```
[[01-variable-hello-world.png]]

Now if we run this program, we will get the same result as running the previous code (`print("Hello World")`). In the second version, we added a variable named `message`. Each variable has a **value** (the information stored in it). This makes the Python interpreter work a bit more, but the code becomes more readable and easier to handle in the future.

If we have two lines with the variable `message` and print both, we can have both pieces of information printed on the screen:

```python
message = "Hello World"
print(message)
message = "Hello Python!"
print(message)
```
[[02-variable-reassign.png]]

**Note:** If we forget to use `print` for the first variable, the Python interpreter will see that we have two variables with the same name, so it will update the content. When we call `print` later, it will only print the most updated value.

### Naming and Using Variables

We have some rules to follow to avoid errors and make our code easy to read:

- Variable names can only contain letters, numbers, and underscores. They can start with a letter or an underscore, but **not** with a number.
- Spaces are not allowed; use underscores to separate words (e.g., `my_variable`).
- Do not use Python keywords or function names as variable names (e.g., do not call your variable `print`).
- Variable names should be short but meaningful and practical (e.g., `name` instead of `n`).
- Be careful with lowercase `l` and uppercase `O` – they can be confused with `1` and `0`.

With practice and by reading others' code, you will get better at naming variables.

**Note:** Try to use all lowercase letters for variable names. Uppercase is used for a different purpose (constants), which we will discuss later.

### Avoiding Name Errors When Using Variables

Every programmer makes mistakes, but good programmers can handle errors quickly and efficiently. Let's create an error on purpose to understand it better.

Type this code and run it:

```python
Message = "Hello Python"
print(mesage)
```
[[03-nameerror.png]]

Python helps us find the problem using a feature called a **traceback**. The traceback shows:
- the line where the problem occurred,
- the part of that line where the problem is,
- and the type of error.

In this case, we see a `NameError`. This means we are trying to use a variable that hasn't been defined, or we have a spelling mistake that Python cannot correct.

Two steps to fix it:
1. Check that we have defined the variable we are calling.
2. If it is defined, check for spelling mistakes in either the variable definition or the call.

Programming languages do not check spelling and grammar. You can create anything you like, but when calling a variable, it must exactly match the name you created.

You might search for an error in 1000+ lines of code only to find it's a simple typo. Remember: **errors are your friends, don't hate them. Love them.** They help you learn.

### Variables Are Labels

Many people say variables are like boxes that store values. This can be helpful at first, but it's not quite right. It's better to think of variables as **labels** that we attach to values. Variables refer to certain values.

Remember: to handle errors effectively, you must deeply understand the source – in this case, variables.

**Note:** The best way to learn programming concepts is by doing and writing code.

---

## Exercises (Try It Yourself)

Write a separate program for each exercise. Save each with a filename that follows Python conventions (lowercase letters and underscores), like `simple_message.py` and `simple_messages.py`.

**2-1. Simple Message:** Assign a message to a variable, then print that message.
[[04-exercise-2-1.png]]

**2-2. Simple Messages:** Assign a message to a variable and print it. Then change the value to a new message and print the new message.
[[05-exercise-2-2.png]]


---

## Strings

We have many data types. One of the easiest is **strings**. They are simple yet very useful. To define a string in Python, you can use single or double quotes:

```python
"This is a Python string"
'This is also a Python string'
```

### Changing Case in a String with Methods

Changing the case of words is one of the easiest string operations. Suppose we have a name in lowercase. We can make it title case like this:

```python
name = "david stones"
print(name.title())
```
[[06-title-method.png]]

`title()` is a **method** that capitalizes the first letter of each word. The dot (`.`) after `name` tells Python to apply the `title` method to the variable `name`. Sometimes methods need additional information, which goes inside the parentheses. `title()` doesn't require any extra information, so the parentheses are empty.

**Note:** `title()` makes the first letter of each word uppercase and all other letters lowercase. For example, `"daViD"` becomes `"David"`. This is very important.

We also have `upper()` and `lower()` methods:

```python
print(name.upper())
print(name.lower())
```
[[07-upper-lower.png]]

**Note:** You can use `title()` to display usernames on a website, and `lower()` to store information consistently while preserving the original user input.

### Using Variables in Strings

Suppose we want to combine a username with a greeting. We can use **f-strings**:

```python
first_name = "david"
last_name = "stones"
full_name = f"Hello {first_name} {last_name}"
print(full_name)
```
[[08-fstring-simple.png]]

The `f` stands for "format". Python formats the string by inserting the variable values.

You can also use methods inside f-strings:

```python
full_name = f"Hello {first_name.title()} {last_name.title()}"
```
[[09-fstring-methods.png]]

### Adding Whitespace to Strings with Tabs or Newlines

**Whitespace** refers to any nonprinting characters, such as spaces, tabs, and end-of-line symbols. We use whitespace to organize output and make it easier to read.

- `\t` – adds a tab
- `\n` – starts a new line

[[10-tab-whitespace.png]]

[[11-newline-whitespace.png]]

You can combine these whitespace characters as well.

### Stripping Whitespace

Consider two strings: `"python"` and `" python"`. To us, they look almost the same, but Python treats the second one as different because of the leading space. This can cause problems (e.g., when users log in with accidental spaces).

Python provides methods to remove whitespace:
- `rstrip()` – removes whitespace from the right end
- `lstrip()` – removes whitespace from the left end
- `strip()` – removes whitespace from both ends

[[12-rstrip.png]]

[[13-strip-methods.png]]

### Removing Prefixes and Suffixes

Suppose you have a URL like `https://www.google.com`. You can remove the `https://` prefix using `.removeprefix()`:

```python
url = "https://www.google.com"
print(url.removeprefix("https://"))
```

Similarly, `.removesuffix()` removes a suffix like `.com`:

```python
print(url.removesuffix(".com"))
```
[[14-removeprefix-suffix.png]]

This is the first time we use parentheses with methods to provide additional information.

### Avoiding Syntax Errors with Strings

A **syntax error** occurs when Python doesn't understand part of your code as valid Python. For example, using single quotes around a string that contains an apostrophe can cause an error:

```python
print('I'm Tom')  # SyntaxError
```
[[15-syntaxerror.png]]

There are many kinds of errors. Some are easy to handle; others can be frustrating and hard to understand.

**Note:** Using a good code editor can help you find errors early.

---

## Try It Yourself

**2-3. Personal Message:** Use a variable to represent a person's name, and print a message to that person. The message should be simple, such as "Hello Eric, would you like to learn some Python today?"
[[16-exercise-2-3.png]]

**2-4. Name Cases:** Use a variable to represent a person's name, then print that name in lowercase, uppercase, and title case.
[[17-exercise-2-4.png]]

**2-5. Famous Quote:** Find a quote from a famous person you admire. Print the quote and the author's name. Your output should look like: Albert Einstein once said, "A person who never made a mistake never tried anything new."
[[18-exercise-2-5.png]]

**2-6. Famous Quote 2:** Repeat Exercise 2-5, but this time represent the famous person's name with a variable called `famous_person`. Then compose your message in a variable called `message`. Print the message.
[[19-exercise-2-6.png]]

**2-7. Stripping Names:** Use a variable to represent a person's name with whitespace characters at the beginning and end. Use `\t` and `\n` at least once. Print the name with whitespace visible. Then print the name using `lstrip()`, `rstrip()`, and `strip()`.
[[20-exercise-2-7.png]]

**2-8. File Extensions:** Assign `'python_notes.txt'` to a variable called `filename`. Use the `removesuffix()` method to display the filename without the extension.
[[21-exercise-2-8.png]]

---

## Numbers

Numbers appear frequently in programming. Python handles them in several ways. We'll start with the simplest: **integers**. Python supports standard order of operations.

### Integers

Operations:
- `+` addition
- `-` subtraction
- `*` multiplication
- `/` division
- `**` exponentiation

[[22-integer-math.png]]

### Floats

A **float** is any number with a decimal point. Floats are easy to work with, but sometimes you may get an arbitrary number of decimal places.
[[23-float-examples.png]]


**Note:** When dividing two integers, the result is always a float. When mixing integers and floats, the result is also a float.

### Underscores in Numbers

Large numbers like `2000000` can be hard to read. Use underscores for readability:

```python
balance = 2_000_000
print(balance)  # Output: 2000000
```
[[24-underscores-numbers.png]]

Python ignores the underscores when printing. This works for both integers and floats.

### Multiple Assignment

You can assign multiple variables in one line by separating them with commas:

```python
x, y, z = 10, 20, 30
```
[[25-multiple-assignment.png]]

### Constants

If a variable's value should never change, programmers use **uppercase letters** to indicate a constant. Python does not have a built-in constant type, but this convention is widely used:

```python
FRAME_PER_SECOND = 60
```

---

## Try It Yourself

**2-9. Number Eight:** Write addition, subtraction, multiplication, and division operations that each result in the number 8. Enclose each operation in a `print()` call. Your output should be four lines, each showing the number 8.
[[26-exercise-2-9.png]]

**2-10. Favorite Number:** Use a variable to represent your favorite number. Then create a message that reveals your favorite number and print it.
[[27-exercise-2-10.png]]

---

## Comments

**Comments are important!** They add human-readable explanations so others can understand your code. In Python, the hash mark (`#`) indicates a comment. The Python interpreter ignores everything after `#` on that line. Keep a balance: not too few comments, not too many.

---

## Try It Yourself

**2-11. Adding Comments:** Choose two of the programs you've written and add at least one comment to each. If your programs are too simple, just add your name and the current date at the top of each file, plus one sentence describing what the program does.
[[28-exercise-2-11a.png]]
[[29-exercise-2-11b.png]]


## The Zen of Python

Some important guiding principles for writing Python code. Type `import this` in a Python interpreter to see them.
[[30-zen-of-python.png]]
