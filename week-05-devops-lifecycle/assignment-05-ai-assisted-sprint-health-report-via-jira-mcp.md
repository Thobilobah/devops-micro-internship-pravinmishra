# Assignment 5 — AI-Assisted Sprint Health Report via Jira MCP

Part of the DevOps Micro Internship (DMI) Cohort 3 with Agentic AI

---

## Purpose

In this assignment, you will connect Claude Code to your Jira board through an MCP server, the same way you connected it to GitHub in Week 2, and build a read-only `/sprint-health` skill. The skill reads your current sprint through Jira's API and reports sprint velocity, stories at risk of missing the sprint, and items missing an estimate — but it must never create, edit, comment on, or transition a single ticket itself. You will prove that boundary holds by making a real change on the board yourself and confirming the skill only ever reports, never acts.

---

# Task 1 — Create a Jira API Token

## Goal

Generate an API token from your Atlassian account that the MCP server will use to authenticate with your Jira site. Do not screenshot the token value itself.

### Evidence

#### Screenshot 1 — Jira API token creation confirmation page showing the token name, with the token value not visible

![Jira API token creation confirmation page showing the token name, with the token value not visible](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-01.png)

### Notes You Must Write (Very Important):

Why does the MCP server need your site URL and account email in addition to the token?

The MCP server needs the Jira site URL to know which Atlassian instance to connect to and the account email to identify which user the API token belongs to. The API token is then used to authenticate that user securely, allowing the MCP server to access Jira resources and perform actions based on the permissions of that account.

---

# Task 2 — Create .mcp.json at the Project Root

## Goal

Create or update `.mcp.json` at your project root with a Jira MCP server block, following the same shape as the GitHub MCP server you configured in Week 2.

### Evidence

#### Screenshot 2 — `.mcp.json` open in VS Code showing the Jira server configuration

![.mcp.json` open in VS Code showing the Jira server configuration](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-02.png)

### Notes You Must Write (Very Important):

Compare this jira block to the github block from Week 2 Assignment 5. The GitHub server ran via npx (a Node.js package); this one runs via uvx (a Python package) — what stays exactly the same shape despite that difference, and why doesn't Claude Code care which language a given MCP server is written in?

You can write:

The overall structure remains the same. Both the GitHub and Jira configurations are defined under `mcpServers` and include a server name, a `command`, an `args` array, and an optional `env` section. The only difference is the command used to start the server: GitHub uses `npx` because it is a Node.js package, while Jira uses `uvx` because it is a Python package. Claude Code does not care what programming language an MCP server is written in because it communicates with all MCP servers through the same standardized Model Context Protocol. As long as the server follows the MCP specification, Claude Code can interact with it regardless of whether it is implemented in JavaScript, Python, or another language.


---

# Task 3 — Add Your Credentials to settings.local.json

## Goal

Add your Jira site URL, account email, and API token to `.claude/settings.local.json`, and confirm that file is listed in `.gitignore` so it is never committed.

### Evidence

#### Screenshot 3 — `settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered

![settings.local.json` open in VS Code showing the `env` section, with the actual token value blurred or covered](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-03.png)

### Notes You Must Write (Very Important):

Why must JIRA_API_TOKEN live in settings.local.json and never in .mcp.json?

You can write:

**`JIRA_API_TOKEN` should be stored in `settings.local.json` because it contains sensitive authentication credentials that are specific to the local machine and user. Keeping it out of `.mcp.json` helps prevent accidentally committing the token to version control or sharing it with others. The `.mcp.json` file is intended for defining MCP server configurations, while `settings.local.json` is designed to securely store local secrets and environment variables that should remain private.**


---

# Task 4 — Verify the Connection with /mcp

## Goal

Restart Claude Code and confirm the Jira MCP server shows as connected.

### Evidence

#### Screenshot 4 — `/mcp` output showing `jira: connected`

![`/mcp` output showing `jira: connected`](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-04.png)

---

# Task 5 — Run a Live Query to Prove Real Board Data

## Goal

Ask Claude to list the issues in your current active sprint through the Jira MCP connection, and confirm the result matches what you see on your live board in the browser.

### Evidence

#### Screenshot 5 — Claude's response showing the live sprint issue list retrieved via Jira MCP

![Claude's response showing the live sprint issue list retrieved via Jira MCP](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-05.png)

### Notes You Must Write (Very Important):

This task verifies that the Jira MCP server can successfully retrieve live data from the Jira project instead of generating or guessing information.
When the prompt is submitted, Claude sends the request to the Jira MCP server. The MCP server authenticates with Jira using the configured site URL, account email, and API token, then queries the Jira REST API for the active sprint. Jira returns the sprint data, and Claude formats it into a readable report.
The returned information includes the sprint details, all issues in the sprint, their status, assignees, story points, priorities, and an overall sprint summary.
Because the data is retrieved directly from Jira, the response reflects the current state of the project.


---

# Task 6 — Build the /sprint-health Skill

## Goal

Create a `/sprint-health` skill restricted to read-only Jira tools plus `Read`, with no issue-mutating tools and no `Write`. Run it and confirm it produces a report covering sprint velocity, at-risk stories, and items missing an estimate.

### Evidence

#### Screenshot 6 — `SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`

![`SKILL.md` frontmatter showing `allowed-tools` limited to read-only Jira tools plus `Read`, with `disable-model-invocation: true`](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-06.png)

#### Screenshot 7 — `/sprint-health` output showing the full triage report against your real sprint

![`/sprint-health` output showing the full triage report against your real sprint](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-07.png)

### Notes You Must Write (Very Important):

1. Which Jira MCP tools does this skill's allowed-tools list include, and which mutating tools (create issue, update issue, transition issue, add comment) does it deliberately exclude?

The skill includes only read-only Jira MCP tools that retrieve information such as the active sprint, issues, story points, statuses, assignees, and timestamps. It deliberately excludes all mutating tools, including create issue, update issue, transition issue, and add comment, to ensure it can analyze sprint health without making any changes to the Jira project.

2. Why does a Scrum Master need this restriction more than almost any other role in this course?

A Scrum Master is responsible for facilitating the Scrum process and maintaining transparency rather than directly modifying the team's work. Restricting the skill to read-only tools ensures it provides accurate insights and identifies risks without accidentally changing issue statuses, assignments, or Sprint data. This preserves the integrity of the board and keeps decision-making and updates under the control of the human Scrum Master and the development team.

---

# Task 7 — Prove the Skill Never Mutates the Board

## Goal

Manually update one ticket on your board in the browser (for example, move a story to "Done" or add a missing estimate), then run `/sprint-health` again and confirm the new report reflects your change — proving the skill only ever reads live state and never wrote to the board itself.

### Evidence

#### Screenshot 8 — Second `/sprint-health` run showing the report now reflects your manual board change

![Second `/sprint-health` run showing the report now reflects your manual board change](/week-05-devops-lifecycle/screenshots/Ass-05-Screenshot-08.png)

### Notes You Must Write (Very Important):

Map this assignment to Gather → Analyze → Human Act → Verify from Week 3 Assignment 6. Which step did you perform manually in the browser, and why must that step stay human?

This assignment follows the Gather → Analyze → Human Act → Verify workflow from Week 3 Assignment 6.

Gather: The /sprint-health skill retrieved the latest sprint data from Jira using the read-only MCP tools.

Analyze: The skill analyzed the sprint by calculating velocity, identifying at-risk stories, and highlighting issues with missing estimates or acceptance criteria.

Human Act: I manually updated the Jira board in the browser by changing a ticket (such as moving it to Done or adding a missing estimate). This step must remain human because modifying the board affects the project's source of truth and should require deliberate human judgment rather than an automated action.

Verify: I ran /sprint-health again and confirmed that the report reflected the manual change. This verified that the skill only reads the current state of the board and never creates, edits, comments on, or transitions Jira issues itself.

---

# Submission Instructions

Complete all tasks in sequence.

Your submission must include:
- All 8 required screenshots
- All the required notes

---

# Completion Checklist

- [ ] Task 1: Jira API token created, value never screenshotted (Screenshot 1)
- [ ] Task 2: `.mcp.json` has the Jira server block (Screenshot 2)
- [ ] Task 3: Credentials stored in `settings.local.json`, token blurred, file gitignored (Screenshot 3)
- [ ] Task 4: `/mcp` shows the Jira server connected (Screenshot 4)
- [ ] Task 5: Live query returned real sprint data, verified against the browser (Screenshot 5)
- [ ] Task 6: `/sprint-health` skill created with correct read-only `allowed-tools`, and produced a full report (Screenshots 6–7)
- [ ] Task 7: A manual board change was reflected in a second `/sprint-health` run (Screenshot 8)
- [ ] Skill never created, edited, transitioned, or commented on any issue
- [ ] Reflection answered (Notes)
- [ ] No API token value exposed

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

*This submission is part of DevOps Micro Internship (DMI) Cohort 3 — Agentic AI Track.*
