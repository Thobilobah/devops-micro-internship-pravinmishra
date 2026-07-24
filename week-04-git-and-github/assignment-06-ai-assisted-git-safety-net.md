# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In Week 2 you built Claude Code hooks that block a dangerous action *before* it happens (`PreToolUse`), and a restricted skill that could look but not touch (`allowed-tools` without `Write`). In this assignment you will discover that Git has the exact same idea, decades older: a **pre-commit hook** that blocks a commit before it's created.

You will build both halves of a real "PR Ready" workflow:

1. A **Git hook that follows fixed rules** — scans staged changes for hardcoded secrets and oversized files and refuses the commit. No AI involved, no guessing, just a rule that gives the same answer every time.
2. A **restricted Claude Code skill** (`/pr-ready`) that reads your staged diff and drafts a Pull Request title, description, and a short list of things worth a second look — the kind of judgment a fixed rule can't make (mixed changes, missing context, unclear intent). The skill never commits, pushes, or opens the PR. You do that yourself, using its draft as a starting point.

This mirrors the Agentic Loop from Week 3's Linux triage assignment: **Gather → Analyze → Human Act → Verify**. The hook and the skill both gather and analyze; only you act.

---

# Task 0 — Confirm Your Fork and Create a Feature Branch

## Goal

Confirm you are working in your own fork, then create a dedicated branch for this assignment.

### Evidence

#### Screenshot 1 — Output of git remote -v and git branch showing the new branch

![Output of git remote -v and git branch showing the new branch](/week-04-git-and-github/screenshots/Ass-06-Screenshot-01a.png)

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

Based on the assignment and the Git workflow you've been practicing (feature branches, commits, rebasing, pull requests, and open-source collaboration), here's a strong answer:

**1. Why create a dedicated branch instead of doing this work on `main`?**

Creating a dedicated branch keeps the `main` branch stable and free from unfinished or experimental changes. It allows me to work on a specific feature or assignment in isolation, make multiple commits without affecting the production-ready code, and safely test or refine my work before merging it back. Using feature branches also makes collaboration easier, as changes can be reviewed through Pull Requests, conflicts can be resolved before merging, and the commit history remains clean and organized. This is a standard Git workflow used in both open-source and professional software development because it improves code quality, enables code reviews, and reduces the risk of introducing unintended changes into the main branch.


---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

![Output of  `git status` showing the staged file on feature/ai-pr-ready](/week-04-git-and-github/screenshots/Ass-06-Screenshot-01b.png)

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

A fake AWS access key is used to ensure no real credentials are exposed or compromised during the exercise. It allows us to safely practice detecting hardcoded secrets with Git hooks without creating a security risk. This teaches the importance of never committing sensitive information to a repository while providing a realistic example that can be safely scanned, blocked, and removed before a commit is made.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

![hooks/pre-commit` open in VS Code showing the full script](/week-04-git-and-github/screenshots/Ass-06-Screenshot-02.png)


---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

![Output of `git config core.hooksPath` confirming it points to `hooks](/week-04-git-and-github/screenshots/Ass-06-Screenshot-2b.png)

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

Tracking `hooks/pre-commit` in the repository allows the hook to be version-controlled and shared with every contributor. Since the default `.git/hooks/` directory is local to each developer and isn't tracked by Git, storing the hook in a tracked hooks/ folder ensures everyone uses the same pre-commit checks. Configuring Git to use this folder (core.hooksPath) makes the hook reusable, consistent, and easy to maintain across the team.

---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

`PreToolUse` and Git's `pre-commit` hook both intercept actions before they happen, but they operate at different stages of the workflow. `PreToolUse` intercepts tool commands before they are executed by Claude Code, allowing it to block potentially destructive operations such as `terraform destroy` or deleting AWS resources. In contrast, the Git `pre-commit` hook intercepts the `git commit` process, scanning the staged changes for issues like hardcoded secrets or oversized files before a commit is created. What they have in common is that they both act as preventative safety mechanisms, enforcing rules early to stop risky actions before they can cause problems, rather than trying to fix the issues afterward.

---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

![Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file](/week-04-git-and-github/screenshots/Ass-06-Screenshot-03.png)

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The line that matched the fake key was:
```bash
if git diff --cached -- "$file" | grep -qE ; then
```
It matched because the fake AWS access key used in the assignment began with the AKIA prefix and followed the format expected by the regular expression. The pre-commit hook scanned the staged changes, detected a string that matched this pattern, and blocked the commit before it could be created.

---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No. This hook would only detect secrets that match the specific patterns defined in its regular expression, such as AWS access keys beginning with AKIA or private key headers. If a secret was stored in a variable with a different format or without those patterns, the hook would not detect it. This highlights the limitation of fixed-rule validation: it is deterministic and reliable for known patterns, but it cannot identify every type of secret or understand context. More advanced tools or AI-assisted reviews can complement these rules by identifying suspicious content that doesn't match predefined patterns.
---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

![`SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`](/week-04-git-and-github/screenshots/Ass-06-Screenshot-04.png)

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

![/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement](/week-04-git-and-github/screenshots/Ass-06-Screenshot-05.png)

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

`/pr-ready` has `Bash` and `Read` because it needs to inspect the repository by running commands like `git diff --cached` and `git status`, and read the staged changes to generate a Pull Request draft. It does not have `Write` because its purpose is only to analyze and provide recommendations. Excluding `Write` prevents it from modifying files, creating commits, pushing changes, or opening a Pull Request, ensuring that all final actions remain under the developer's control.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

No, they did not flag the same things. The pre-commit hook focused on fixed, predefined rules by scanning the staged diff for known secret patterns (such as AWS access keys) and oversized files, blocking the commit if any were found. In contrast, /pr-ready performed a broader review by looking for debug statements, TODO/FIXME comments, mixed or unrelated changes, missing documentation, and drafting a Pull Request title and description. The hook was effective at catching known patterns automatically, while /pr-ready provided contextual feedback and highlighted issues that required judgment rather than simple pattern matching. Together, they complement each other by combining deterministic validation with AI-assisted code review.

---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

![git commit` succeeding after the fix (no BLOCKED message)](/week-04-git-and-github/screenshots/Ass-06-Screenshot-06.png)

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

![second `/pr-ready` run showing a clean risk report and a drafted PR title + description](/week-04-git-and-github/screenshots/Ass-06-Screenshot-07.png)

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

To satisfy the pre-commit hook, I removed the hardcoded fake AWS access key and deleted the debug echo statement that printed the credential. After making these changes, I staged the updated file again, and the pre-commit hook no longer detected any secret-like patterns or debug output, allowing the commit to proceed successfully.

---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

![Pull request](/week-04-git-and-github/screenshots/Ass-06-Screenshot-08.png)

---

#### PR Link

https://github.com/Thobilobah/devops-micro-internship-pravinmishra/pull/1

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I reviewed the AI-generated draft and edited it to make it more accurate and relevant to the changes I actually made. I updated the summary, removed any unnecessary or generic information, and ensured the description clearly reflected the work completed in my branch. This helped make the PR more professional and easier for reviewers to understand.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

Blindly copying the AI's draft could result in incorrect or misleading information in the Pull Request. The description might include changes that were never made, omit important details, or contain inaccurate explanations. Reviewing the draft ensures the PR accurately represents the work and prevents confusion for reviewers.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

This PR should target my own fork because the assignment files, hooks, skills, and configurations are part of my personal practice work and are not intended to be merged into the shared upstream repository. Opening the PR against my fork allows me to demonstrate my work safely without affecting the official class repository or other students' work.

---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

The Gather step involves collecting information about the project before creating the Pull Request. Claude gathers details such as the changed files, commit history, Git status, and other project context needed to understand what was modified.

---

**2. Which step(s) represent Analyze?**

The Analyze step is where Claude processes the gathered information and generates a draft Pull Request title and description. It summarizes the changes, organizes them into a clear format, and identifies the important updates that should be included in the PR.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

The Human Act step is when I review Claude's suggestions, confirm they are accurate, and then run git commit, git push, and create the Pull Request myself. These actions modify the Git repository and publish changes to GitHub, so they require human approval and responsibility to prevent mistakes or unintended changes.

---

**4. Which step is Verify?**

The Verify step involves checking that the Pull Request was created correctly. This includes confirming the correct base repository and branch, ensuring the title and description accurately represent the changes, and verifying that the required screenshots and evidence have been included.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

The fixed-rule pre-commit hook automatically enforces predefined rules, such as blocking secrets or unwanted files before a commit. The AI skill complements this by understanding the context of the changes, generating meaningful PR descriptions, and providing intelligent suggestions that fixed rules cannot. Together, they make the workflow both safer and more effective.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

https://www.linkedin.com/posts/oluwatobiloba-taiwo_devops-git-github-ugcPost-7486209588124553216-RzsQ/?utm_source=share&utm_medium=member_desktop&rcm=ACoAACz7IugBIR_XEr4WBbn9LYa6OeAS8fYZbYA

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

-Learned how to initialize and configure Git repositories using both local and global Git settings.
-Understood how AI can assist in creating Pull Request titles and descriptions while still requiring human review and approval.
-Learned the importance of reviewing AI-generated content instead of copying it directly to ensure accuracy and relevance.
-Gained a better understanding of the Gather → Analyze → Human Act → Verify workflow and how it applies to AI-assisted software development.
-Learned why combining fixed-rule safety checks (hooks) with AI-assisted analysis creates a safer and more effective development workflow.

---

# Submission Instructions

- Ensure `hooks/pre-commit` and `.claude/skills/pr-ready/SKILL.md` are committed to your GitHub repository
- Add all required screenshots to your submission
- All written answers must be in your own words
- Do not use a real secret or credential anywhere in your submission — the fake key in Task 1 is intentional and must stay clearly fake
- Open your Pull Request against your own fork, not the shared upstream repository
- Push your final changes to your forked repository
- Include your PR link and LinkedIn post URL

---

## GitHub Repository URL

Paste your forked repository URL here:

https://github.com/Thobilobah/devops-micro-internship-interviews/tree/feature-readme-update

---

# Completion Checklist

- [ ] Branch `feature/ai-pr-ready` created with a staged file containing a fake secret and a debug statement
- [ ] `hooks/pre-commit` created and tracked in the repo (not only in `.git/hooks/`)
- [ ] `core.hooksPath` configured to point at `hooks/`
- [ ] Pre-commit hook shown blocking the risky commit
- [ ] `.claude/skills/pr-ready/SKILL.md` created with correct `allowed-tools` (no `Write`) and `disable-model-invocation: true`
- [ ] `/pr-ready` run against the risky diff and shown flagging issues
- [ ] Risky file fixed; `git commit` succeeds cleanly
- [ ] `/pr-ready` re-run showing a clean report and drafted PR title/description
- [ ] Pull Request opened using the AI draft as a starting point, with your own fork as the base repository (not upstream), PR link included
- [ ] Agentic Loop mapping (Task 7) completed in your own words
- [ ] LinkedIn post published and URL submitted
- [ ] All required screenshots added
- [ ] GitHub repository URL provided

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
