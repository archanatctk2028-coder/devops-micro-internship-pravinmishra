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

#### Screenshot 1 — Output of `systemctl is-active nginx`, `ss -ltn | grep ':80'`, and `curl -I http://localhosthhhh

<img width="1210" height="558" alt="Screenshot 2026-10-11 122758" src="https://github.com/user-attachments/assets/6b895542-a73e-4387-a809-61900bc74e52" />

---

#### Screenshot 2 — Output of `pwd` and `find . -maxdepth 4 -type d | sort` showing the workspace folder structure

<img width="1177" height="390" alt="Screenshot 2026-10-11 123159" src="https://github.com/user-attachments/assets/9aa4b5d2-7f12-4f44-beac-8bf3e1d146e6" />



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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/c159f419-da47-4d11-b337-89bb79c9af1e" />


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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e2182ec6-fd57-444f-b31e-342e36d9e18d" />

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

<img width="772" height="337" alt="image" src="https://github.com/user-attachments/assets/b26a2516-f082-43ac-9e6d-791eb982c008" />


---

#### Screenshot 6 — Middle section showing check functions and conditionals

<img width="836" height="733" alt="image" src="https://github.com/user-attachments/assets/ede442e0-85bb-4665-8bee-526f93da32de" />

---

#### Screenshot 7 — Bottom section showing the loop, summary function, and exit behavior

<img width="1486" height="793" alt="image" src="https://github.com/user-attachments/assets/c4f38993-4031-40e3-9dd5-8601ffa94a91" />


---

#### Screenshot 8 — Output of `bash -n scripts/linux-triage.sh` (no syntax errors) and `ls -l scripts/linux-triage.sh` showing executable permission

<img width="705" height="101" alt="image" src="https://github.com/user-attachments/assets/4361f1c0-79eb-44f4-a7d0-504c8d9f84e7" />


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

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/27be18b1-6704-4017-892a-393f1b058504" />

---

#### Screenshot 10 — Output showing the captured exit code and final summary

<img width="762" height="315" alt="image" src="https://github.com/user-attachments/assets/833558fd-6d6e-4b4f-b175-ccb0ed715ced" />


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

<img width="810" height="675" alt="image" src="https://github.com/user-attachments/assets/daf0fc06-4c5b-40ca-9240-22781504f253" />


---

#### Screenshot 12 — `/linux-triage` output for the healthy server

<img width="1067" height="497" alt="image" src="https://github.com/user-attachments/assets/d0fc5fc5-71ad-486f-8aca-aa354852a766" />

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

<img width="1007" height="228" alt="image" src="https://github.com/user-attachments/assets/ab08e7b0-7fe0-40c3-a4f5-80ef156a2864" />


---

#### Screenshot 14 — `/linux-triage` output showing failed evidence, most likely cause, and a suggested recovery command

<img width="866" height="593" alt="image" src="https://github.com/user-attachments/assets/e994ac8b-6ac1-4b89-829c-2233548a9a43" />

#### Screenshot 15 — `incident-failure-report.txt` showing the failed checks and your Full Name

<img width="747" height="835" alt="image" src="https://github.com/user-attachments/assets/3659412b-d83a-42f4-85e0-434ceb470bf6" />


---

### Notes

Answer the following in your own words:

**1. Which three checks failed?**

In a Linux health monitoring script, three checks that may fail during an incident are:

Nginx Service Check: Fails when the Nginx web server is stopped or inactive.

Disk Space Check: Fails when the available disk space is below the required limit.

Memory Check: Fails when the available RAM is below the minimum required threshold.

These checks help identify system problems and maintain server reliability. The script reports failures so that the administrator can take corrective action.

---

**2. What evidence supports the conclusion that Nginx is unavailable?**

Nginx is considered unavailable when the following evidence is observed:

Service Status: The command systemctl is-active nginx returns inactive or failed.

HTTP Response: The command curl -I http://localhost fails to connect to the server.

Port Check: Nginx is not listening on port 80, which can be checked using ss -tuln.

System Logs: The command journalctl -u nginx may show errors or messages indicating that the service stopped or failed to start.
---

**3. Did Claude execute the recovery command? Why is that important?**

Did Claude execute the recovery command? Why is that important? (4 Marks)

Claude executed the recovery command to restart the Nginx service and restore its availability.

Recovery: The command helps restart Nginx if the service has stopped.

Verification: After recovery, the service status can be checked using systemctl is-active nginx.

Availability: It helps restore access to the website if Nginx was unavailable.

Safety: Verifying the result confirms whether the recovery was successful instead of assuming the problem is fixed.
---

**4. Which phase of the Agentic Loop is represented by the Bash report?**

The Bash report represents the Verify phase of the Agentic Loop because it checks the system's health and confirms whether the system is working correctly. It displays the results, such as PASS, WARN, FAIL, and the overall status. This helps determine whether the recovery action was successful.

---

**5. Which phase is represented by Claude's explanation?**

Claude's explanation represents the Gather phase of the Agentic Loop because Claude analyzes the collected system information, identifies possible problems, and explains the evidence behind the system's status.

---

# Task 8 — Recover Manually, Verify Again, and Write the Incident Summary

## Goal

Recover the service as the human operator and prove that the system is healthy again.

### Evidence

#### Screenshot 16 — Output showing Nginx is active and `curl -I http://localhost` returns 200 OK

<img width="710" height="427" alt="image" src="https://github.com/user-attachments/assets/bc13773f-7146-49ec-8ef6-1a9ac25fd359" />

---

#### Screenshot 17 — Second `/linux-triage` output showing successful recovery with no FAIL results

<img width="787" height="823" alt="image" src="https://github.com/user-attachments/assets/f072e5b6-84a1-435f-9a32-5f6133aa7c08" />

---

#### Screenshot 18 — Output of `ls -lah reports` showing both `incident-failure-report.txt` and `recovery-report.txt`

<img width="797" height="232" alt="image" src="https://github.com/user-attachments/assets/035f5e33-11f1-4f58-869d-70ae6d7b5774" />

---

#### Screenshot 19 — `incident-summary.md` showing all required sections and your Full Name

<img width="802" height="886" alt="image" src="https://github.com/user-attachments/assets/f2b9652b-e0a8-4375-8d32-d4743e993dfb" />


---

### Notes

Answer the following in your own words:

**1. What action did you execute manually?**

I manually executed the Bash command to copy the Linux health report into incident-failure-report.txt and used the cat command to view the report and verify its contents.

---

**2. What evidence proves that the service recovered?**

The evidence that proves the service recovered is that the Nginx service status shows active (running), the HTTP request using curl -I http://localhost returns HTTP/1.1 200 OK, and the system logs confirm that Nginx started successfully. These results indicate that Nginx is running and the website is accessible again.
---

**3. Why is the second triage run necessary?**

The second triage run is necessary to verify that the recovery action was successful. It checks the Nginx service status, memory, disk space, and other system health conditions again. By comparing the results of the first and second runs, we can confirm whether the problem is resolved and the system has returned to a healthy state.
---

**4. What could go wrong if an AI agent automatically restarted every failed service?**

 What could go wrong if an AI agent automatically restarted every failed service?**
 
 --

**5.  What could go wrong if an AI agent automatically restarted every failed service?**
An AI agent that automatically restarts every failed service could cause service interruptions, data loss, and failures in dependent applications. Repeated restarts may create an endless restart loop without fixing the root cause. It could also restart critical services unnecessarily. Therefore, the AI agent should analyze the failure, assess the risks, obtain approval when required, and verify the service status after recovery.
---

# Incident Summary

Fill in all seven sections below in your own words.

**Full Name:** Kota Archana

**Date:** 11/10/2026

---

**1. Reported Symptom**

The Nginx web server became unavailable, and users were unable to access the website through their browser. The issue may have occurred because the Nginx service stopped unexpectedly, failed to start correctly, or encountered a configuration or resource-related problem. To investigate the incident, a Linux health-triage Bash script was executed to check the Nginx service status, available memory, disk space, listening ports, and recent system logs.

---

**2. Evidence Collected**

The Linux health-triage Bash script checked the Nginx service status, available memory, disk space, listening ports, and system logs. The report showed 7236 MB of available memory, 5 PASS checks, 0 WARN, and 0 FAIL, with the overall status HEALTHY. The system logs confirmed that Nginx started successfully, indicating that the service was running after recovery.
---

**3. Most Likely Cause**

The most likely cause of the incident was that the Nginx service stopped temporarily, making the website unavailable. This may have happened due to a service interruption or a configuration issue. The system logs showed that Nginx stopped successfully and was later started again. Further investigation of the logs and configuration is needed to confirm the exact root cause.
---

**4. Human-Approved Recovery Action**

After reviewing the system health report and identifying that Nginx was unavailable, the administrator approved the recovery action. The Nginx service was restarted using the command sudo systemctl restart nginx. Then, the service status was checked using systemctl is-active nginx, and the website was tested using curl -I http://localhost. The successful service status and HTTP response confirmed that Nginx had recovered and the website was accessible again.

---

**5. Verification**
After the recovery action, the Nginx service was checked using systemctl is-active nginx. The command returned active, confirming that the service was running. The website was tested using curl -I http://localhost, which returned an HTTP 200 OK response. Finally, the Linux health-triage script was executed again and reported PASS: 5, WARN: 0, FAIL: 0, with the overall status HEALTHY. These results confirmed that the service had recovered successfully.Add your answer here.

---

**6. Safety Decision**

The recovery action was performed only after reviewing the system health report and obtaining human approval. Automatically restarting every failed service could cause downtime, data loss, or repeated failures. Therefore, the AI agent should analyze the evidence, recommend a suitable recovery action, and request human approval when necessary. After recovery, the service must be checked again to confirm that it is working correctly and safely.

---

**7. Agentic Loop Mapping**

The Agentic Loop consists of three main phases: Gather, Act, and Verify. In the Gather phase, Claude collects and analyzes system information, including Nginx status, memory usage, and system logs. In the Act phase, the human-approved recovery command is executed to restore the Nginx service. In the Verify phase, the Bash health-triage script runs again to confirm that the service is working correctly. The final report shows HEALTHY, confirming successful recovery.
---

# LinkedIn Post (Required)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

https://lnkd.in/p/dFMTNtq3

---

#### Screenshot — Published LinkedIn post

<img width="1920" height="1080" alt="Screenshot 2026-10-11 155330" src="https://github.com/user-attachments/assets/d40c5e85-7d25-40e8-8e4b-066c4a67ba93" />


---

# GitHub Repository URL

Paste the URL of your GitHub folder or repository containing the assignment files here:

https://github.com/archanatctk2028-coder/Ultimate-Agentic-DevOps-with-Claude-Code
https://github.com/archanatctk2028-coder/devops-micro-internship-pravinmishra
---

# Submission Instructions

- Add all required screenshots in your submission
- Full Name must be visible in required screenshots and the Bash report
- All written answers must be in your own words
- Do not expose sensitive information (keys, passwords, AWS account IDs, tokens)
- GitHub URL must be included in this document

---

# Completion Checklist

- [✅] Task 1: Healthy baseline confirmed, workspace created (Screenshots 1–2, Notes answered)
- [✅] Task 2: CLAUDE.md created with all four sections (Screenshot 3, Notes answered)
- [✅] Task 3: Five-check plan produced by Claude using read-only tools (Screenshot 4, Notes answered)
- [✅] Task 4: `linux-triage.sh` created, syntax validated, executable permission set (Screenshots 5–8, Notes answered)
- [✅] Task 5: Healthy-state report generated with no FAIL result (Screenshots 9–10, Notes answered)
- [✅] Task 6: `/linux-triage` skill created and run successfully on healthy server (Screenshots 11–12, Notes answered)
- [✅] Task 7: Nginx incident simulated, failed evidence captured, Claude did not execute recovery (Screenshots 13–15, Notes answered)
- [✅] Task 8: Nginx recovered manually, recovery verified, reports saved, incident summary complete (Screenshots 16–19, Notes answered)
- [✅] Incident summary contains all seven required sections
- [✅] LinkedIn post published and URL submitted
- [✅] Full Name visible in all required screenshots and the Bash report
- [✅] Skill does not have Write permission
- [✅] Skill did not execute any recovery commands
- [✅] No sensitive data exposed

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
