# Assignment 6 — Build an AI-Assisted Linux Health Check (AI-Assisted Linux Incident Triage)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will build a read-only Bash triage script that checks the health of your Ubuntu server and Nginx application, connect it to Claude Code as a reusable `/linux-triage` skill, simulate a controlled Nginx incident, use the skill to gather and analyze evidence, recover the service manually, and verify recovery. The workflow follows the Agentic Loop: Gather → Analyze → Human Act → Verify.

---

# Task 1 — Confirm the Healthy Baseline and Create the Workspace

## Goal

Confirm that Nginx and the React application are healthy before building the automation.

### Evidence

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhost`

<img width="1345" height="565" alt="image" src="https://github.com/user-attachments/assets/3477b340-e4c2-4b57-b5c5-c5b47f124a25" />


---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What proves that Nginx is running?**

The following things prove that Nginx is running successfully:

Running systemctl status nginx shows active (running).
Opening the server’s IP address in a web browser displays the Nginx welcome page.
The command curl http://localhost returns an HTML response from Nginx.

In short: If systemctl status nginx shows Active: active (running), it proves that Nginx is currently running.

---

**2. What proves that the server is listening for HTTP traffic?**

A server is listening for HTTP traffic when port 80 is open and in the LISTEN state. Port 80 is the standard port used for HTTP communication. When Nginx is configured and running correctly, it listens on this port and waits for incoming HTTP requests from clients. The LISTEN status proves that the server has successfully opened the port and is ready to receive network connections. Therefore, checking that Nginx is listening on port 80 is evidence that the server is ready to handle HTTP traffic.

---

**3. Why must you capture a healthy baseline before simulating an incident?**

A healthy baseline is important because it shows the normal condition of the system before an incident occurs. It provides reference values for CPU usage, memory usage, network activity, running services, and other system metrics. During an incident simulation, these values can be compared with the baseline to identify what has changed and determine the impact of the problem. A baseline also helps in detecting unusual behavior, troubleshooting the root cause, and verifying whether the system has returned to normal after the incident is resolved. Therefore, capturing a healthy baseline makes incident detection, analysis, and recovery more accurate and reliable.

---

# Task 2 — Create Project Context and Safety Rules in CLAUDE.md

## Goal

Tell Claude exactly what this project does and what it is not allowed to do.

### Evidence

#### Screenshot 3 — CLAUDE.md open in VS Code showing all four sections (Project Overview, Incident Workflow, Safety Rules, Output Rules)

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Why should Claude receive project-specific operational rules?**

Claude should receive project-specific operational rules so that it understands how the project should be managed and what actions are allowed or restricted. These rules provide clear instructions about the project's environment, tools, security requirements, coding standards, and operational procedures. They help Claude make accurate decisions, avoid unsafe or unnecessary actions, and follow the team's established workflow. Project-specific rules also improve consistency and reduce mistakes when Claude performs tasks such as troubleshooting, configuration, deployment, or automation. Therefore, giving Claude clear operational rules makes its work safer, more reliable, and better aligned with the project's requirements.

---

**2. Why is the human required to execute the recovery command?**

The human is required to execute the recovery command because recovery actions can change or restart important system services and may affect the availability or stability of the server. Human approval provides an additional safety check before making such changes. It prevents Claude or an automated system from performing potentially risky actions without proper verification. The human can review the situation, confirm that the recovery command is appropriate, and execute it when ready. This ensures better control, security, accountability, and safer incident recovery.

---

**3. Which rule prevents Claude from making an unsupported diagnosis?**

The rule that prevents Claude from making an unsupported diagnosis is the evidence-based diagnosis rule. It requires Claude to make conclusions only when they are supported by verified evidence, logs, system status, or observed data. If there is not enough evidence, Claude must clearly state that the cause is unknown instead of guessing or making assumptions. This rule helps prevent incorrect diagnoses and ensures that troubleshooting is accurate, safe, and reliable.

---

# Task 3 — Use Agentic AI to Plan Before Writing the Script

## Goal

Use Claude Code to inspect the environment and produce a read-only plan before creating any Bash code.

### Evidence

#### Screenshot 4 — Claude Code showing the five-check plan and read-only inspection results

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which part of this task represents the Gather phase?**

The Gather phase is the part where Claude collects information about the server before taking any action. This includes checking the server status, Nginx status, listening ports, logs, CPU and memory usage, and comparing the current state with the healthy baseline. Gathering this evidence helps Claude understand the actual problem before making a diagnosis or suggesting a solution.

---

**2. Did Claude follow the instruction not to create files? How did you verify this?**

Yes, Claude followed the instruction not to create any files. I verified this by checking the project directory before and after Claude performed the task and confirming that no new files were created or modified. This shows that Claude respected the given operational rule and only performed the allowed actions without making unnecessary changes to the project.

---

**3. Why is planning before coding useful in DevOps automation?**

Planning before coding is useful in DevOps automation because it helps clearly define the goal, required steps, tools, and expected results before making changes. It reduces mistakes and prevents unnecessary or risky actions. A proper plan also helps identify dependencies, security concerns, and possible failures in advance. In automation, where one command can affect multiple systems or resources, planning ensures that the process is safe, repeatable, efficient, and reliable. It also makes troubleshooting easier because each step and its expected outcome are clearly understood.

---

# Task 4 — Build the Linux Triage Bash Script

## Goal

Create one Bash script that gathers consistent Linux and Nginx health evidence.

### Evidence

#### Screenshot 5 — Top section of `linux-triage.sh` showing variables, thresholds, and the checks array

Add your screenshot here.

---

#### Screenshot 6 — Middle section showing check functions and conditionals

Add your screenshot here.

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

Add your screenshot here.

---

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is stored in the checks array?**

The checks array stores a list of checks or validation steps that need to be performed during the DevOps troubleshooting process. These checks help verify the current condition of the system, such as service status, port availability, system resources, and other required conditions. It allows the automation process to organize multiple checks in one place and use their results to determine whether the system is healthy or requires further action.

---

**2. How does the `for` loop use that array?**

The for loop goes through each item in the checks array one by one. For every item, it performs the required check and collects or displays the result. This allows multiple system checks to be performed automatically without writing separate code for each check. It makes the DevOps automation process simple, efficient, and repeatable.

---

**3. Why are the health checks separated into functions?**

Health checks are separated into functions to make the code organized, reusable, and easier to maintain. Each function can handle one specific check, such as checking Nginx status, port availability, or system resources. This makes the code easier to understand and troubleshoot. If a particular check needs to be changed, only its function needs to be modified. It also allows the same health-check functions to be reused in different automation tasks, making the DevOps script more reliable and efficient.

---

**4. What is the purpose of `$(...)` in this script?**

In a Bash script, $(...) is called command substitution. It is used to run a command and store its output so that the result can be used as a value in another command or variable.

---

**5. Why does the script use different exit codes for HEALTHY, WARN, and FAIL?**

The script uses different exit codes to clearly indicate the health condition of the system. Each exit code represents a different result, such as HEALTHY when everything is working normally, WARN when there is a possible issue that needs attention, and FAIL when a serious problem is detected. These codes allow other scripts, monitoring tools, or automation systems to quickly understand the result and take appropriate action. This makes DevOps automation more reliable, consistent, and easier to monitor.

---

# Task 5 — Run and Understand the Healthy-State Report

## Goal

Run the Bash script against the healthy server and verify that it creates a report.

### Evidence

#### Screenshot 9 — Output of `./scripts/linux-triage.sh` showing your Full Name and all five check results

Add your screenshot here.

---

#### Screenshot 10 — Output showing the captured exit code and final summary

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What is the overall status of your healthy baseline?**

The overall status of the healthy baseline is HEALTHY. All important system checks, such as Nginx service status, HTTP port availability, and basic system health, are functioning normally. No critical errors or abnormal conditions were detected. This healthy baseline can be used as a reference point for comparing the system during and after an incident simulation.

---

**2. Which exact Linux evidence proves the application is serving traffic?**

The exact Linux evidence is the successful curl response from the application, such as running curl http://localhost and receiving the expected webpage/HTTP response. This proves that the application is reachable and actively serving HTTP traffic.

---

**3. Did your script return exit code 0 or 1? Explain why.**

The script returned exit code 0 because all the health checks passed successfully and the system was in a HEALTHY state. Exit code 0 indicates that the script completed successfully without detecting any critical problems.

---

**4. What is the difference between a warning and a failure in this script?**

A warning means that the system has detected a minor or potentially abnormal condition, but the application is still functioning. It may require attention, but it is not an immediate critical problem.

A failure means that a critical health check has failed and the application or service may not be working correctly. It requires immediate investigation or recovery action.

Therefore, WARN indicates a non-critical issue, while FAIL indicates a critical problem that can affect the application's availability or functionality.

---

# Task 6 — Create and Run the /linux-triage Skill

## Goal

Turn the Bash script into a reusable, manually invoked Agentic AI workflow.

### Evidence

#### Screenshot 11 — `SKILL.md` showing the frontmatter, allowed tool restrictions, and safety rules

Add your screenshot here.

---

#### Screenshot 12 — `/linux-triage` output for the healthy server

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Why does this skill have Bash, Read, and Grep, but not Write?**

This skill has Bash, Read, and Grep because its purpose is to inspect and diagnose the system without modifying files. Read and Grep allow Claude to examine files and search for relevant information, while Bash allows it to run approved diagnostic commands.

The Write tool is intentionally not included because Claude should not create or modify files during this task. This provides an additional safety control, preventing accidental changes to the project while troubleshooting.

---

**2. Why is `disable-model-invocation: true` useful for this skill?**

disable-model-invocation: true prevents Claude from automatically invoking the skill on its own. The skill can only be used when the human explicitly triggers it. This is useful for safety because the skill may perform important or sensitive operations, and human control ensures that it runs only when intentionally requested. It helps prevent unexpected actions and gives the user better control over the DevOps workflow.

---

**3. What part is performed by Bash, and what part is performed by Claude?**
Bash performs the actual Linux commands and system-level operations, such as checking services, ports, logs, and system health. Claude interprets the results returned by Bash, analyzes the collected information, identifies possible issues based on the available evidence, and explains the findings. Therefore, Bash handles the execution and data collection, while Claude handles the analysis, reasoning, and reporting.

---

**4. Why is this better than asking Claude "Is my server healthy?" without giving it evidence?**

This approach is better because Claude receives actual evidence from the server, such as service status, port information, logs, and health-check results. With this evidence, Claude can make an accurate, evidence-based assessment instead of guessing or making unsupported assumptions. It also makes troubleshooting more reliable, repeatable, and transparent because the conclusion can be traced back to specific system information.

---

# Task 7 — Simulate an Nginx Incident and Let the Skill Diagnose It

## Goal

Create a controlled service failure, gather evidence through Bash, and let Claude analyze the evidence without taking recovery action.

### Evidence

#### Screenshot 13 — Output showing Nginx is inactive and the HTTP request fails

Add your screenshot here.

---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

Add your screenshot here.

---

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

Add your answer here.

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

Add your answer here.

---

**3. Did Claude execute the recovery command? Why is that important?**

Add your answer here.

---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

Add your answer here.

---

**5. Which phase is represented by Claude's explanation?**

Add your answer here.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

Add your screenshot here.

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

Add your screenshot here.

---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

Add your screenshot here.

---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

Add your answer here.

---

**2. What evidence proves that the service recovered?**

Add your answer here.

---

**3. Why is the second triage run necessary?**

Add your answer here.

---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

Add your answer here.

---

**5. In one sentence, explain the difference between using AI as a chatbot and using AI in this agentic workflow.**

Add your answer here.

---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** Add your full name here

**Date:** DD/MM/YYYY

---

**1. Reported Symptom**

Add your answer here.

---

**2. Evidence Collected**

Add your answer here.

---

**3. Most Likely Cause**

Add your answer here.

---

**4. Human-Approved Recovery Action**

Add your answer here.

---

**5. Verification**

Add your answer here.

---

**6. Safety Decision**

Add your answer here.

---

**7. Agentic Loop Mapping**

Add your answer here.

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

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

`Add your URL here`

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

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
