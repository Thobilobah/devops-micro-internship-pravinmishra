# Week 00 - Internet and Networking

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

# 🧑‍💻 Task 1: Using ChatGPT as Your Learning Assistant

## Scenario

You're new to DevOps and will frequently encounter technical questions. ChatGPT can be your learning companion.

## Your Task

Write a clear ChatGPT prompt to help you understand:

> "What is a protocol in networking? Explain with a simple real-life example."

Take a screenshot of your interaction showing:

* Your detailed prompt (with clear expectations)
* ChatGPT's simplified response with an example

## Screenshot

Save your screenshot in the `screenshots` folder and update the file name below.

![Task 1 Screenshot](/week-00-internet-and-networking/screenshots/chatgpt-prompt.png)


Replace `task-1-chatgpt.png` with your actual screenshot file name.

---

## What I Learned (2–3 lines)

This task helped me better understand how networking concepts apply to real-world communication. It also improved my ability to explain technical ideas in a simple and relatable way. Overall, it strengthened both my technical understanding and communication skills.


---

# 🌐 Task 2: Internet and Networking

## Scenario

Your friend is launching an online bookstore named **EpicReads**.

He asked you to explain how users globally can access his website hosted in Finland.

## Your Task

Write a short explanation (**100–150 words**) that includes:

* Packet Switching
* IP Address
* TCP/IP
* HTTP/HTTPS

💡 **Tip:** You may use ChatGPT (as demonstrated in Task 1) to refine your explanation.

## Answer

When a user visits EpicReads from anywhere in the world, their request is broken into small pieces using packet switching. These packets travel across different networks to reach the server in Finland. Each device connected to the internet has a unique IP address, which helps route the request to the correct server hosting the website. Communication between the user’s device and the server is governed by TCP/IP, which ensures the data is sent reliably and reassembled correctly. Once the request reaches the server, HTTP or HTTPS is used to transfer the webpage data back to the user’s browser. HTTPS adds a layer of security by encrypting the data, ensuring safe communication. Together, these technologies make it possible for users globally to access EpicReads seamlessly.


---

# 🏗️ Task 3: Application Architecture & Stack

## Scenario

EpicReads bookstore has two application versions:

### Two-Tier Application

* Frontend
* Database

### Three-Tier Application

* Frontend
* Backend
* Database

## Your Task

* Draw simple diagrams (hand-drawn or tool-based such as draw.io)
* Label each layer clearly
* List at least two common technologies or tools used for each layer
* Submit a screenshot or photo clearly showing your own drawing

## Diagram Screenshot / Photo

Save your diagram image in the `screenshots` folder and update the file name below.

![Application Architecture Diagram](/week-00-internet-and-networking/screenshots/My-app-architecture.png)


Replace `task-3-diagram.png` with your actual diagram file name.

---

## Technologies Used

### Frontend

* HTML
* CSS

### Backend

* Nodejs
* .Net

### Database

* Mysql
* Postgresql

---

# 🌍 Task 4: Domain Name & DNS (Basic Concepts)

## Scenario

Your friend's bookstore **EpicReads** is currently accessible through:

```text
52.172.142.222:3000
```

He purchased the domain:

```text
epicreads.com
```

## Your Task

In **50–100 words**, explain in your own words:

1. What is DNS (Domain Name System)?
2. Which DNS record type should be used to connect the domain to the given IP, and why?

## Answer

The Domain Name System (DNS) is like the internet’s phonebook. It translates human-friendly domain names like epicreads.com into IP addresses such as 52.172.142.222, which computers use to locate servers.
To connect the domain to the IP, an A record should be used. This is because an A record directly maps a domain name to an IPv4 address, ensuring that when users enter epicreads.com, they are routed to the correct server hosting the application.


---

# 💻 Task 5: Visual Studio Code Setup (Hands-on)

## Your Task

Install Visual Studio Code (if not already installed).

Take a screenshot of your VS Code environment showing:

* Terminal open inside VS Code
* Running a basic command:

### Windows

```powershell
dir
```

### Linux / macOS

```bash
pwd
ls
```

* Your selected VS Code theme clearly visible

⚠️ **Important:** The screenshot must show your username or another identifiable detail to confirm it is your environment.

## Screenshot

Save your screenshot in the `screenshots` folder and update the file name below.

![VS Code Setup Screenshot](/week-00-internet-and-networking/screenshots/vscode-setup.png)


Replace `task-5-vscode.png` with your actual screenshot file name.

---

# 🔗 Task 6: Publish Your Assignment as a LinkedIn Post

## Objective

Publishing on LinkedIn helps you:

* Build your professional online presence
* Reinforce your learning
* Document your DevOps journey publicly

## Your Task

Summarize your answers from Tasks 1–5 into a LinkedIn post.

Clearly structure your post into the following sections:

* ChatGPT
* Internet & Networking
* App Architecture
* DNS
* VS Code Setup

Add the following credit note at the end of your post:

> **P.S. This post is part of the DevOps Micro Internship (DMI) with Agentic AI — Cohort 3 — by Pravin Mishra. My graded progress is public: https://dmi.pravinmishra.com/s/YOUR-GITHUB-USERNAME.html · Start your DevOps journey: https://dmi.pravinmishra.com/?utm_source=student&utm_medium=ps-linkedin&utm_campaign=cohort3**

---

## LinkedIn Post URL

https://www.linkedin.com/posts/oluwatobiloba-taiwo_epic-reads-shop-young-adult-ya-books-activity-7457178625457414144-EcD2?utm_source=share&utm_medium=member_desktop&rcm=ACoAACz7IugBIR_XEr4WBbn9LYa6OeAS8fYZbYA

---

## LinkedIn Post Backup Copy

🚀 Excited to share my learnings from Tasks 1–5 of the DevOps Micro Internship Cohort!
Here is a quick summary of what I explored and understood:
✅ 1. ChatGPT
ChatGPT is an AI tool that helps generate answers, explain concepts, and solve problems.
I used it to learn faster, troubleshoot issues, and understand DevOps concepts from different angles.
🌍 2. Internet & Networking
A protocol is a set of rules that devices follow to communicate.
For example, when you open a website, HTTP or HTTPS ensures your browser and the server understand each other.
In a real scenario like EpicReads, requests are broken into packets, sent using TCP/IP to the server’s IP address, and returned securely via HTTP or HTTPS.
🏗 3. App Architecture (EpicReads Bookstore)
Two-Tier (Frontend + Database):
Frontend: HTML, CSS, JavaScript, React
Database: MySQL, PostgreSQL, MongoDB
Three-Tier (Frontend + Backend + Database):
Frontend: HTML, CSS, JavaScript, React
Backend: Node.js, .NET, Django
Database: MySQL, PostgreSQL, MongoDB
Three-tier architecture is more scalable, secure, and easier to maintain.
🌐 4. DNS (Domain Name System)
DNS works like the internet’s phonebook. It converts domain names like epicreads.com into IP addresses.
An A record is used to connect the domain to an IPv4 address, making the website accessible to users.
💻 5. VS Code Setup
I used VS Code to set up my project environment and manage files.
I worked with the integrated terminal (PowerShell) to run commands and organize my workflow.
Extensions like Live Server and Prettier helped improve productivity.
🎯 Each task helped me better understand core DevOps and networking concepts while strengthening my hands-on skills.
P.S. This post is part of the FREE DevOps Micro Internship Cohort run by Pravin Mishra. You can start your DevOps journey for free from his YouTube Playlist.

> **P.S. This post is part of the DevOps Micro Internship (DMI) with Agentic AI — Cohort 3 — by Pravin Mishra. My graded progress is public: https://lnkd.in/eq3NrvMj · Start your DevOps journey: https://lnkd.in/e8xfwJvy
hashtag#DevOps hashtag#LearningInPublic hashtag#Internship hashtag#Cloud hashtag#Networking hashtag#Linux hashtag#AWS hashtag#Growth

---

# Reflection – Week 0

### What did you find easy?

I found Tasks 1 and 2 pretty easy

---

### What was difficult?

I found using the draw.io a little difficult at first

---

### What will you improve next week?

I will try to focus on Success mindset and getting the best of the mentorship

---

## 📌 About DMI & CloudAdvisory

DevOps Micro Internship (DMI) is a project-based DevOps program run by Pravin Mishra (The CloudAdvisory) focused on real-world execution, systems thinking, and career readiness.

It helps learners build strong DevOps foundations with hands-on experience.


## 📌 Resources

- 🌐 **DMI Official Website:** https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 **University:** https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 **Discord Community:** https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 **Blog:** https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ **YouTube Playlist (DMI Cohort 3):** https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 **Pravin Mishra (LinkedIn):** https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 **CloudAdvisory (LinkedIn):** https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track*