# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

![Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`](/week-03-linux-for-devops/screenshots/Screenshot-67-wk3.png)

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

![Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure](/week-03-linux-for-devops/screenshots/Screenshot-68-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

Nginx is confirmed to be running because the command `systemctl is-active nginx` returned active. The health report also showed "[PASS] Nginx service is active", confirming that the web server was running normally.

---

**2. What proves that the server is listening for HTTP traffic?**

The server is listening for HTTP traffic because the health check confirmed that Port 80 is listening, and `curl -I http://localhost` returned HTTP/1.1 200 OK. This shows that Nginx was accepting HTTP connections and successfully responding to requests.

---

**3. Why must you capture a healthy baseline before simulating an incident?**

Capturing a healthy baseline provides a known good state to compare against after an incident is simulated. It helps identify exactly what changed, confirms that the failure was caused by the simulation, and makes it easier to verify that the recovery restored the system to its original healthy state.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

![Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)](/week-03-linux-for-devops/screenshots/Screenshot-69-wk3.png)

---

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

Project-specific operational rules ensure that Claude analyzes incidents in a way that follows the team's established workflow and safety requirements. They keep Claude focused on using the Bash report as evidence, avoiding unsafe actions, and providing recommendations that are consistent with how the project is managed.

---

**2. Why is the human required to execute the recovery command?**

The human is required to execute the recovery command because recovery actions can directly affect the system. According to the project's safety rules, Claude should only recommend a recovery command after analyzing the evidence, while the human reviews, approves, and executes it. This adds a layer of oversight and helps prevent accidental changes or service disruptions.

---

**3. Which rule prevents Claude from making an unsupported diagnosis?**

The rule "Do not claim a root cause unless the report contains supporting evidence" prevents Claude from making an unsupported diagnosis. It ensures that conclusions are based only on the evidence collected in the Bash report rather than assumptions or guesses, leading to more accurate and reliable incident analysis.

---

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

![Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results](/week-03-linux-for-devops/screenshots/Screenshot-70a-wk3.png)
![Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results](/week-03-linux-for-devops/screenshots/Screenshot-70b-wk3.png)


---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

The Gather phase is where Claude creates the incident-triage plan and identifies the five read-only checks to collect system information. These checks include verifying the Nginx service status, checking if port 80 is listening, testing the localhost HTTP response, reviewing disk usage, and checking available memory. At this stage, no changes are made to the server—only evidence is gathered for analysis.

---

**2. Did Claude follow the instruction not to create files? How did you verify this?**

Yes. Claude followed the instruction not to create or edit any files. Instead of generating scripts or modifying the server, it only read the `CLAUDE.md` instructions and produced a step-by-step incident-triage plan. It also ended by asking me to run the five checks and share the output, which confirmed that it stayed within the project's read-only rules.

---

**3. Why is planning before coding useful in DevOps automation?**

Planning before coding helps ensure that the correct approach is taken before making any changes to a system. By gathering evidence first and deciding what needs to be checked, it reduces the risk of mistakes, avoids unnecessary changes, and makes troubleshooting more structured. This approach leads to safer, more reliable automation and aligns with DevOps best practices of validating before acting.

---

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

![creenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array](/week-03-linux-for-devops/screenshots/Screenshot-71-wk3.png)

---

#### Screenshot 6 — Middle section showing check functions and conditionals

![Screenshot 6 — Middle section showing check functions and conditionals](/week-03-linux-for-devops/screenshots/Screenshot-72a-wk3.png)

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

![Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior](/week-03-linux-for-devops/screenshots/Screenshot-72b-wk3.png)

---

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

![Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission](/week-03-linux-for-devops/screenshots/Screenshot-73-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

The `checks` array stores the names of all the health check functions that the script needs to run. These functions are `check_service`, `check_port`, `check_http`, `check_disk`, and `check_memory`, and together they perform the main system health checks for the server.

---

**2. How does the `for` loop use that array?**

The `for` loop goes through each function name stored in the `checks` array one at a time and executes it. This allows the script to run all the health checks in sequence without having to call each function individually, making the code cleaner and easier to maintain.

---

**3. Why are the health checks separated into functions?**

Separating the health checks into functions makes the script more organized and easier to understand. Each function is responsible for checking one specific part of the system, such as the Nginx service, HTTP response, or disk usage. This also makes it easier to update or add new health checks without affecting the rest of the script.

---

**4. What is the purpose of `$(...)` in this script?**

The `$(...)` syntax is used for command substitution. It runs a command and stores its output so the script can use it later. For example, it captures the current date, hostname, disk usage, available memory, and HTTP status code, allowing the script to include this information in the health report and make decisions based on the results.

---

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

The script uses different exit codes to clearly indicate the overall health of the system. An exit code of 0 means everything is healthy, 1 means there are warnings that should be reviewed, and 2 means one or more critical checks have failed. Using different exit codes makes it easy for other scripts, automation tools, or CI/CD pipelines to detect the system's status and respond appropriately.

---

# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

![Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results](/week-03-linux-for-devops/screenshots/Screenshot-74-wk3.png)

---

#### Screenshot 10 — Output showing the captured exit code and final summary

![Screenshot 10 — Output showing the captured exit code and final summary](/week-03-linux-for-devops/screenshots/Screenshot-75-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

The overall status of my healthy baseline is WARN. Most of the health checks passed successfully, including the Nginx service, port 80, the local HTTP check, and available memory. However, the root disk usage reached 80%, which met the warning threshold set in the script, so the overall status was reported as WARN instead of HEALTHY.

---

**2. Which exact Linux evidence proves the application is serving traffic?**

The strongest evidence is:
```
[PASS] Local HTTP check returned status 200
```

This shows that the application responded with an HTTP 200 OK status when the script sent a request to http://localhost, confirming that Nginx is running and successfully serving the application.

---

**3. Did your script return exit code 0 or 1? Explain why.**

The script returned exit code 1 because there was one warning. Although there were no failures, the root disk usage was exactly 80%, which matched the script's warning threshold. Since the script detected a warning, it reported an overall status of WARN and returned exit code 1.

---

**4. What is the difference between a warning and a failure in this script?**

In this script, a warning means the system is still working but there is something that should be monitored or addressed before it becomes a bigger problem. A failure means a critical health check did not pass, such as Nginx not running, port 80 not listening, or the HTTP check failing. Warnings indicate potential issues, while failures indicate problems that require immediate attention.

---

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

![Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules](/week-03-linux-for-devops/screenshots/Screenshot-76-wk3.png)

---

#### Screenshot 12 — `/linux-triage` output for the healthy server

![Screenshot 12 — `/linux-triage` output for the healthy server](/week-03-linux-for-devops/screenshots/Screenshot-77-wk3.png)

---

### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

This skill is designed to inspect the server without making any changes. Bash is used to run the health-check script, Read is used to view the generated report and project instructions, and Grep can be used to search for specific information in the report if needed. It does not include Write because the goal is to analyze the system safely without creating or modifying files.

---

**2. Why is `disable-model-invocation: true` useful for this skill?**

Setting `disable-model-invocation: true` helps ensure the skill follows the predefined workflow instead of allowing the model to improvise or take unnecessary actions. It keeps the process consistent by relying on the Bash script to collect evidence first and then analyzing the results according to the project's safety rules.

---

**3. What part is performed by Bash, and what part is performed by Claude?**

Bash is responsible for running the Linux health-check script and collecting system evidence, such as the status of Nginx, port 80, disk usage, memory, and the HTTP response. Claude then reads the generated report, analyzes the results, identifies any warnings or failures, explains the most likely cause based on the evidence, and recommends a safe recovery command for me to review and execute manually.

---

**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

This approach is more reliable because Claude bases its conclusions on real system data instead of making assumptions. The health-check script gathers objective evidence from the server, and Claude analyzes that evidence to provide an informed assessment. This reduces the risk of incorrect diagnoses and ensures that any recommendations are supported by actual system information.

---

# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

![Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails](/week-03-linux-for-devops/screenshots/Screenshot-78-wk3.png)

---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

![Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command](/week-03-linux-for-devops/screenshots/Screenshot-79-wk3.png)

---

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

![Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name](/week-03-linux-for-devops/screenshots/Screenshot-80-wk3.png)

---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

The three checks that failed were the Nginx service check, the port 80 listening check, and the local HTTP check. These failures show that Nginx was not running, port 80 was not accepting connections, and the web application was unavailable.

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

There are several pieces of evidence that support this conclusion. The report shows `[FAIL] Nginx service is not active`, [`FAIL] Port 80 is not listening`, and `[FAIL] Local HTTP check returned status 000`, which means the server could not establish an HTTP connection. The recent Nginx logs also show that the service was intentionally stopped:

`Stopping nginx.service`
`nginx.service: Deactivated successfully`
`Stopped nginx.service`

---

**3. Did Claude execute the recovery command? Why is that important?**

No. Claude did not execute the recovery command. Instead, it recommended `sudo systemctl start nginx` and asked me to review and run it manually. This is important because it follows the project's safety rules by leaving all recovery actions under human control, reducing the risk of making unintended changes to the server.

---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

The Bash report represents the Gather phase of the Agentic Loop. It collects evidence about the system, including the service status, port availability, HTTP response, disk usage, memory, and recent logs, without making any changes to the server.

---

**5. Which phase is represented by Claude's explanation?**

Claude's explanation represents the Analyze phase of the Agentic Loop. It reviews the evidence collected by the Bash script, identifies the failed checks, determines the most likely cause based on the report, and recommends a safe recovery command for the human to execute.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

![Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK](/week-03-linux-for-devops/screenshots/Screenshot-81-wk3.png)

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

![Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results](/week-03-linux-for-devops/screenshots/Screenshot-82-wk3.png)


---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

![Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`](/week-03-linux-for-devops/screenshots/Screenshot-83-wk3.png)


---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

![Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name](/week-03-linux-for-devops/screenshots/Screenshot-84-wk3.png)


---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

I manually reviewed the AI's recommended recovery command and started the Nginx service by running:

```bash
sudo systemctl start nginx
```
After that, I verified that the service was running and responding correctly.

---

**2. What evidence proves that the service recovered?**

The recovery was confirmed by multiple pieces of evidence:

`systemctl is-active` nginx returned active, confirming the Nginx service was running.
`curl -I http://localhost` returned HTTP/1.1 200 OK, proving the web server was successfully serving requests.
The second triage report showed the service, port 80, and HTTP checks all passed, with only a disk usage warning remaining.

---

**3. Why is the second triage run necessary?**

The second triage run verifies that the manual recovery actually fixed the problem. It confirms the system's current health after the change and ensures there are no remaining failures before considering the incident resolved.

---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

Automatically restarting services could hide the real root cause, interrupt running applications, cause repeated failures, or make an incident worse. Some failures require investigation rather than an immediate restart, which is why human approval is important before making changes.

---

**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

AA chatbot mainly answers questions, while an agentic AI follows a structured workflow by gathering evidence, analyzing the results, recommending safe actions, and leaving execution to the human operator.

---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** Oluwatobiloba Taiwo

**Date:** 17/07/2026

---

**1. Reported Symptom**

The web application became unavailable after the Nginx service was stopped. The health check script reported that Nginx was inactive, port 80 was not listening, and HTTP requests to `http://localhost` failed. As a result, the application could not be reached through the web server.

---

**2. Evidence Collected**

The Bash health-check script identified the following issues:

[FAIL] Nginx service is not active
[FAIL] Port 80 is not listening
[FAIL] Local HTTP check returned status 000
[WARN] Root disk usage is 80%
[PASS] Available memory is 254 MB

The recent Nginx service logs also showed:

`Stopping nginx.service`
`nginx.service: Deactivated successfully`
`Stopped nginx.service`

These logs confirmed that the Nginx service had been stopped.

---

**3. Most Likely Cause**

Based on the collected evidence, the most likely cause was that the Nginx service had been intentionally stopped. This was supported by the system logs showing a normal shutdown sequence rather than a crash. Because Nginx was no longer running, port 80 was not listening and HTTP requests returned status 000, making the application unavailable.

---

**4. Human-Approved Recovery Action**

Following Claude's recommendation, I manually reviewed and executed the recovery command:

```bash
sudo systemctl start nginx
```

The recovery command was not executed automatically by Claude, ensuring that a human remained responsible for making changes to the production service.

---

**5. Verification**

After starting the service, I verified that the application had recovered by checking:

```bash
systemctl is-active nginx
```
Output:

`active`

I then confirmed that the web server was responding correctly:

```bash
curl -I http://localhost
```

Output:

`HTTP/1.1 200 OK`

Finally, I reran the Linux health-check script. The report showed:

Nginx service active
Port 80 listening
Local HTTP check returned 200
Overall Status: WARN

The remaining warning was only due to the root disk reaching the configured 80% warning threshold.

---

**6. Safety Decision**

The AI skill was allowed to gather evidence and analyze the incident because these actions are read-only and do not change the system. However, it was intentionally prevented from restarting the Nginx service or making any modifications. This separation reduces operational risk, ensures human approval before production changes, and prevents accidental actions that could make an incident worse.

---

**7. Agentic Loop Mapping**

Gather → Analyze → Human Act → Verify

Gather: The Bash health-check script collected system information, service status, HTTP response, disk usage, memory usage, and recent Nginx logs.
Analyze: Claude read the generated report, identified the failed checks, explained the most likely cause, and recommended a safe recovery command without executing it.
Human Act: I reviewed the recommendation and manually started the Nginx service using `sudo systemctl start nginx`.
Verify: I confirmed the recovery by checking that Nginx was active, verifying an HTTP 200 OK response from `localhost`, and rerunning the health-check script to ensure the service was healthy.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://www.linkedin.com/posts/oluwatobiloba-taiwo_devops-linux-bash-ugcPost-7483881329839869952-yp4G/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACz7IugBIR_XEr4WBbn9LYa6OeAS8fYZbYA

---

#### Screenshot — Published LinkedIn post

![Screenshot — Published LinkedIn post](/week-03-linux-for-devops/screenshots/Screenshot-85-wk3.png)


---

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

https://github.com/Thobilobah/devops-micro-internship-pravinmishra/tree/main/week-03-linux-for-devops

---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [ ] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [ ] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [ ] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [ ] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [ ] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [ ] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [ ] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [ ] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [ ] Incident summary contains all seven required sections
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots and the Bash report
- [ ] Skill does not have Write permission
- [ ] Skill did not execute any recovery commands
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