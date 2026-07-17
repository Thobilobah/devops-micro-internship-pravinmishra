# Assignment 5 — Bash Script Automation Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will practice Bash scripting by building a series of small automation scripts covering environment setup, variables, arrays, loops, file conditionals, if-else logic, and functions. These scripts form the foundation of real-world Linux automation used in DevOps, cloud, and production support environments.

---

# Task 1 — Bash Environment & Workspace Setup

## Goal

Verify that Bash is available on your system and create a clean workspace for this assignment.

### Evidence

#### Screenshot 1 — Output of `echo $SHELL` and `bash --version`

![Image of `echo $SHELL` and `bash --version`](/week-03-linux-for-devops/screenshots/Screenshot-46-wk3.png)

---

#### Screenshot 2 — Output of `pwd` and `ls -lah` showing the scripts directory

![Image of pwd` and `ls -lah` showing the scripts directory](/week-03-linux-for-devops/screenshots/Screenshot-47-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is Bash?**

Bash is a command-line shell and scripting language commonly used on Linux and Unix systems. It allows users to interact with the operating system, run commands, automate repetitive tasks, and manage files and processes through scripts.

---

**2. What is the difference between shell and Bash?**

A shell is a program that provides a way for users to communicate with the operating system through commands. Bash is one specific type of shell that offers additional features such as command history, scripting, command completion, and improved automation. In simple terms, all Bash programs are shells, but not all shells are Bash.

---

**3. Why is it important to confirm the Bash version before writing scripts?**

It's important to confirm the Bash version because some scripting features are only available in newer versions of Bash. Checking the version helps ensure that the script will run correctly on the target system and avoids compatibility issues caused by unsupported commands or syntax.

---

# Task 2 — Your First Bash Script

## Goal

Create your first Bash script, make it executable, and run it from the terminal.

### Evidence

#### Screenshot 1 — Content of `first-script.sh`

![Image of `first-script.sh`](/week-03-linux-for-devops/screenshots/Screenshot-48-wk3.png)

---

#### Screenshot 2 — Output of `./first-script.sh`

![Image of ./first-script.sh`](/week-03-linux-for-devops/screenshots/Screenshot-49-wk3.png)

---

#### Screenshot 3 — Output of `ls -l first-script.sh` showing executable permission

![Image output of `ls -l first-script.sh` showing executable permission ](/week-03-linux-for-devops/screenshots/Screenshot-49a-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of `#!/bin/bash`?**

```#!/bin/bash``` is called a shebang. It tells the operating system to use the Bash interpreter to execute the script. This ensures the script runs with Bash, even if the user's default shell is something different.

---

**2. Why do we use `chmod +x` before running a script?**

The ```chmod +x ```command makes a script executable by giving it execute permission. Without this permission, the operating system will not allow the script to be run directly using ```./script.sh.```

---

**3. What is the difference between running a script using `./script.sh` and `bash script.sh`?**

When you run a script with ```./script.sh```, the system executes the script directly using the interpreter specified in the shebang, and the script must have execute permission. When you run `bash script.sh`, you explicitly tell Bash to interpret the script, so it can run even if the script doesn't have execute permission, as long as you have permission to read the file.

---

# Task 3 — Variables: User Information Script

## Goal

Use variables to store and display user-related information.

### Evidence

#### Screenshot 1 — Content of `user-info.sh`



![Image of Content of `user-info.sh`](/week-03-linux-for-devops/screenshots/Screenshot-50-wk3.png)

#### Screenshot 2 — Output of `./user-info.sh`

![Image of output of `user-info.sh`](/week-03-linux-for-devops/screenshots/Screenshot-51-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is a variable in Bash?**

A variable in Bash is a named placeholder used to store a value, such as text, numbers, or the output of a command. It allows you to reuse that value throughout a script without having to type it repeatedly.

---

**2. Why should we avoid spaces around the `=` sign when creating variables?**

Spaces should not be used around the `= `sign because Bash will treat the variable name, the =, and the value as separate commands or arguments. This results in a syntax error instead of creating the variable.

---

**3. How do you access the value stored inside a Bash variable?**

You access the value of a Bash variable by placing a dollar sign (`$`) before the variable name. For example, if the variable is name, you can display its value using echo `$name`.

---

# Task 4 — Arrays & Loops: Tools Checklist Script

## Goal

Use arrays and loops to print a checklist of tools used in Bash scripting.

### Evidence

#### Screenshot 1 — Content of `tools-checklist.sh`

![Image of Content of `tools-checklist.sh`](/week-03-linux-for-devops/screenshots/Screenshot-52-wk3.png)

---

#### Screenshot 2 — Output of `./tools-checklist.sh`

![Image of Output of `tools-checklist.sh`](/week-03-linux-for-devops/screenshots/Screenshot-53-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is an array in Bash?**

An array in Bash is a variable that can store multiple values under a single name. Instead of creating separate variables for related items, you can keep them together in one array and access each value when needed.

---

**2. Why are arrays useful in scripts?**

Arrays are useful because they make it easier to work with a group of related values. They help reduce repetitive code and allow you to process multiple items efficiently using loops.

---

**3. What does `"${tools[@]}"` mean?**

`"${tools[@]}"` represents all the elements stored in the `tools` array. It is commonly used to loop through or display every item in the array while preserving each element as a separate value.

---

**4. What is the purpose of the `for` loop in this script?**

The `for` loop is used to go through each item in the array one at a time and perform the same action on each one. This makes it easy to process multiple values without writing the same code repeatedly.

---

# Task 5 — Loops: Number Counter Script

## Goal

Use loops to repeat a task multiple times.

### Evidence

#### Screenshot 1 — Content of `counter.sh`

![Image Content of `counter.sh`](/week-03-linux-for-devops/screenshots/Screenshot-54-wk3.png)

---

#### Screenshot 2 — Output of `./counter.sh`

![Image Output of `counter.sh`](/week-03-linux-for-devops/screenshots/Screenshot-55-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is a loop?**

A loop is a programming structure that repeats a block of code multiple times until a specified condition is met or all items have been processed. It helps automate repetitive tasks in a script.

---

**2. Why do we use loops in Bash scripting?**

Loops are used in Bash scripting to perform the same task repeatedly without writing the same code multiple times. They make scripts shorter, easier to maintain, and more efficient.

---

**3. How many times did the loop run in your script?**

The loop ran 5 times because it iterated through the five values (1, 2, 3, 4, and 5) listed in the `for` loop. During each iteration, it printed a message showing the current step.

---

**4. What would you change if you wanted the loop to run 10 times?**

I would update the for loop to include the numbers 1 through 10 instead of 1 through 5. For example:
```bash
for number in 1 2 3 4 5 6 7 8 9 10
```

---

# Task 6 — Files & Conditionals: File Validation Script

## Goal

Use file checks and conditionals to verify whether files and directories exist.

### Evidence

#### Screenshot 1 — Output of `ls -lah ../test-folder`

![Image Output of `ls -lah ../test-folder`](/week-03-linux-for-devops/screenshots/Screenshot-56-wk3.png)

---

#### Screenshot 2 — Content of `file-check.sh`

![Image Content of `file-check.sh`](/week-03-linux-for-devops/screenshots/Screenshot-57-wk3.png)

---

#### Screenshot 3 — Output of `./file-check.sh`

![Image Output of `file-check.sh`](/week-03-linux-for-devops/screenshots/Screenshot-58-wk3.png)


---

### Notes

Answer the following in your own words:

**1. What does `-d` check in Bash?**

The `-d` test checks whether a specified path exists and is a directory. It returns true if the directory exists and false if it does not.

---

**2. What does `-f` check in Bash?**

The `-f` test checks whether a specified path exists and is a regular file. It returns true only if the file exists and is not a directory or another type of file.

---

**3. Why should file and directory paths be stored in variables?**

Storing file and directory paths in variables makes scripts easier to read, maintain, and update. If the path changes, you only need to update it in one place instead of changing it throughout the entire script.

---

**4. What happens if the file does not exist?**

If the file does not exist, the `-f ` test returns false, allowing the script to handle the situation appropriately. For example, it can display an error message, skip the operation, or take another action instead of failing unexpectedly.

---

# Task 7 — Conditionals: Pass or Retry Script

## Goal

Use if-else conditionals to make decisions based on a variable value.

### Evidence

#### Screenshot 1 — Content of `score-check.sh` with `score=85`

![Image Content of `score-check.sh` with `score=85`](/week-03-linux-for-devops/screenshots/Screenshot-59-wk3.png)

---

#### Screenshot 2 — Output showing `Result: Pass`

![Image Output showing `Result: Pass`](/week-03-linux-for-devops/screenshots/Screenshot-60-wk3.png)

---

#### Screenshot 3 — Content of `score-check.sh` with `score=55`

![Image Content of `score-check.sh` with `score=55`](/week-03-linux-for-devops/screenshots/Screenshot-61-wk3.png)

---

#### Screenshot 4 — Output showing `Result: Retry`

![Image Output showing `Result: Pass`](/week-03-linux-for-devops/screenshots/Screenshot-62-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is the purpose of if-else in Bash?**

The `if-else` statement is used to make decisions in a Bash script. It allows the script to execute different blocks of code depending on whether a condition is true or false.

---

**2. What does `-ge` mean?**

The `-ge` operator means greater than or equal to. It is used to compare two numbers and checks if the first number is greater than or equal to the second.

---

**3. Why should conditions be tested with different values?**

Testing conditions with different values helps ensure that the script behaves correctly in different situations. It allows you to verify that both the true and false conditions work as expected and helps identify any errors before the script is used.

---

**4. How can conditionals help in automation scripts?**

Conditionals make automation scripts more intelligent by allowing them to make decisions based on different situations. For example, a script can check if a file exists before using it, verify whether a service is running before restarting it, or handle errors automatically without requiring manual intervention.

---

# Task 8 — Functions: Final Bash Automation Script

## Goal

Create a final Bash script using functions to organize reusable code.

### Evidence

#### Screenshot 1 — Content of `final-automation.sh`

![Content of `final-automation.sh`](/week-03-linux-for-devops/screenshots/Screenshot-63-wk3.png)

---

#### Screenshot 2 — Output of `./final-automation.sh`

![Outpou of `final-automation.sh`](/week-03-linux-for-devops/screenshots/Screenshot-64-wk3.png)

---

#### Screenshot 3 — Output of `ls -lah` showing all created scripts

![Output of `ls -lah` showing all created scripts](/week-03-linux-for-devops/screenshots/Screenshot-65-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is a function in Bash?**

A function in Bash is a reusable block of code that performs a specific task. Instead of writing the same commands multiple times, you can define them once in a function and call the function whenever you need it.

---

**2. Why are functions useful in scripts?**

Functions make scripts easier to read, organize, and maintain. They reduce repeated code, make updates simpler, and allow different parts of a script to be reused without rewriting the same commands.

---

**3. Which functions did you create in this script?**
In this script, I created four functions: `print_header()`, `print_user_details()`, `check_files()`, and `print_tools()`. Each function has a specific purpose, such as displaying the script header, printing my details, checking that the required files and directories exist, and listing the tools used in the assignment.

---

**4. How does this final script combine variables, arrays, loops, conditionals, files, and functions?**

This script combines several Bash concepts to automate different tasks. It uses variables to store information such as my name and file paths, an array to store the list of tools, a for loop to print each tool, conditionals (`if` statements) to check whether the required directory and file exist, file and directory paths to verify resources, and functions to organize the script into reusable sections. Combining these features makes the script cleaner, more efficient, and easier to maintain.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

<<<<<<< HEAD:week-03-linux-for-devops/assignment-05-bash-script-automation-drill-ops-checklist.md
https://www.linkedin.com/posts/oluwatobiloba-taiwo_bash-linux-shellscripting-ugcPost-7483665158704349184-6gN6/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACz7IugBIR_XEr4WBbn9LYa6OeAS8fYZbYA
=======
`Add your URL here`
>>>>>>> upstream/main:week-03-linux-and-bash-for-devops/assignment-05-bash-script-automation-drill-ops-checklist.md

---

#### Screenshot — Published LinkedIn post

![Published Linkedin Post](/week-03-linux-for-devops/screenshots/Screenshot-66-wk3.png)

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

- 🌐 DMI Official Website: https://pravinmishra.com/dmi  
- 🎓 DevOps for Beginners (Udemy): https://www.udemy.com/course/devops-for-beginners-docker-k8s-cloud-cicd-4-projects/  
- 🎓 Agentic AI DevOps with Claude Code: https://www.udemy.com/course/ultimate-agentic-ai-devops-with-claude-code/  
- 🎓 DevOps with Claude Code: Terraform, EKS, ArgoCD & Helm: https://www.udemy.com/course/devops-with-claude-code-terraform-eks-argocd-helm/  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*