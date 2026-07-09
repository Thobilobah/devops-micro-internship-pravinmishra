# Assignment 4 — Building Your AI Team

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will build and configure a set of specialized AI subagents inside your project. You will learn how different models and tool permissions define agent behavior, and you will trigger two real agent delegations to analyze security and cost aspects of your Terraform infrastructure.

---

# Task 1 — Create the Agents Folder and Add Files

## Goal

Create the `.claude/agents/` directory and add all required agent files.

### Evidence

#### Screenshot 1 — Agents folder structure in VS Code

![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-17a.png)
![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-17b.png)
![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-17c.png)

---

# Task 2 — Compare the Agent Configurations

## Goal

Analyze the configuration differences between the three agents and demonstrate understanding of model and tool selection.

### Written Answers

#### 1. Why does the cost optimizer use Haiku instead of Sonnet?

The cost optimizer uses Haiku because cost analysis is mainly a read-and-analyze task that doesn't require deep reasoning or code generation. Haiku is faster and less expensive to run, making it ideal for quickly reviewing Terraform files and suggesting cost-saving opportunities.

---

#### 2. Why does the security auditor NOT have Write in its tools list?

The security auditor is designed only to inspect and report security issues, not modify infrastructure code. By omitting the Write tool, the agent cannot accidentally change Terraform files, ensuring its role remains a safe, read-only security review.

---

#### 3. Why does the tf-writer use `inherit` instead of a specific model?

The tf-writer uses inherit so it automatically uses whichever model is currently active in the Claude Code session. This provides flexibility—users can choose a faster model like Haiku or a more capable model like Sonnet or Opus depending on the complexity of the Terraform code they need to generate, without changing the agent configuration.

---

### Evidence

#### Screenshot 2 — security-auditor.md frontmatter

![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-18.png)

---

#### Screenshot 3 — cost-optimizer.md frontmatter

![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-19.png)

---

# Task 3 — Run the Security Auditor

## Goal

Trigger the security auditor agent and analyze the generated security report for your Terraform infrastructure.

### Evidence

#### Screenshot 4 — Security auditor delegation triggered

![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-20.png)

---

#### Screenshot 5 — Security audit report output

![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-22.png)
![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-21.png)

---

# Task 4 — Run the Cost Optimizer

## Goal

Trigger the cost optimizer agent and review the generated cost optimization report.

### Evidence

#### Screenshot 6 — Cost optimization report output

![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-23.png)
![Assignment-04-sc](/week-02-agentic-ai/screenshots/Screenshot-24.png)

---

# Submission Instructions

- Ensure all agent files are committed in `.claude/agents/`
- Complete all written answers in your Google Doc submission
- Push final changes to your forked GitHub repository
- Submit only the Google Doc link as required

---

## Google Doc Link

Paste your Google Doc URL here:

`__________________________`

---

## GitHub Repository URL

Paste your forked repository URL here:

`__________________________`

---

# Completion Checklist

- [ ] `.claude/agents/` folder contains all 3 agent files
- [ ] Screenshot 2 shows correct `security-auditor.md` configuration
- [ ] Screenshot 3 shows correct `cost-optimizer.md` configuration
- [ ] All 3 written answers completed in Google Doc
- [ ] Security auditor executed successfully
- [ ] Cost optimizer executed successfully
- [ ] Security report is visible with findings
- [ ] Cost report is visible with recommendations
- [ ] All required screenshots added
- [ ] GitHub repo updated with agents

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