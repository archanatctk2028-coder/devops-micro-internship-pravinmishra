# Assignment 3 — Production Maintenance Drill (OPS Checklist)

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will treat your already deployed React application (on Ubuntu VM with Nginx) as a live production system. You will perform structured operational checks covering network validation, service health, log analysis, resource monitoring, configuration verification, and incident simulation with recovery — mirroring real on-call DevOps responsibilities.

---

# Task 1 — Server Access & Networking Validation

## Goal

Verify that the deployed React application is reachable from the browser and confirm basic network connectivity of the Ubuntu VM.

### Evidence

#### Screenshot 1 — Browser showing the React app with your Full Name visible on the UI

Add your screenshot here.

---

#### Screenshot 2 — Output of `ip a`

<img width="732" height="712" alt="image" src="https://github.com/user-attachments/assets/cba1f9af-68de-4417-8a68-f3c311318a1a" />


---

#### Screenshot 3 — Output of `sudo ss -tulpen`

<img width="1727" height="937" alt="image" src="https://github.com/user-attachments/assets/be3e8875-01db-4a4a-86f8-b18b4552a384" />

---

#### Screenshot 4 — Output of `sudo ufw status`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What proves Nginx is listening on 0.0.0.0:80?**

The command sudo ss -tlnp | grep :80 can be used to check whether Nginx is listening on port 80.
If the output shows 0.0.0.0:80, it proves that Nginx is listening on port 80 on all network interfaces. This means the server can accept HTTP connections from external clients.
---

**2. What proves SSH is active on port 22?**

The command sudo ss -tlnp | grep :22 can be used to check SSH.
If the output shows :22 with sshd, it proves that the SSH service is active and listening on port 22. This allows remote users to connect to the server using SSH.

---

**3. Did you find any unexpected open ports? Explain briefly.**

After checking the server’s open ports, no unexpected open ports were found. The required ports, such as 22 for SSH and 80 for HTTP, were open. This indicates that the server is properly configured and unnecessary network access is avoided.
---

# Task 2 — Service Health & Systemd Validation (Nginx)

## Goal

Verify that Nginx is properly installed, running, enabled at boot, and safely configured.

### Evidence

#### Screenshot 1 — Output of `systemctl status nginx --no-pager`

<img width="1372" height="730" alt="image" src="https://github.com/user-attachments/assets/37fa019d-f4ad-41af-b94f-8ce8a837a791" />

---

#### Screenshot 2 — Output of `sudo nginx -t`

<img width="640" height="157" alt="image" src="https://github.com/user-attachments/assets/a174e040-7b35-43ff-acf7-ba520c3285dd" />

---

#### Screenshot 3 — Output of `sudo ss -lptn '( sport = :80 )'`

<img width="1782" height="976" alt="Screenshot 2026-10-06 181825" src="https://github.com/user-attachments/assets/bde6c5f9-437e-41bf-9381-ac27a5a84179" />



---

### Notes

Answer the following in your own words:

**1. What happens if Nginx fails to restart in production?**

If Nginx fails to restart in production, the website or application may become unavailable to users. New requests may not reach the application server, causing errors or downtime. The failure should be checked using logs and configuration tests, and the problem should be fixed before restarting Nginx again. This helps maintain availability and reliability of the application.
---

**2. What's your basic rollback plan?**

A rollback plan is a method of returning an application to its previous stable version when a new deployment fails.

Identify the deployment problem.
Stop or pause the failed deployment.
Restore the previous stable version.
Restart the required services.
Test the application to confirm it is working properly.

This helps reduce downtime and restore the application quickly.
---

# Task 3 — Logs & Request Trace

## Goal

Verify real traffic flow and analyze logs to understand system behavior and errors.

### Evidence

#### Screenshot 1 — Output of `sudo tail -n 30 /var/log/nginx/access.log`

Add your screenshot here.

---

#### Screenshot 2 — Output of `sudo tail -n 30 /var/log/nginx/error.log`

Add your screenshot here.

---

#### Screenshot 3 — Output of `sudo journalctl -u nginx --no-pager -n 50`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Were there any errors in the logs?**

- If yes, mention 1–2 example error lines from the logs and explain what each one means in simple terms.
- If no, explain what it means if the error log is empty or shows no recent errors during your check.

During the check, no recent errors were found in the Nginx error log. An empty error log means that Nginx did not report any major problems during the checked period. This indicates that the server and Nginx service were working normally at the time of the check.

---

**2. If there were no errors, what does that indicate about the system?**

If there were no errors, it indicates that the system is functioning normally and reliably. The services are running properly, and no major problems were detected in the logs during the checking period. It also suggests that the system configuration is working as expected. However, regular monitoring is still important to detect future issues.

---

**3. Based on the access logs, were your curl requests visible in the log entries? What does that prove about traffic flow?**

Yes, the curl requests were visible in the Nginx access logs. This proves that the requests successfully reached the Nginx server and were processed. It confirms that the network traffic was flowing correctly from the client to the server through Nginx.

---

# Task 4 — System Resource Health Check (Capacity Red Flags)

## Goal

Assess server capacity and detect potential performance or failure risks.

### Evidence

#### Screenshot 1 — Output of `uptime`

Add your screenshot here.

---

#### Screenshot 2 — Output of `free -h`

Add your screenshot here.

---

#### Screenshot 3 — Output of `df -h`

Add your screenshot here.

---

#### Screenshot 4 — Output of `sudo du -sh /var/* | sort -h`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. Which resource looks most critical right now? (CPU/load, memory, or disk) Explain why.**

The memory (RAM) looks most critical right now because high memory usage can slow down the server and affect application performance. If memory becomes full, services may become unstable or stop working. Therefore, memory usage should be monitored regularly.

---

**2. What happens if disk becomes 100% full in a production server?**

If the disk becomes 100% full, the server may not be able to create or save new files. This can cause applications and services to fail, logs may stop recording, and databases may face problems. It can also make the server slow or unavailable. Therefore, disk usage should be monitored and unnecessary files or old logs should be removed regularly.
---

# Task 5 — Configuration & Deployment Verification

## Goal

Ensure the correct React build is deployed and Nginx is serving it properly.

### Evidence

#### Screenshot 1 — Output of `ls -lah /var/www/html | head -n 20`

Add your screenshot here.

---

#### Screenshot 2 — Output of `grep -R "Deployed by" -n /var/www/html 2>/dev/null | head`

Add your screenshot here.

---

#### Screenshot 3 — Output of `grep -n "try_files" /etc/nginx/sites-available/default`

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. How do you confirm that the correct version of the application is deployed?**

We can confirm the correct application version by checking the version number or Git commit ID deployed on the server. We can also compare it with the expected release version and test the application. If they match, it confirms that the correct version is deployed.
---

# Task 6 — Nginx Configuration Failure Simulation

## Goal

Simulate a real-world Nginx misconfiguration and recover the service safely.

### Evidence

#### Screenshot 1 — Output of `sudo nginx -t` showing the syntax error (broken config)

Add your screenshot here.

---

#### Screenshot 2 — Output of `sudo nginx -t` showing syntax ok (fixed config)

Add your screenshot here.

---

#### Screenshot 3 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What caused the configuration failure?**

The configuration failure was caused by an incorrect or invalid configuration setting in the Nginx configuration file. Because of this, Nginx could not validate or load the configuration properly. Correcting the configuration and testing it with nginx -t resolves the issue.
---

**2. How did you fix the issue?**

The issue was fixed by identifying and correcting the incorrect Nginx configuration. First, the configuration was tested using nginx -t. After fixing the error, Nginx was restarted successfully. Finally, the application was checked to confirm that it was working properly.

---

**3. How can you avoid this kind of issue in real production systems?**

This type of issue can be avoided by:

Testing configuration before applying changes using nginx -t.
Taking backups of working configuration files.
Using version control to track configuration changes.
Testing changes in a staging environment before production.
Keeping a rollback plan ready in case of failure.

These practices help reduce downtime and production errors.
---

# Task 7 — Web Application Failure Simulation

## Goal

Simulate missing deployment content and recover the application safely.

### Evidence

#### Screenshot 1 — Output of `curl -I http://<public-ip>` showing failure (non-200 response)

Add your screenshot here.

---

#### Screenshot 2 — Output of `curl -I http://<public-ip>` confirming recovery (200 OK)

Add your screenshot here.

---

### Notes

Answer the following in your own words:

**1. What caused the application to break in this scenario?**

The application broke because of an incorrect configuration change. The invalid configuration caused the application or Nginx service to fail. After identifying and correcting the configuration error, the service was restarted and the application worked normally again.

---

**2. How did you fix the issue and restore the application?**

The issue was fixed by identifying and correcting the incorrect configuration. The configuration was tested using nginx -t, and then Nginx was restarted successfully. Finally, the application was tested to confirm that it was restored and working normally.

---

**3. What steps would you take to prevent this kind of issue in real production systems?**

To prevent this issue in production systems:

1.Test configurations before deployment using nginx -t.
2.Keep regular backups of working configurations.
3.Use version control to track changes.
4.Test changes in a staging environment first.
5.Maintain a rollback plan for quick recovery.

---

# Task 8 — Security & Reliability Review

## Goal

Review and reflect on the security and reliability practices applied during this assignment.

### Security & Reliability Notes

Answer the following in your own words:

**1. Why is SSH key-based authentication more secure than sharing passwords?**

SSH key-based authentication is more secure because it uses a private key and public key instead of a password. The private key is kept securely on the user’s device and is not shared with the server. It is also harder to guess or crack than a simple password, providing stronger protection against unauthorized access.
---

**2. Why should only required ports be open on a production server?**

Only required ports should be open on a production server to **reduce security risks**. Every open port can provide a possible entry point for attackers. Closing unnecessary ports reduces the **attack surface** and helps protect the server from unauthorized access and attacks.


---

**3. Why is it important for Nginx to be enabled on boot?**

Nginx should be enabled on boot so that it starts automatically whenever the server restarts. This ensures that the website or application becomes available without manual intervention. It helps maintain service availability, reliability, and reduces downtime.
---

**4. What are the risks of sharing secrets, keys, or credentials publicly?**

Sharing secrets, keys, or credentials publicly can allow unauthorized people to access systems and data. It may lead to data theft, financial loss, service disruption, or security attacks. Therefore, credentials should be kept private and stored securely using environment variables or secret-management tools.
---

**5. Why should cloud resources be stopped or terminated when they are no longer needed?**


Cloud resources should be **stopped or terminated when they are no longer needed** because they may continue to consume resources and incur charges. Removing unused resources helps **reduce costs**, improve resource management, and avoid unnecessary security risks.


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

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
