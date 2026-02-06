Hackers, by their very nature, are doers. They like to touch and play with everything. Sometimes, they create or even break things out of curiosity, to show off, or simply to challenge themselves. We will learn just enough to play and explore the operating systems hackers use: Linux.

**Introductory Terms and Concepts**

To understand the core concepts, we first need to know some basic related terms:

- **Binaries:** These are executable files, similar to `.exe` files in Windows. They usually refer to compiled programs that have been translated from human-readable source code (like C or C++) into machine code (the 0s and 1s the CPU understands). You can find them in directories like `/usr/bin` or `/usr/sbin`. Utilities like `ps`, `cat`, `ls`, and `ifconfig`, as well as applications like `aircrack-ng` (a wireless hacking tool) or `Snort` (an intrusion detection system), are all examples of binaries.
    
- **Case Sensitivity:** Be careful with your Caps Lock! In Linux, typing `Desktop`, `desktop`, and `DeskTop` refers to three different things, unlike in Windows where they would be the same. The "File or directory not found" error will likely be your best friend for a while.
    
- **Directory:** This is simply another name for a folder, just like in Windows.
    
- **Home:** Home sweet home! This is your personal directory. Files you create will often be saved here by default. Every user has their own `/home` directory.
    
- **Kali:** This is a specialized version of Linux focused on penetration testing. It comes with many security tools pre-installed.
    
- **Root:** This is the administrator (or _superuser_) account, which can do absolutely anything on the system. You will need root access to perform many system-level tasks or use certain tools.
    
- **Script:** A script is a series of commands that run in an interpretive environment. Scripting languages like _bash_, _Python_, or _Ruby_ interpret these commands as source code.
    
- **Shell:** This is the command interpreter and environment in Linux—the place where you talk to the system, give commands, and run scripts. There are different types of shells. The current default is often _zsh_ (Z Shell), but we will also be using _bash_ (Bourne-again Shell).
    
- **Terminal:** This is a command-line interface (CLI), or the application you use to access the shell.

### **A Tour of Kali**
To log in, use `kali` for both the username and password. By default, the password is **kali**.

We need to familiarize ourselves with two important things: the **Terminal** and the **file structure**.

**The Terminal**  
Open the **Terminal** by clicking its icon in the top-left corner or by pressing **Ctrl + Alt + T**.

[[01-terminal.png]]
[[02-terminal-page.png]]

This is your command-line environment, where you can run commands and execute scripts. Kali uses **zsh** (Z Shell) by default. To switch to the **bash** shell, simply type `bash` and press Enter in your terminal.

This will:

1. Start a Bash subshell within your current Zsh session.
    
2. Switch to using Bash syntax (which is different from Zsh).
    
3. Keep your original Zsh session running in the background. Type `exit` to return to it.
    

**Example:**

``` bash
# You start in Zsh (Kali's default terminal)
$ echo $SHELL
/bin/zsh

$ bash  # ← Type this command
# You are now in a Bash shell
$ echo $SHELL
/bin/zsh  # This still shows Zsh because it's your default login shell
$ echo $0
bash      # This confirms your current shell is Bash

# Type 'exit' to return to Zsh
$ exit
exit
$  # You are now back in Zsh
```

[[03-changing-shell.png]]

**Quick Comparison:**

- **Zsh Prompt:** Usually shows `%` or `$` followed by the directory.
    
- **Bash Prompt:** Often shows `$` or a format like `username@hostname`.
    

> **⚠️ Warning:** The `bash` command only changes your shell temporarily for the current session. If you close the terminal or type `exit`, you will return to Zsh.

To change your password, use the `passwd` command.

[[04-passwd-command.png]]
### **The Linux Filesystem**
The structure of files in Linux is different from Windows. While Windows organizes files under physical drives (like `C:\`), Linux uses a **logical filesystem**. Imagine it as an upside-down tree.

[[05-linux-filesystem-structure.png]]

At the very top is `/`, referred to as the **root of the filesystem**.

> **Important:** The **root user** (administrator) and the **root directory** (`/`) are related but different concepts.

**Important Subdirectories:**

- **`/root`** : The home directory for the root user (the system administrator).
    
- **`/etc`** : Contains system-wide configuration files. These files control how and when programs start.
    
- **`/home`** : Contains the personal directories for all regular users.
    
- **`/mnt`** : A standard location for temporarily **mounting** external filesystems (e.g., network drives).
    
- **`/media`** : Where removable media (like USB drives and CDs) are automatically mounted.
    
- **`/bin`** : Contains essential **binary** executables (common commands) needed for the system to operate.
    
- **`/lib`** : Stores essential shared **library** files (similar to DLLs in Windows) needed by the binaries in `/bin` and `/sbin`.
    

**⚠️ Crucial Security Practice: Avoid logging in as root.**  
If you are logged in as the root user and your system is compromised, the attacker immediately gains full administrative (root) access. Always use a standard user account and switch to root privileges only when necessary, using commands like `sudo`.

### **Basic Commands in Linux**
Here are some basic commands to get you started with using Linux.

**Finding Yourself with `pwd`:**  
When you need to know which directory you are currently operating in, use the `pwd` command (**P**rint **W**orking **D**irectory) in the terminal. This command will return the full path to your current location within the Linux directory tree. This path will always start from the root directory, `/`.

[[06-pwd-command.png]]

**Checking Your Login with `whoami`:**  
In Kali, the root user has absolute control over the entire system. Therefore, it is crucial to always verify which user account you are currently using. You can use the `whoami` command to see your current username.

[[07-whoami-command.png]]

> **⚠️ Important:** For normal, daily operations, you should **always log in as a standard user, not as the root user!** Use the `whoami` command to check this status frequently.

### **Navigating the Linux Filesystem**
It is essential to be able to navigate the filesystem to check, move, or manage files. Although you can use a Graphical User Interface (GUI), you will often only have access to a terminal command-line interface, such as when working on a remote server. Therefore, navigating the filesystem from the command line is a vital Linux skill.

**Changing Directories with `cd`:**  
To change your current folder (directory), use the `cd` command (**C**hange **D**irectory). Remember to include a space after `cd`, and start your target address with `/` if you are specifying an absolute path from the root. You can then verify your new location with `pwd`.

For example, if you are in the `/var/log` directory and want to move up one level toward the root (`/`), you can use `cd ..`. The `..` represents the parent directory. So, using `cd ..` from `/var/log` will take you to `/var`.

[[08-cd-command.png]]

You can chain these to move up multiple levels at once:

- One level up: `cd ..`
    
- Two levels up: `cd ../..`
    
- Three levels up: `cd ../../..`
    
- ...and so on.
    

> **Note:** To immediately jump to the root of the filesystem (the top of the tree, `/`), use `cd /`.

**Listing the Contents of a Directory with `ls`:**  
You can view the files and subfolders within a directory using the `ls` command (similar to `dir` in Windows).

- To see the contents of your **current** directory, simply type `ls`.
    
- To see the contents of a **specific** directory, add the path: `ls /etc`.
    

The basic `ls` output is minimal. You can add **flags** (options) to get more information:

- `ls -l`: Provides a **l**ong list with detailed information (permissions, owner, size, modification time).
    
- `ls -a`: Shows **a**ll files, including hidden ones (files whose names begin with a dot, like `.bashrc`).
    
- `ls -la`: Combines the above to show a **l**ong list of **a**ll files.
    

> **Tip:** For efficiency, combine single-letter flags after a single dash (e.g., `ls -la` instead of `ls -l -a`).

**Level Up: Common & Useful `ls` Flag Combinations**  
Here are some powerful combinations for controlling how `ls` displays information:

- `ls -lh`: Long list with **h**uman-readable file sizes (e.g., 4.0K, 1.5M).
    
- `ls -ltrh`: A very common combination. Sorts by modification **t**ime ( **t** ), in **r**everse order (newest files last), with **h**uman-readable sizes.
    
- `ls -lSrh`: Sorts by **S**ize (largest first), in **r**everse order (smallest first), with **h**uman-readable sizes.
    

> **Key Note:** The `-r` flag **r**everses whatever the current sort order is. To control _how_ files are sorted, combine it with:
> 
> - `-t`: Sort by time (newest first by default; use `-tr` for oldest first).
>     
> - `-S`: Sort by size (largest first by default; use `-Sr` for smallest first).

[[09-ls-command.png]]
### **Getting Help**
Almost every command and program in Linux includes a help file to guide users. The most common way to access this is via a **help switch**.

For example, when using `aircrack-ng` (one of Kali's primary wireless security tools), you can type `aircrack-ng --help` to view its help documentation.

> **Note the syntax:** We used a **double dash** (`--`) for the `--help` option. For standard, single-letter command options, we use a **single dash**, like `ls -l`.

However, `--help` is not a universal standard. If a command doesn't recognize `--help`, try its common alternatives: `-h` or `-?`.

For instance, with `nmap` (a powerful port-scanning tool), the correct help option is `-h`.

[[10-nmap-help-command.png]]
### **Referencing Manual Pages with `man`**
The `man` command provides access to comprehensive manual pages for commands, utilities, and applications. To use it, type `man` followed by the name of the item you wish to learn about.

For example, to open the manual for `nmap`, you would type: `man nmap`

Once the manual page opens, you can navigate using the **arrow keys**, the **Enter** key to move line-by-line, or the **Page Up** and **Page Down** keys. To exit the manual viewer and return to the command prompt, simply press the **`q`** key (for **q**uit).

[[11-man-nmap-command.png]]
### **Finding Stuff**
The ability to find files and programs on the system is essential. Here, we will look at some useful commands for this task.

**Searching with `locate`**

One of the simplest search commands is `locate`. It performs a rapid search across the entire filesystem for any path containing your keyword. You type `locate` followed by the keyword you wish to find.

For example, to find all files and directories related to `nmap`, you would type: `locate nmap`

> **Note:** The `locate` command is extremely fast because it searches a pre-built database of all files, rather than the live filesystem. However, for common terms, it can return an overwhelming number of results, making it difficult to find what you need.

[[12-locate-nmap-command.png]]

Another limitation of `locate` is its reliance on a database that is typically updated only once per day. If you create a new file and try to search for it immediately, `locate` will not find it.

[[13-sudo-updatedb-command.png]]

To manually update this database, use the command: `sudo updatedb`

> **Understanding `sudo`:** You are logged in as a normal user, but `sudo` allows you to execute a single command with root (administrator) privileges. You will be prompted to enter **your own user's password** (not the root password) to proceed.

Despite its limitations—potentially overwhelming results and reliance on a sometimes-stale database—the `locate` command remains useful for quickly finding known files that have existed on the system for more than a day.

### **Finding Binaries with `whereis`**
The `whereis` command is specifically designed to locate the **binary** (executable), **source code**, and **manual page** for a given program. It searches only standard directories, which results in a clean, focused output.

To use it, simply type `whereis` followed by the program name: `whereis nmap`

[[14-whereis-nmap-command.png]]

The primary advantage of `whereis` over `locate` is its precision. Instead of an overwhelming list of every file containing the keyword, `whereis` returns a concise, one-line result showing only the essential related paths (if they exist), making it much easier to read.

### **Finding Binaries in the PATH Variable with `which`**
The `which` command is more specific than `locate` or `whereis`. It returns the location of the **executable binary** that will actually run when you type a command, but _only_ if that binary is located in a directory listed in your `PATH` variable.

**Understanding the PATH Variable**  
The `PATH` is an environment variable that holds a list of directories. When you type a command like `nmap` in the terminal, the system searches through each directory in your `PATH`, in order, to find an executable file with that name.

For example, typing `which nmap` tells the system to check the `PATH` and print the full path to the first `nmap` executable it finds.

[[15-which-nmap-command.png]]

The output is the system saying, "When you ask for `nmap`, I will run the program from _this specific location_."

**Why PATH Matters: A Practical Example**  
Imagine you have a program named `myapp` installed in `/usr/local/myapp/bin/`.

- If `/usr/local/myapp/bin/` is **in your PATH**, you can simply type `myapp` to run it.
    
- If it is **not in your PATH**, you will get a `command not found` error and must type the full path every time. `/usr/local/myapp/bin/myapp`
    

> **⚠️ Important: The Order in PATH is Critical**  
> The `PATH` is a list of directories separated by colons (`:`), like this:  
> `/usr/local/bin:/usr/bin:/bin:/home/user/mytools`
> 
> The system searches from **left to right**. The first matching executable found is the one that runs. This is crucial if you have multiple versions of a program (e.g., Python 3.11 and Python 3.12). Whichever version's directory appears first in the `PATH` will be executed when you type `python`. Always be mindful of the order when modifying your `PATH`.

### **Performing More Powerful Searches with `find`**

The `find` command is the most powerful and flexible search utility in Linux. You can start a search from any directory and filter results by countless criteria, including name, type, permissions, size, modification date, and more.

**Basic Syntax:** `find <directory> <options> <expression>`

**Example:**  
To search the entire filesystem for a file named `journalctl`, you would type:
`find / -type f -name journalctl`

[[16-find-journalctl.png]]

**Breaking Down the Command:**

1. **`/`**: The starting directory for the search. Using `/` tells `find` to search the entire filesystem.
    
2. **`-type f`**: The **option** to filter by type. Here, `f` stands for a regular **f**ile. Use `d` to search for **d**irectories.
    
3. **`-name journalctl`**: The **expression** specifying what to find—in this case, the exact name.
    

> **Performance Tip:** Searching from the root (`/`) can be slow. To make your searches faster, always start in the most specific directory possible. For example, to find a binary, start your search in `/bin`, `/usr/bin`, or `/sbin` instead of the entire filesystem.

[[17-find-usr-journalctl.png]]

By default, the `find` utility performs **exact name** matching. For example, the command `find / -type f -name nmap` will only return files named precisely `nmap`, not `nmap.exe`, `nmap-b`, etc.

To perform broader searches, you must use **wildcards**. The asterisk `*` is the most common wildcard, representing **zero or more** of any character.

**Example: Finding Files That Start With "nmap"**  
To find all files whose names _begin_ with `nmap` (regardless of what comes after), you would use: `sudo find / -type f -name "nmap*"`

[[18-find-wildcard-asterisk.png]]

This command is powerful for finding files when you:

- Only know part of the name.
    
- Want to find all related files (e.g., `nmap`, `nmap-b`, `nmap.xml`).
    
- Don't know the file extension.

### **Essential Wildcards for Powerful Searches**

Wildcards are essential characters that greatly enhance your search capabilities by allowing for pattern matching instead of exact names.

To illustrate their use, imagine we are searching for files with the following names in a directory: `hat`, `cat`, `bat`, `what`, and `mat`.

Here is how different wildcards would filter the results:

#### **1. The `?` (Question Mark) Wildcard**

The `?` matches **exactly one** character in a specific position.

- **Command:** `find / -type f -name "?at"`
    
- **Matches:** `hat`, `cat`, `bat`, `mat`
    
- **Does Not Match:** `what` (because `?` represents only one character, and `what` begins with two: `wh`)
    

#### **2. The `[ ]` (Square Bracket) Wildcard**

The `[ ]` matches **any one** of the characters listed inside the brackets.

- **Command:** `find / -type f -name "[cb]at"`
    
- **Matches:** `cat`, `bat`
    
- **Does Not Match:** `hat`, `mat`, `what` (because the first letter must be either `c` or `b`)
    

#### **3. The `*` (Asterisk) Wildcard**

The `*` matches **zero or more** of any character. It is the most broad and commonly used wildcard.

- **Command:** `find / -type f -name "*at"`
    
- **Matches:** `hat`, `cat`, `bat`, `mat`, `what` (matches any string ending in `at`)
    

[[19-find-wildcard-question-mark.png]]
[[20-find-wildcard-square-bracket.png]]

### **Filtering Output with `grep`**
The `grep` utility is used to search for specific **keywords** or **patterns** within text. Its power is often combined with other commands using a technique called **piping**.

**What is Piping?**  
In Linux (and Windows command line), you can take the output of one command and send it directly as the input to another command. This is done using the **pipe** operator: `|`.

**Practical Example: Finding a Specific Running Process**  
A common use case is filtering the list of all running processes to find a specific program.

1. The `ps` command lists processes. By itself, `ps` only shows processes for your current shell session.
    
2. To see **all** processes running on the system, use `ps aux`:
    
    - `a`: Show processes for **a**ll users.
        
    - `u`: Display the **u**ser/owner in a detailed format.
        
    - `x`: Include processes not attached to a terminal (e.g., background services).
        
3. The output of `ps aux` can be extensive. To filter it, pipe (`|`) the output into `grep` followed by your search term.
    

**Example: Find if `msf6` (Metasploit Framework) is running:**

```bash
ps aux | grep msf6
```

This command first gets the full process list (`ps aux`) and then passes it to `grep`, which displays only the lines containing "msf6".

[[21-ps-aux-msf6.png]]

**Note**: Metasploit Framework is known as ***`ruby`*** in the utility ***`top`*** (The Metasploit Framework process often appears as `ruby` since it's a Ruby application."). While we are in `top` we can look at `PID` of `ruby`and press `k`to command kill switch from `zsh`to Metasploit. 

[[22-top-msfconsole.png]]
[[23-kill-switch-msf.png]]

### **Modifying Files and Directories**

In this section, we will learn how to work with files and directories. We'll cover ways to create, copy, rename, and delete them.

**Creating Files**

Linux offers multiple utilities for creating files. We will look at two of the most common methods.

### **Creating a File with `cat`**

The `cat` command, short for **concatenate** (to combine or link together), is typically used to display the contents of a file. However, it can also create small files by redirecting your keyboard input.

> **Note:** For creating or editing larger files, it's better to use a dedicated text editor like `nano` or `vim`.

**To create a file with `cat`:**  
Use the `cat` command followed by a **redirect operator** (`>`) and the name of the file you want to create.

```bash
cat > hello_world.txt
```
After pressing **Enter**, the terminal enters an **interactive input mode**. You can start typing the content for your new file.

**To save the file and exit,** press **Ctrl+D** on a new line. This sends the "End of File" (EOF) signal, saving your input to `hello_world.txt` and returning you to the command prompt.

[[24-cat-basic.png]]

**To View a File's Contents:**  
This is the most common use of `cat`. Simply type `cat` followed by the filename.

``` bash
cat hello_world.txt
```

The entire contents of the file will be printed to the terminal.

[[25-cat-viewing-file.png]]

**To Append Content to an Existing File:**

``` bash
cat >> existingfile.txt
```
- Uses a double redirect (`>>`).
    
- **Adds new content to the end** of the file without deleting the old content.
    
- Press **Ctrl+D** to save the appended text and exit.

[[26-cat-appending.png]]

**To Overwrite an Existing One:**

``` bash
cat > myfile.txt
```

- Uses a single redirect (`>`).
    
- **Action:** If `myfile.txt` doesn't exist, it will be created. If it _does_ exist, it will be **completely overwritten**.
    
- Overwriting is identical to creating a new file—just use the same filename. The original content is lost.

[[27-cat-overwriting.png]]
### **Creating a File with `touch`**

The `touch` command is the second primary method for creating files. Its original purpose is to update a file's **access and modification timestamps** to the current time. However, if the specified file does not exist, `touch` will create an empty file by default.

**Syntax:**

``` bash
touch filename.txt
```

**Key Difference from `cat`:**

- **`cat > file`**: Opens an **interactive mode** for you to immediately enter content. It creates a file _with_ content.
    
- **`touch file`**: Creates an **empty file** instantly and returns you to the command prompt. No interactive mode is entered.
    

**In summary:** Use `touch` to quickly create one or multiple empty files. Use `cat >` when you want to create and write to a file in one step.

[[28-touch-command.png]]
[[29-touch-created-file.png]]

### **Creating a Directory**

To create a new directory, use the `mkdir` command (**m**a**k**e **dir**ectory).

**Basic Syntax:**

``` bash
mkdir directory_name
```

[[30-mkdir-command.png]]
[[31-mkdir-created.png]]

**Creating Nested Directories:**  
A powerful and frequently used option is `-p` (parents). It allows you to create a directory _and_ any necessary parent directories that don't already exist.

``` bash
mkdir -p projects/2024/hacking/reports
```

This single command creates the entire nested folder structure at once, instead of requiring you to create each level separately.

**Note:** The permissions of the new directory will depend on your current `umask` setting, but it's typically created with read, write, and execute permissions for the owner.

### **Copying a File**

The `cp` command is used to copy files from a source to a destination.

**Basic Syntax:**

``` bash
cp <source_file> <destination>
```

**Example 1: Copy to a Directory**  
To copy a file named `document.txt` to your Desktop directory, keeping the original filename:

``` bash
cp document.txt /home/kali/Desktop/
```

[[32-cp-command.png]]

**Example 2: Copy and Rename**  
To copy a file and give it a new name in the destination directory, specify the new filename as part of the destination path:

``` bash
cp document.txt /home/kali/Desktop/backup_document.txt
```

[[33-cp-backup.png]]

**Important Notes:**

- The destination directory (`/home/kali/Desktop/`) must exist before copying.
    
- If a file with the same name already exists in the destination, `cp` will **overwrite it silently**. Use the `-i` (interactive) flag to be prompted before overwriting: `cp -i source destination`.
    
- To copy a **directory** and all its contents, you must use the `-r` (recursive) flag: `cp -r sourcedir/ destination/`.

### **Renaming and Moving Files/Directories (with `mv`)**

The `mv` (**m**o**v**e) command serves a dual purpose: it can **rename** a file/directory in place, or **move** it to a new location. There is no separate "rename" command.

**Basic Syntax:**

``` bash
mv <source> <destination>
```

**How it works:** Conceptually, you are always "moving" the item from one _path_ to another. Changing its name in the same directory is simply moving it to a new path in that same location.

**Example 1: Renaming a File (Moving in Place)**  
To rename `oldname.txt` to `newname.txt` in your current directory:

``` bash
mv oldname.txt newname.txt
```

[[34-mv-rename.png]]

**Example 2: Moving (and Possibly Renaming)**  
To **move** a file to a different directory, specify the target path. You can choose to keep or change its name.

``` bash
# Move and keep the same name
mv file.txt /home/kali/Documents/

# Move AND rename in one step
mv file.txt /home/kali/Documents/report.txt
```

[[35-mv-moving-renaming.png]]

> **⚠️ Important:** Like `cp`, the `mv` command will **silently overwrite** an existing file at the destination if it has the same name. Use the `-i` (interactive) flag to get a warning prompt: `mv -i source destination`.

### **Removing Files and Directories**

#### **Removing Files with `rm`**

The `rm` (**r**e**m**ove) command deletes files permanently.

**Syntax:**

``` bash
rm filename
```

[[36-rm-command.png]]

> **⚠️ Critical Warning:** Unlike a graphical trash bin, `rm` typically **deletes files immediately and irreversibly**. Use it with extreme caution.

#### **Removing _Empty_ Directories with `rmdir`**

The `rmdir` (**r**e**m**ove **dir**ectory) command is a safe utility that **only removes empty directories**.

**Syntax:**

``` bash
rmdir directoryname
```

[[37-rmdir-command.png]]

This is a built-in safety measure to prevent accidental deletion of files.

#### **Removing Non-Empty Directories with `rm -r`**

To remove a directory **and all of its contents** (files and subdirectories), you must use `rm` with the `-r` (**r**ecursive) flag.

**Syntax:**

``` bash
rm -r directoryname
```

[[38-rm-recursive-directory.png]]

> **🚨 EXTREME CAUTION:** This command is **powerful and dangerous**. It will delete everything inside the specified directory without confirmation by default. There is no undo.

#### **Best Practices & Safety Tips**

- **Use `-i` for Interactive Deletion:** The `-i` flag prompts you for confirmation before deleting each file.
    
```bash
rm -i filename       # Confirm file deletion
rm -ri directoryname # Confirm each file in a directory
```
    
- **Test First with `ls`:** Before using `rm -r`, list the directory contents to ensure you're targeting the right place.
    
``` bash
ls -la directoryname/
```

[[39-creating-files-and-directories.png]]
[[40-rm-interactive-file.png]]
[[41-rm-interactive-recursive-directory.png]]

- **The Nuclear Option `rm -rf`:** Combining `-r` (recursive) with `-f` (**f**orce) makes the command skip any prompts and ignore errors. **`rm -rf` is famously dangerous and should be used with the utmost care.**
[[42-rm-recursive-force.png]]