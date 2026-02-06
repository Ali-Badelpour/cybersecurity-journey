
In this chapter, we will run our first Python file: the `hello_world.py` program. We can start using the terminal or a text editor. A text editor allows us to write lines of code and execute them. It will also highlight your code syntax, which makes coding easier and more understandable.

#### **Setting Up Your Programming Environment**

Before using Python, we need to consider that it works slightly differently on various operating systems. We must ensure everything is set up correctly before running any code.

**Python Versions**  
Python is continuously updated to become more powerful and versatile. At the time of writing this summary, the latest version is Python 3.12 (Note: 3.15 is not yet released; 3.12 is current). I am using version 3.12.1, which is perfectly fine for all exercises in this book.

#### **Running Snippets of Python Code**

We can use a terminal window to run bits of Python code without saving them to a file. When you open the terminal (Command Prompt on Windows) and type `python`, you will be prompted with `>>>` (three angle brackets). This is the Python interpreter prompt. Some short code snippets can be run directly here. The `>>>` prompt means you are using the Python interpreter in the terminal, not a text editor.

1. Run your terminal and enter `python`.
    
2. Type this command: `print("Hello World")`  
    [[01-python-terminal.jpg]]
    

#### **Python on Different Operating Systems**

Python is a cross-platform programming language, meaning you can run programs on almost any modern operating system. However, installation and setup differ slightly.

- **On Windows:** Python is not pre-installed. You must download and install it, along with a text editor if desired. Once installed, you can run it from the terminal or a text editor. To exit the Python interpreter in the terminal, type `exit()` or press `CTRL+Z` and then `Enter`.
    
- **On macOS:** Similar to Windows, Python (version 3) is not pre-installed and must be installed by the user.
    
- **On Linux Systems:** Python is usually pre-installed, as these systems are designed with programming in mind. To check your version, open the Terminal (`Ctrl+Alt+T`) and type `python3 --version`.  
    [[02-python-linux-terminal.jpg]]


#### **Running a Hello World Program**

We use text editors to write and save Python programs in files. After installing a text editor (like VS Code), ensure Python is also installed on your system.

**Running `hello_world.py`**

1. Create a folder named `python_work` on your desktop. For naming files or directories (folders), use lowercase letters and underscores.
    
2. Open this folder in your text editor (e.g., VS Code).
    
3. Create a new file inside it named `hello_world.py`.
    
4. Type: `print("Hello World")`
    
5. Run the program from within the editor. You should see a terminal panel open and display "Hello World".  
    [[03-hello-world-vscode.jpg]]


#### **Running Python Programs from a Terminal**

Sometimes you need to run a program directly from the system terminal.

1. Open the terminal and navigate to your project folder using the `cd` command (e.g., `cd Desktop/python_work`).
    
2. Use the `ls` command (or `dir` on Windows) to list the folder's contents and confirm your file is there.
    
3. Run the program:
    
    - **On Windows:** `python hello_world.py`
        [[04-run-terminal-windows.jpg]]
    - **On Linux/macOS:** `python3 hello_world.py`  



---

### **Try it Yourself**

**1-1. [python.org](https://python.org/):** Explore the Python homepage ([https://python.org](https://python.org/)) to find topics that interest you. As you become more familiar with Python, different parts of the site will become more useful.  
[[05-python-org-jobs.jpg]]

**1-2. Hello World Typos:** Open your `hello_world.py` file. Make a typo somewhere and run the program again.

- Can you make a typo that generates an error?
    
- Can you make sense of the error message?
    
- Can you make a typo that doesn’t generate an error? Why do you think it didn't?
    

Examples:

- **NameError:** This happens if you misspell a built-in function name (e.g., `pint("Hello World")`).  
    [[06-error-nameerror.jpg]]
    
- **SyntaxError:** This happens if the spelling is correct but the syntax is wrong (e.g., `print "Hello World")` missing a parenthesis).  
    [[07-error-syntaxerror.jpg]]
    
- **No Error, Wrong Output:** This happens if the string inside the quotes is misspelled (e.g., `print("Hllo Wrld")`). This doesn't cause an error because it's just text to be printed.  
    [[08-wrong-output.jpg]]


**1-3. Infinite Skills:** If you had infinite programming skills, what would you build? Having an end goal in mind gives you an immediate use for your new skills. Keep an "ideas" notebook. Take a few minutes now to describe three programs you want to create.  

I would focus on building tools to enhance cybersecurity, both offensive and defensive.