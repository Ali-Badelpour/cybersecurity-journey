When working with Linux, we are predominantly working with files—and most of them are **text files**.

Consider a typical task: you want to change a system or shell configuration. You open a text file with an editor, modify a few lines, and save it. That's it. Your reconfiguration is complete—simply by changing lines in a text file.

This ability to view, edit, and manipulate text files is not just useful; it's a **crucial skill** that we need to master.

### **Viewing Files**
#### **The Limitations of `cat`**

The most basic command for viewing files is `cat`. However, when used on larger files, `cat` prints the **entire contents** directly to the terminal all at once.

``` bash
cat /etc/nikto.conf
```

[[1-cat nikto.conf.png]]

For small files, this is fine. But for configuration files, logs, or any lengthy text, the output can scroll past in an overwhelming flood, making it impossible to read comfortably.

#### **Viewing Parts of a File: `head` and `tail`**

Instead of displaying everything, we can use two convenient commands that show only portions of a file.

**`head` — View the Beginning of a File**  
By default, `head` displays the first **10 lines** of a file.

``` bash
head /etc/nikto.conf
```

[[2-head nikto.conf.png]]

To specify a different number of lines, use the `-n` option:

``` bash
head -n 20 /etc/nikto.conf   # Shows first 20 lines
```

Alternatively, you can use a shorthand syntax: a dash followed directly by the number:

``` bash
head -20 /etc/nikto.conf     # Also shows first 20 lines
```

[[3-head 20 nikto.conf.png]]

**`tail` — View the End of a File**  
By default, `tail` displays the last **10 lines** of a file.

``` bash
tail /etc/nikto.conf
```

[[4-tail nikto.conf.png]]

Like `head`, you can specify a custom number of lines:

``` bash
tail -n 15 /etc/nikto.conf   # Shows last 15 lines
```

Like `head`, you can specify a custom number of lines using either the `-n` option or the shorthand dash syntax:

``` bash
tail -20 /etc/nikto.conf     # Also shows last 20 lines
```

[[5-tail 20 nikto.conf.png]]

**Why This Matters:** When troubleshooting or configuring, you often only need to see the beginning (for headers/comments) or the end (for recent logs or the bottom of a config file) rather than the entire document.

### Numbering Lines with `nl`

When working with long text files, having line numbers can be incredibly helpful. They allow you to:

- **Locate specific content** much faster
    
- **Reference lines** when discussing configurations with others
    
- **Navigate** more efficiently when editing
    

The `nl` command (**n**umber **l**ines) displays a file with line numbers added to the beginning of each line.

**Syntax:**

``` bash
nl /path/to/file
```

**Example:**

``` bash
nl /etc/nikto.conf
```

[[6-nl nikto.conf.png]]

> **Important Note:** By default, `nl` **does not number blank lines**. This is intentional—it only numbers lines that actually contain content. If you need to number every line including blanks, use the `cat -n` command instead.

**Comparison:**

- `nl filename` → Numbers only non-blank lines
    
- `cat -n filename` → Numbers every line (including blanks)

### Filtering with `grep`

One of the most frequently used and powerful text manipulation commands in Linux is `grep`. It allows you to **search for specific patterns or keywords** within files or command output, displaying only the lines that match.

**Basic Usage with Piping:**  
Imagine you want to find all lines containing "plugin"  in the Nikto configuration file. You can combine `cat` with `grep` using a pipe (`|`):

``` bash
cat /etc/nikto.conf | grep plugin
```

[[7-nikto grep plugin.png]]

This command:

1. `cat` displays the entire file contents
    
2. The pipe (`|`) sends that output to `grep`
    
3. `grep` filters and shows **only the lines containing "plugin"**
    

**Direct Usage (Even Simpler):**  
Since `grep` can read files directly, you don't even need `cat`:

``` bash
grep plugin /etc/nikto.conf
```

This produces the same result more efficiently.

[[8-grep plugin nikto.conf.png]]

**Why `grep` Is So Powerful:**

- **Search any text:** Files, command output, or even entire directory trees
    
- **Pattern matching:** Supports regular expressions for complex searches
    
- **Case-insensitive search:** Add `-i` to ignore uppercase/lowercase
    
- **Line numbers:** Add `-n` to show matching line numbers
    
- **Recursive search:** Add `-r` to search through entire directories
    
- **Count matches:** Add `-c` to count matching lines instead of displaying them
    

**Examples:**

``` bash
grep -i "error" /var/log/syslog    # Case-insensitive search for "error"
grep -n "Listen" /etc/apache2/ports.conf   # Show line numbers with matches
grep -r "password" /etc/            # Search all files in /etc for "password"

```

> **The `grep` command is one of the most powerful tools in Linux. Master it—you'll use it constantly!**

### Using `sed` to Find and Replace

The `sed` command (short for **s**tream **ed**itor) is a powerful text manipulation tool. It allows you to find text patterns and perform actions on them—most commonly, find and replace operations.

Think of it as the Linux equivalent of the **Find and Replace** function in Windows word processors, but far more powerful and scriptable. Unlike interactive editors, `sed` works on text streams, making it ideal for:

- Batch processing multiple files
    
- Automating text edits in scripts
    
- Filtering command output
    

**Getting Started with a Practical Example:**

Let's begin by examining a password list from John the Ripper (a popular password cracking tool). We'll use `head` to preview just the first few lines:

``` bash
head /usr/share/wordlists/john.lst
```

[[9-head john.lst.png]]

This gives us a sample of the password list we'll work with. Next, we'll learn how to use `sed` to find and replace text within this file.

**Real-World Scenario: Password Munging**

Imagine your network uses a common default password: "password". Some users practice **munging**—replacing repeated characters with something else to create variations. For example, changing "ss" in "password" to "33" creates "pa33word". While this might seem like a stronger password, we can crack it using `sed` and dictionary attacks.

**The `sed` Command for Munging:**

``` bash
sed 's/s/3/g' /usr/share/wordlists/john.lst > /home/kali/Desktop/mutated_passwords.txt
```

[[10-sed john.lst.png]]

**Breaking Down the Command:**

- `sed`: The stream editor command
    
- `'s/s/3/g'`: The substitution instruction
    
    - `s`: **S**ubstitute command
        
    - `/s/`: Find occurrences of the letter "s"
        
    - `/3/`: Replace them with the number "3"
        
    - `g`: **G**lobal flag (replace _all_ occurrences, not just the first)
        
- `/usr/share/wordlists/john.lst`: The input file (original password list)
    
- `> /home/kali/Desktop/mutated_passwords.txt`: Redirect the output to a new file
    

**Result:** When you open the new file, every "s" in the original password list has been replaced with "3".

[[11-sed john.lst mutated_password.txt.png]]

> **⚠️ Important Note on the `g` Flag:**
> 
> - **With `g`** (`s/s/3/g`): Replaces **every** occurrence of "s" in each line. "password" → "pa33word" (both 's' characters are changed)
>     
> - **Without `g`** (`s/s/3/`): Replaces **only the first** occurrence in each line. "password" → "pa3sword" (only the first 's' changes)
>     

**Why This Matters for Security:**  
Understanding this technique helps you:

- Generate custom wordlists for penetration testing
    
- Understand how attackers mutate common passwords
    
- Test password policy strength against common substitutions

### Viewing Files with `more` and `less`

The `cat` command works well for viewing small files, but it becomes impractical for larger files—especially configuration files, logs, or any text that spans hundreds or thousands of lines. When a file is too large to fit on one screen, the content scrolls past too quickly to read.

For these situations, we have two better alternatives: **`more`** and **`less`**.

#### `more` — View One Page at a Time

When using the `more` command, it displays only **one page** of the file at a time, allowing you to control how quickly you move through the content.

**Basic Usage:**

``` bash
more /etc/nikto.conf
```

[[12-more nikto.conf.png]]

**Navigation in `more`:**

- **Enter**: Scroll down one line
    
- **Space bar**: Scroll down one full screen
    
- **q**: Quit and return to the command prompt
    

> **⚠️ Note:** Don't confuse the `more` command with the `man` command. `man` is used for viewing manual pages, while `more` is for viewing configuration files, logs, or any other text files.

#### `less` — Display and Filter with More Power

The `less` command is exactly like `more` but with **greater options** (the name is a playful pun—it does "more" than `more`). It displays content page by page but adds powerful filtering and search capabilities.

**Basic Usage:**

``` bash
less /etc/nikto.conf
```

[[13-less nikto.conf.png]]

Notice at the **bottom left** of the screen, `less` displays the file path and name—a helpful reference showing exactly which file you're viewing.

**Searching in `less`:**

- Press the forward slash key (**/**) to enter search mode
    
- Type the term you want to find and press **Enter**
    
- `less` will highlight all occurrences and jump to the first match
    
- Press **n** (for **n**ext) to move to the next occurrence
    
- Press **N** (Shift + n) to move to the previous occurrence
    

[[14-less nikto.conf search scan.png]]

**Additional `less` Navigation:**

- **Space bar** or **f**: Move forward one screen
    
- **b**: Move backward one screen
    
- **g**: Go to the beginning of the file
    
- **G**: Go to the end of the file
    
- **q**: Quit
    

> **Pro Tip:** `less` is excellent for searching through large files like logs or configuration files. The ability to search forward (`/`) and backward (`?`) makes it indispensable for troubleshooting.