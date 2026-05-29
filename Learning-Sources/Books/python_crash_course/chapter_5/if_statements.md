
In programming, sometimes we need to test some conditions and then do something based on them. These are called **if statements**, and Python has them as well.

In this chapter, we will learn to write simple and complex if statements and combine them with lists and for loops.

## A Simple Example

Imagine we have a list of cars. We can make a loop to check the items one by one. Then in our loop, we will write an if statement to check whether an item in the list matches our condition. We can use `else` to perform an action if the item does not match. Look at the example:

![cars.py example](pictures/01-cars-py.png)

## Condition Tests

A condition test is an expression that evaluates to either `True` or `False`. Python checks the value and proceeds accordingly. If `True`, Python executes the code inside the if statement. If `False`, Python ignores that code.

### Checking for Equality

Imagine we define a variable called `car` and assign it the value `'bmw'`. We can then check the variable against other values and get `True` or `False`:

![equality check](pictures/02-equality.png)

Remember: to check equality, use the **double equal sign** (`==`). It asks the question: "Is the value of variable `car` equal to `'bmw'`?"

### Ignoring Case When Checking for Equality

Python is case‑sensitive: `'bmw'` is not the same as `'BMW'`. To perform a case‑insensitive test, use the `.lower()` method to temporarily convert the string to lowercase:

![ignore case](pictures/03-ignore-case.png)

This technique is useful when checking usernames during website sign‑up – for example, ensuring that `'John'` and `'john'` are treated as the same.

### Checking for Inequality

The inequality operator is `!=` (means "not equal"):

![inequality check](pictures/04-inequality.png)

**Note:** Sometimes checking for inequality is faster or more logical than checking for equality.

### Numerical Comparisons

Python supports standard numerical operators:
- `>` greater than
- `<` less than
- `>=` greater than or equal to
- `<=` less than or equal to

![numerical operators](pictures/05-numerical.png)

### Checking Multiple Conditions

Sometimes we need to check two or more conditions together. Use `and` (all must be true) or `or` (at least one must be true).

#### Using `and`
- All conditions must pass → `True`
- If any condition fails → `False`

![and operator](pictures/06-and-operator.png)

For better readability, you can use parentheses: `(age_1 > age_0) and (age_0 == 25)`

#### Using `or`
- Any condition passes → `True`

![or operator](pictures/07-or-operator.png)

### Checking Whether a Value Is in a List

Use the keyword `in` to test if an item exists in a list. For example, before accepting a new username, check that it is not already taken:

![in operator](pictures/08-in-operator.png)

**Note:** `in` is a powerful and readable way to check for membership.

### Checking Whether a Value Is Not in a List

Use `not in` to confirm that a value is absent. For example, if a username is not in a ban list, we can allow sign‑up:

![not in operator](pictures/09-not-in-operator.png)

### Boolean Expressions

A **Boolean expression** is another name for a conditional test – it evaluates to `True` or `False`. Booleans are often used to track the state of an application:

```python
game_running = True
interactive_mode = False
```

---

## Try It Yourself

**5-1. Conditional Tests:** Write a series of conditional tests. Print a statement describing each test and your prediction for the results. Your code should look something like:
```python
car = 'subaru'
print("Is car == 'subaru'? I predict True.")
print(car == 'subaru')
print("\nIs car == 'audi'? I predict False.")
print(car == 'audi')
```
- Look closely at your results and make sure you understand why each line evaluates to `True` or `False`.
- Create at least 10 tests. Have at least 5 evaluate to `True` and 5 to `False`.
![exercise 5-1](pictures/10-exercise-5-1-part-1.png)
![exercise 5-1](pictures/11-exercise-5-1-part-1-result.png)
![exercise 5-1](pictures/12-exercise-5-1-part-2.png)
![exercise 5-1](pictures/13-exercise-5-1-part-2-result.png)
![exercise 5-1](pictures/14-exercise-5-1-part-3.png)
![exercise 5-1](pictures/15-exercise-5-1-part-3-result.png)
![exercise 5-1](pictures/16-exercise-5-1-part-4.png)
![exercise 5-1](pictures/17-exercise-5-1-part-4-result.png)
![exercise 5-1](pictures/18-exercise-5-1-part-5.png)
![exercise 5-1](pictures/19-exercise-5-1-part-5-result.png)


**5-2. More Conditional Tests:** Write more tests in `conditional_tests.py`. Include at least one `True` and one `False` result for each of the following:
- Equality and inequality with strings
- Tests using the `lower()` method
- Numerical tests (equality, inequality, `>`, `<`, `>=`, `<=`)
- Tests using `and` and `or`
- Test whether an item is in a list
- Test whether an item is **not** in a list

![exercise 5-2](pictures/20-exercise-5-2-part-1.png)![exercise 5-2](pictures/21-exercise-5-2-part-2.png)
![exercise 5-2](pictures/22-exercise-5-2-part-3.png)
![exercise 5-2](pictures/23-exercise-5-2-part-4.png)

---

## If Statements

Once we understand conditions, we can start writing if statements. Python offers several forms.

### Simple `if` Statement

The simplest version has one test and one action:

```python
if condition:
    do something
```

If the test is `True`, Python executes the indented block. If `False`, Python ignores it and continues.

Example: check if a person is old enough to vote:

![voting.py simple](pictures/24-voting-simple.png)

An if statement forms a **block** – all indented lines under it are executed together if the condition is true, and all are skipped if it is false.

![voting.py block](pictures/25-voting-block.png)

If the test fails and there is no other code, the program produces no output.

### `if-else` Statements

Use `if-else` when you want one action for when the test passes and another action for when it fails:

![voting.py if-else](pictures/26-voting-if-else.png)

In this example, the test is `False`, so the `else` block runs. **Note:** In an `if-else` block, exactly one side will always execute.

### The `if-elif-else` Chain

Use `if-elif-else` when you have more than two possible outcomes. Python checks conditions one by one; when one condition is `True`, it executes that block and skips the rest. If none are `True`, the `else` block (if present) runs.

Imagine an amusement park with different ticket prices based on age:

![amusement park chain](pictures/27-amusement-chain.png)

**Improvement:** Instead of repeating the `print()` statement, we can store the price in a variable and print it once at the end:

![amusement park improved](pictures/28-amusement-improved.png)

Now you only need to change the message in one place.

### Using Multiple `elif` Blocks

You can include as many `elif` blocks as needed:

![multiple elif](pictures/29-multiple-elif.png)

### Omitting the `else` Block

The `else` block is not required. Sometimes it is better to omit it so that no action is taken for unexpected values:

![no else](pictures/30-no-else.png)

**Note:** The `else` block can be useful for catching invalid input, but don't always include it. If you want to explicitly handle only certain conditions, omit the `else`.

### Testing Multiple Conditions (Not a Chain)

The `if-elif-else` chain runs **at most one** block. When you need to check **all** conditions independently (multiple could be true), use separate `if` statements:

![multiple if statements](pictures/32-multiple-if.png)

With separate `if` statements, Python tests every condition regardless of previous results.

---

## Try It Yourself

**5-3. Alien Colors #1:** Create a variable `alien_color` with value `'green'`, `'yellow'`, or `'red'`.
- Write an if statement that tests whether the alien's color is green. If so, print a message that the player earned 5 points.
- Write one version that passes the test and one that fails (the failing version should have no output).

![exercise 5-3](pictures/33-exercise-5-3.png)

**5-4. Alien Colors #2:** Write an `if-else` chain.
- If alien color is green, print that the player earned 5 points.
- If not green, print that the player earned 10 points.
- Write one version that runs the `if` block and one that runs the `else` block.

![exercise 5-4](pictures/34-exercise-5-4.png)

**5-5. Alien Colors #3:** Convert your chain from 5-4 into an `if-elif-else` chain.
- Green → 5 points
- Yellow → 10 points
- Red → 15 points
- Write three versions to test each color.

![exercise 5-5](pictures/35-exercise-5-5-part-1.png)
![exercise 5-5](pictures/36-exercise-5-5-part-2.png)
![exercise 5-5](pictures/37-exercise-5-5-part-3.png)
![exercise 5-5](pictures/38-exercise-5-5-part-4.png)


**5-6. Stages of Life:** Write an `if-elif-else` chain that prints a person's life stage based on age:
- < 2: baby
- 2–3: toddler
- 4–12: kid
- 13–19: teenager
- 20–64: adult
- ≥ 65: elder

![exercise 5-6](pictures/39-exercise-5-6-part-1.png)
![exercise 5-6](pictures/40-exercise-5-6-part-2.png)

**5-7. Favorite Fruit:** Make a list of your three favorite fruits. Write five independent if statements that check whether certain fruits are in the list. If a fruit is in the list, print a statement like `You really like bananas!`

![exercise 5-7](pictures/41-exercise-5-7.png)

---

## Using `if` Statements with Lists

Combining if statements with lists creates powerful, flexible programs.

### Checking for Special Items

Use an if statement inside a loop to handle specific values differently:

![toppings special](pictures/42-toppings-special.png)

### Checking That a List Is Not Empty

Before processing a list, it's good practice to check whether it contains any items:

![check empty list](pictures/43-check-empty-list.png)

**Note:** When you use a list name in an if statement, Python returns `True` if the list has at least one item, and `False` if it is empty.

### Using Multiple Lists

You can compare a user's requests against a list of available items:

![multiple lists](pictures/44-multiple-lists.png)

---

## Try It Yourself

**5-8. Hello Admin:** Make a list of five or more usernames, including `'admin'`. Loop through the list and print a greeting:
- If the username is `'admin'`, print `Hello admin, would you like to see a status report?`
- Otherwise, print `Hello [username], thank you for logging in again.`

![exercise 5-8](pictures/45-exercise-5-8.png)

**5-9. No Users:** Add a test to your program from 5-8 to check if the list of users is empty.
- If empty, print `We need to find some users!`
- Remove all usernames and verify the correct message is printed.

![exercise 5-9](pictures/46-exercise-5-9-part-1.png)
![exercise 5-9](pictures/47-exercise-5-9-part-2.png)

**5-10. Checking Usernames:** Simulate a website's unique username check.
- Create a list `current_users` with five or more names.
- Create another list `new_users` with five names (include some duplicates case‑insensitively).
- Loop through `new_users` and check if each username is already used (case‑insensitive comparison). Print appropriate messages.

![exercise 5-10](pictures/48-exercise-5-10-part-1.png)
![exercise 5-10](pictures/49-exercise-5-10-part-2.png)

**5-11. Ordinal Numbers:** Store numbers 1–9 in a list. Loop through them and print the proper ordinal ending (1st, 2nd, 3rd, 4th, etc.). Each result on a separate line.

![exercise 5-11](pictures/50-exercise-5-11-part-1.png)
![exercise 5-11](pictures/51-exercise-5-11-part-2.png)

---

## Styling Your `if` Statements

PEP 8 recommends using **one space** around operators such as `=`, `<`, `<=`, etc., for readability.

Example:
```python
if age < 18:      # Good
if age<18:        # Not recommended
```

---

## Try It Yourself

**5-12. Styling if Statements:** Review the programs you wrote in this chapter and make sure you styled your conditional tests appropriately.

![exercise 5-12](pictures/52-exercise-5-12.png)

**5-13. Your Ideas:** You are now a more capable programmer than when you started this book. Record new ideas for programs you might want to solve – games, datasets to explore, web applications to create.

![exercise 5-13](pictures/53-exercise-5-13.png)
