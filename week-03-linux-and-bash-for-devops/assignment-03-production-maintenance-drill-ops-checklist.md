# Assignment 3 — Production Maintenance Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a live production system. You will perform structured operational checks covering network validation, service health, log analysis, resource monitoring, configuration verification, and incident simulation with recovery — mirroring real on-call DevOps responsibilities.

---

# Task 1 — Server Access & Networking Validation

## Goal

Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.

### Evidence

#### Screenshot 1 — Browser showing the React app with your Full Name visible on the UI


![Image of Browser showing the deployed React app at `http://<public-ip>` with your name and date visible](/week-03-linux-for-devops/screenshots/Screenshot-11-wk3.png)

---

#### Screenshot 2 — Output of `ip a`

![Image Output of `ip a`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-13-wk3.png)

---

#### Screenshot 3 — Output of `sudo ss -tulpen`

![Image Output of `sudo ss -tulpen`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-14-wk3.png)

---

#### Screenshot 4 — Output of `sudo ufw status`

![Image Output of sudo ufw status](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-15-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What proves Nginx is listening on 0.0.0.0:80?**


The proof is this line from sudo ss -tulpen:
```bash
tcp LISTEN 0 511 0.0.0.0:80 0.0.0.0:* users:(("nginx",...))
```

This shows that the nginx process is in the LISTEN state on 0.0.0.0:80. The address 0.0.0.0 means Nginx is accepting connections on port 80 from all network interfaces, not just localhost.
**2. What proves SSH is active on port 22?**

The proof is this line from sudo ss -tulpen:

```bash
tcp LISTEN 0 4096 0.0.0.0:22 0.0.0.0:* users:(("sshd",...))
```

This confirms that the sshd (OpenSSH server) process is actively listening on port 22 for incoming SSH connections

---

**3. Did you find any unexpected open ports? Explain briefly.**

No, there were no unexpected open ports.

The externally accessible TCP ports shown are:

80 — used by Nginx for HTTP web traffic.

22 — used by SSH for remote administration.

The other ports (such as 53, 68, and 323) are either bound to 127.0.0.1 (localhost only) or are normal system services for DNS resolution, DHCP, and time synchronization. Since they are not exposed publicly on all interfaces, they are not considered unexpected open internet-facing ports.

---

# Task 2 — Service Health & Systemd Validation (Nginx)

## Goal

Verify that Nginx is properly installed, running, enabled at boot, and safely configured.

### Evidence

#### Screenshot 1 — Output of `systemctl status nginx --no-pager`

![Image Output of systemctl status nginx --no-pager](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-16-wk3.png)

---

#### Screenshot 2 — Output of `sudo nginx -t`

![Image Output of sudo nginx -t](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-17-wk3.png)

---

#### Screenshot 3 — Output of `sudo ss -lptn '( sport = :80 )'`

![Image Output of sudo ss -lptn '( sport = :80 )](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-18-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What happens if Nginx fails to restart in production?**

If Nginx fails to restart, the web server will not be able to serve websites or applications, which can make them unavailable to users. This may lead to downtime, failed requests, and a poor user experience until the issue is fixed and the service is running again.

---

**2. What's your basic rollback plan?**

If a deployment causes problems, I would restore the previous working version of the application or Nginx configuration from a backup. I would then restart Nginx, verify that the website is working correctly, and investigate the cause of the failure before attempting another deployment.

---

# Task 3 — Logs & Request Trace

## Goal

Verify real traffic flow and analyze logs to understand system behavior and errors.

### Evidence

#### Screenshot 1 — Output of `sudo tail -n 30 /var/log/nginx/access.log`

![Image Output of sudo tail -n 30 /var/log/nginx/access.log](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-19-wk3.png)

---

#### Screenshot 2 — Output of `sudo tail -n 30 /var/log/nginx/error.log`

![Image Output of sudo tail -n 30 /var/log/nginx/error.log](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-20-wk3.png)


---

#### Screenshot 3 — Output of `sudo journalctl -u nginx --no-pager -n 50`


![Image Output of sudo journalctl -u nginx --no-pager -n 50](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-21-wk3.png)


---

### Notes

Answer the following in your own words:

**1. Were there any errors in the logs?**

- If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.
- If no, explain what it means if the error log is empty or shows no recent errors during your check.

There were no errors recorded in the Nginx error log during my check. The only entry was:

```bash
2026/07/15 11:43:14 [notice] 24807#24807: using inherited sockets from "5;6;"
```
This is a notice, not an error. It indicates that Nginx successfully reused existing sockets during startup or a reload, allowing it to continue serving requests without interruption.

The access log did show some requests that resulted in HTTP error responses, for example:
```bash
160.119.76.106 ... 400 166
```
This means a client sent an invalid request (likely attempting an HTTPS connection to the HTTP port), so Nginx returned a 400 Bad Request.

Another example is:
```bash
45.39.17.14 ... "POST / HTTP/1.1" 405 568
```
This means the client attempted to send a POST request to the root URL, but that method is not allowed, so Nginx returned 405 Method Not Allowed.

---

**2. If there were no errors, what does that indicate about the system?**

Since the Nginx error log contained no recent errors, it indicates that Nginx is running normally and there are no server-side problems affecting its operation. The web server started successfully and continued handling incoming requests without reporting configuration or runtime issues.

---

**3. Based on the access logs, were your curl requests visible in the log entries? What does that prove about traffic flow?**

No. The access logs you provided do not show any requests from curl. Instead, they contain requests from external IP addresses with browser user agents and automated scans looking for files such as .env and .git/config.

This proves that Nginx is actively receiving and logging incoming traffic from the network. If my curl requests had been made during this logging period, they would have appeared in the access log as additional entries, confirming that requests were successfully reaching the web server.

---

# Task 4 — System Resource Health Check (Capacity Red Flags)

## Goal

Assess server capacity and detect potential performance or failure risks.

### Evidence

#### Screenshot 1 — Output of `uptime`

![Image Output of uptime](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-22-wk3.png)

---

#### Screenshot 2 — Output of `free -h`

![Image Output of uptime](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-23-wk3.png)

---

#### Screenshot 3 — Output of `df -h`

![ Output of `df -h`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-24-wk3.png)

---

#### Screenshot 4 — Output of `sudo du -sh /var/* | sort -h`

![Output of `sudo du -sh /var/* | sort -h`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-25-wk3.png)

---

### Notes

Answer the following in your own words:

**1. Which resource looks most critical right now? (CPU/load, memory, or disk) Explain why.**

From the system checks, disk usage looks like the most important resource to keep an eye on. The root partition is using 60% of its available space, which isn't critical yet but is higher than the CPU load, which is 0.00, and the memory usage, which still has about 549 MiB available. The server is performing well overall, but disk usage will need monitoring as applications and logs continue to grow.

---

**2. What happens if disk becomes 100% full in a production server?**

If the disk reaches 100% capacity, the server can start experiencing serious problems. Applications may fail to write data or logs, updates and deployments can fail, and some services may stop working or crash completely. To prevent downtime, it's important to monitor disk usage regularly and clean up or expand storage before it becomes full.

---

# Task 5 — Configuration & Deployment Verification

## Goal

Ensure the correct React build is deployed and Nginx is serving it properly.

### Evidence

#### Screenshot 1 — Output of `ls -lah /var/www/html | head -n 20`


![Output of `ls -lah /var/www/html | head -n 20`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-26-wk3.png)

---

#### Screenshot 2 — Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`

![Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-27-wk3.png)

---

#### Screenshot 3 — Output of `grep -n "try_files" /etc/nginx/sites-available/default`


![Output of `grep -n "try_files" /etc/nginx/sites-available/default`](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-28-wk3.png)

---

### Notes

Answer the following in your own words:

**1. How do you confirm that the correct version of the application is deployed?**

I checked the files in /var/www/html using ls -lah

I confirmed the React build files are present (like index.html and static folder)

I verified the custom change is deployed by checking the "Deployed by <Oluwatobiloba Taiwo>" line in the code

You ensured Nginx is serving the correct application through the correct web root

You validated that the application loads correctly in the browser
---

# Task 6 — Nginx Configuration Failure Simulation

## Goal

Simulate a real-world Nginx misconfiguration and recover the service safely.

### Evidence

#### Screenshot 1 — Output of `sudo nginx -t` showing the syntax error (broken config)

![Output of `sudo nginx -t` showing the syntax error (broken config)](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-29-wk3.png)

---

#### Screenshot 2 — Output of `sudo nginx -t` showing syntax ok (fixed config)

![Output of `sudo nginx -t` showing syntax ok (fixed config)](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-30-wk3.png)

---

#### Screenshot 3 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

![Output of `curl -I http://<public-ip>` confirming recovery (200 OK))](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-31-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What caused the configuration failure?**

The configuration failed because I intentionally removed the semicolon (;) from the try_files directive in the Nginx configuration file. Since Nginx configuration files require each directive to end with a semicolon, sudo nginx -t detected the syntax error and prevented the configuration from being applied.

---

**2. How did you fix the issue?**

I fixed the issue by editing the Nginx configuration file and adding the missing semicolon back to the try_files directive. After making the correction, I ran sudo nginx -t again to confirm that the syntax was valid, restarted the Nginx service, and verified that the website was accessible by sending a request with curl, which returned a 200 OK response.

---

**3. How can you avoid this kind of issue in real production systems?**

To avoid this type of issue in production, I would always test configuration changes with sudo nginx -t before restarting or reloading Nginx. It's also good practice to review configuration changes, keep backups of working configurations, and use version control so changes can be tracked and rolled back quickly if needed.

---

# Task 7 — Web Application Failure Simulation

## Goal

Simulate missing deployment content and recover the application safely.

### Evidence

#### Screenshot 1 — Output of `curl -I http://<public-ip>` showing failure (non-200 response)

![Output of curl -I http://<public-ip> showing failure (non-200 response)](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-32-wk3.png)

---

#### Screenshot 2 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

![Output of `curl -I http://<public-ip>` confirming recovery (200 OK)](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-33-wk3.png)

---

### Notes

Answer the following in your own words:

**1. What caused the application to break in this scenario?**

The application stopped working because I temporarily moved the entire /var/www/html directory, which contains the website files, to a backup location. After creating an empty /var/www/html directory, Nginx no longer had the application files to serve, so requests to the website returned a failure instead of a successful response.

---

**2. How did you fix the issue and restore the application?**

To restore the application, I removed the empty /var/www/html directory and moved the original html_backup directory back to /var/www/html. I then restarted the Nginx service to ensure it was serving the restored files. Finally, I verified that the application was working again by sending a request with curl, which returned a 200 OK response.

---

**3. What steps would you take to prevent this kind of issue in real production systems?**

In a real production environment, I would avoid modifying or deleting the live application directory directly. Instead, I would keep backups of previous deployments, use automated deployment tools or CI/CD pipelines, and validate deployments in a staging environment before releasing them. I would also verify the deployment after each release and have a rollback plan ready so the previous working version can be restored quickly if anything goes wrong.

---

# Task 8 — Security & Reliability Review

## Goal

Review and reflect on the security and reliability practices applied during this assignment.

### Security & Reliability Notes

Answer the following in your own words:

**1. Why is SSH key-based authentication more secure than sharing passwords?**

SSH key-based authentication is more secure because it uses a pair of cryptographic keys instead of a password that can be guessed or stolen. The private key stays only with the user, making it much harder for attackers to gain unauthorized access through brute-force or password-based attacks.
---

**2. Why should only required ports be open on a production server?**

Only the ports needed for the application should be open because every open port increases the server's attack surface. Closing unnecessary ports reduces the risk of unauthorized access and helps keep the server more secure.

---

**3. Why is it important for Nginx to be enabled on boot?**

Enabling Nginx to start automatically ensures that the web server comes back online after a reboot or system restart without requiring manual intervention. This helps reduce downtime and keeps the application available to users.

---

**4. What are the risks of sharing secrets, keys, or credentials publicly?**

Sharing secrets, API keys, SSH keys, or passwords publicly can allow unauthorized users to access systems, steal sensitive data, or misuse cloud resources. It can also lead to security breaches, financial costs, and service disruptions if the credentials are compromised.

---

**5. Why should cloud resources be stopped or terminated when they are no longer needed?**

Stopping or terminating cloud resources when they are no longer needed helps avoid unnecessary charges and reduces wasted resources. It also improves security by ensuring unused servers or services cannot be exploited by attackers.

---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

https://www.linkedin.com/posts/oluwatobiloba-taiwo_production-maintenance-drill-learning-to-ugcPost-7483509750811402240-7oLp/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACz7IugBIR_XEr4WBbn9LYa6OeAS8fYZbYA

#### Screenshot — Published LinkedIn post

![Published LinkedIn post](/week-03-linux-and-bash-for-devops/screenshots/Screenshot-34-wk3.png)

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

- [ ] Task 1: Screenshots (browser, ip a, ss -tulpen, ufw status) + Notes answered
- [ ] Task 2: Screenshots (nginx status, nginx -t, ss port 80) + Notes answered
- [ ] Task 3: Screenshots (access log, error log, journalctl) + Notes answered
- [ ] Task 4: Screenshots (uptime, free -h, df -h, du -sh) + Notes answered
- [ ] Task 5: Screenshots (ls html, grep deployed by, grep try_files) + Notes answered
- [ ] Task 6: Screenshots (nginx -t fail, nginx -t pass, curl recovery) + Notes answered
- [ ] Task 7: Screenshots (curl failure, curl recovery) + Notes answered
- [ ] Task 8: Security & Reliability Notes answered
- [ ] LinkedIn post published and URL submitted
- [ ] Full Name visible in all required screenshots
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