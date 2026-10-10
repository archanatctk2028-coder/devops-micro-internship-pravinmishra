# Assignment 5 — Bash Script Automation Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will practice Bash scripting by building a series of small automation scripts covering environment setup, variables, arrays, loops, file conditionals, if-else logic, and functions. These scripts form the foundation of real-world Linux automation used in DevOps, cloud, and production support environments.

---

# Task 1 — Bash Environment & Workspace Setup

## Goal

Verify that Bash is available on your system and create a clean workspace for this assignment.

### Evidence

#### Screenshot 1 — Output of `echo $SHELL` and `bash --version`


<img width="1003" height="85" alt="Screenshot 2026-10-06 205807" src="https://github.com/user-attachments/assets/7f8cfe1d-1adf-4e3a-b5d0-1f162badc3b7" />


---

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory


<img width="1590" height="956" alt="image" src="https://github.com/user-attachments/assets/3faf8d91-a99f-4bee-b0c3-5cab4180adc4" />


---

### Notes

Answer the following in your own words:

**1. What is Bash?**

Bash (Bourne Again Shell) is a command-line shell used in Linux and Unix systems. It allows users to run commands, manage files, execute programs, and automate tasks using scripts. Bash is widely used in Linux administration, DevOps, and cloud computing.
---

**2. What is the difference between shell and Bash?**

A shell is a program that allows users to interact with the operating system using commands. Bash is one specific type of shell called Bourne Again Shell.

Shell: General term for command-line interpreters.
Bash: A specific and widely used shell.
Examples of shells: Bash, Zsh, Fish, and Sh.
Bash supports commands, scripting, variables, loops, and functions.

In short: Shell is a general category, while Bash is a specific type of shell.

---

**3. Why is it important to confirm the Bash version before writing scripts?**

It is important to confirm the Bash version because different Bash versions may support different features and syntax. Checking the version helps ensure that the script will run correctly and avoids compatibility errors.

Example: Some commands or features available in newer Bash versions may not work in older versions.

---

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

<img width="1595" height="982" alt="image" src="https://github.com/user-attachments/assets/386deb9c-fd5e-4317-ab9d-b976fde69d18" />

---

#### Screenshot 2 — Output of `./first-script.sh`

Add your screenshot here.

---

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

<img width="501" height="95" alt="image" src="https://github.com/user-attachments/assets/08868d36-6c33-4cc9-a0ed-dc2c95fd0350" />

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

#!/bin/bash is called a shebang or hashbang. It is usually written as the first line of a Bash shell script. Its purpose is to tell the operating system that the script should be executed using the Bash interpreter.

When we run a script, the operating system needs to know which program should interpret and execute the commands written inside it. The #!/bin/bash line specifies that Bash should be used.
---

**2. Why do we use `chmod +x` before running a script?**

chmod +x is a Linux command used to give execute permission to a file or script. In Linux, files have different permissions such as read (r), write (w), and execute (x). By default, a newly created script may not have permission to execute directly.
---

**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

./script.sh is used when the script is configured as an executable file, while bash script.sh directly invokes Bash to execute the script. Therefore, the main difference is how the script is executed and whether execute permission is required.

---

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`

Add your screenshot here.

---

#### Screenshot 2 — Output of `./user-info.sh`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

A variable in Bash is a named storage location used to hold a value, such as text, numbers, or command output. Variables allow a Bash script to store, access, and reuse information during execution. They make scripts more flexible and easier to manage.

For example, name="Archana" stores the value Archana in the variable name. The value can later be accessed using $name.

---

**2. Why should we avoid spaces around the `=` sign when creating variables?**

In Bash, spaces around the = sign are not allowed when assigning a value to a variable. Bash treats the assignment as a command when spaces are used, which causes an error.

---

**3. How do you access the value stored inside a Bash variable?**

In Bash, the value stored inside a variable is accessed by placing a $ symbol before the variable name. This tells Bash to replace the variable name with the value it contains.

---

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

Add your screenshot here.

---

#### Screenshot 2 — Output of `./tools-checklist.sh`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

An array in Bash is a variable that can store multiple values under a single variable name. Each value is stored at a specific index, starting from 0. Arrays are useful for storing and processing a list of related items, such as filenames, server names, or health checks.

---

**2. Why are arrays useful in scripts?**
Arrays are useful in Bash scripts because they allow multiple related values to be stored in a single variable. This makes it easier to manage lists of items such as server names, files, commands, or health checks. Arrays can be processed using loops, reducing the need to write the same code repeatedly. They make scripts more organized, efficient, reusable, and easier to maintain. In DevOps automation, arrays are especially useful for performing the same operation on multiple resources automatically.

---

**3. What does `"${tools[@]}"` mean?**

"${tools[@]}" is a Bash syntax used to access all elements of an array named `tools.

Explanation:
tools → The name of the array.
[@] → Refers to all elements in the array.
"..." → Keeps each array element as a separate argument, even if it contains spaces.

---

**4. What is the purpose of the `for` loop in this script?**

The purpose of the for loop in a Bash script is to execute a set of commands repeatedly for each element in a list or array. In this script, the for loop takes each element from the tools array one by one and processes it. It helps automate repetitive tasks, reduces code duplication, and makes the script easier to maintain.

---

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

Add your screenshot here.

---

#### Screenshot 2 — Output of `./counter.sh`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is a loop?**

A loop is a programming statement that repeats a set of instructions multiple times until a specified condition is met.

---

**2. Why do we use loops in Bash scripting?**

We use loops in Bash scripting to repeat a set of commands automatically multiple times, which saves time and reduces repetitive work.

---

**3. How many times did the loop run in your script?**
The loop ran 5 times, once for each item in the array.

The loop ran 5 times in my script, once for each item in the array.
---

**4. What would you change if you wanted the loop to run 10 times?**

I would add 5 more items to the array, so the loop runs 10 times.


---

# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

Add your screenshot here.

---

#### Screenshot 2 — Content of `file-check.sh`

Add your screenshot here.

---

#### Screenshot 3 — Output of `./file-check.sh`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

d is a Bash file test operator used to check whether a specified path exists and is a directory. If the path is a directory, the condition returns true; otherwise, it returns false.
---

**2. What does `-f` check in Bash?**

-f is a Bash file-test operator used to check whether a specified path exists and is a regular file. It returns true if the file exists and is not a directory.

---

**3. Why should file and directory paths be stored in variables?**

File and directory paths should be stored in variables because it makes Bash scripts easier to read, reuse, and modify.

4-mark answer:

It avoids repeating the same path multiple times.
It makes the script easier to understand.
If the path changes, we only need to change it in one place.
It reduces typing mistakes and makes the script easier to maintain.

---

**4. What happens if the file does not exist?**

If the file does not exist, -f returns false. Bash then executes the else block, if present. This helps the script safely check for a file before trying to use it.

---

# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

Add your screenshot here.

---

#### Screenshot 2 — Output showing `Result: Pass`

Add your screenshot here.

---

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

Add your screenshot here.

---

#### Screenshot 4 — Output showing `Result: Retry`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

The if-else statement in Bash is used to make decisions based on a condition.

4-mark answer:

if checks whether a condition is true.
If the condition is true, the if block runs.
If the condition is false, the else block runs.
It helps scripts make decisions automatically.

---

**2. What does `-ge` mean?**

-ge is a Bash numeric comparison operator that means greater than or equal to. It checks whether the first number is greater than or equal to the second number.

---

**3. Why should conditions be tested with different values?**

It checks whether the condition works correctly.
It helps find errors or bugs in the script.
It tests different cases, such as true and false conditions.
It makes the script more reliable and accurate.

Example: If using -ge 18, test with 18, 20, and 15 to check both true and false results.

---

**4. How can conditionals help in automation scripts?**

Conditionals help automation scripts make decisions automatically based on different condition

They allow scripts to check whether a condition is true or false.
They can perform different actions based on the result.
They help handle errors and unexpected situations.
They make automation scripts more reliable and efficient.

Example: A script can check whether a file exists and create it only if it is missing.

---

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

Add your screenshot here.

---

#### Screenshot 2 — Output of `./final-automation.sh`

Add your screenshot here.

---

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

A function in Bash is a block of commands grouped together under a name. It can be called whenever we need to perform the same task.
---

**2. Why are functions useful in scripts?**

They avoid repeating the same commands.
They allow us to reuse code multiple times.
They make scripts easier to read and understand.
They make debugging and maintaining scripts easier.

Example: A function that checks server status can be called whenever needed instead of writing the same commands repeatedly.

---

**3. Which functions did you create in this script?**

They avoid repeating the same commands.
They allow us to reuse code multiple times.
They make scripts easier to read and understand.
They make debugging and maintaining scripts easier.

Example: A function that checks server status can be called whenever needed instead of writing the same commands repeatedly.
---

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

Variables store important values such as file or directory paths.
Arrays store multiple items, such as a list of tools.
Loops repeat commands for each item in the array.
Conditionals check conditions, while files store or provide data, and functions group reusable commands.

Together, these features make the script organized, reusable, and automated.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot — Published LinkedIn post

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- All script files must be created and run successfully
- Required notes must be answered clearly for every task
- Do not expose sensitive information (keys, passwords, credentials)

---

# Completion Checklist

- [ ] Task 1: Environment setup verified, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: First script created, executed, permissions verified (Screenshots 1–3, Notes answered)
- [ ] Task 3: Variables script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 4: Arrays and loops script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 5: Counter loop script created and run (Screenshots 1–2, Notes answered)
- [ ] Task 6: File validation script created and run (Screenshots 1–3, Notes answered)
- [ ] Task 7: Pass/Retry conditional script tested with both values (Screenshots 1–4, Notes answered)
- [ ] Task 8: Final automation script created and run (Screenshots 1–3, Notes answered)
- [ ] All scripts run without errors
- [ ] Full Name visible in all required screenshots
- [ ] LinkedIn post published and URL submitted
- [ ] No sensitive data exposed

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.

---

## 📌 Resources

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
