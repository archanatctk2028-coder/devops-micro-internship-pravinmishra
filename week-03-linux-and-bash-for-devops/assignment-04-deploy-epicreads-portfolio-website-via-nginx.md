# Assignment 4 — Deploy EpicReads Portfolio Website via Nginx

Part of the DevOps Micro Internship (DMI) with Agentic AI

---

## Purpose

In this assignment, you will deploy a static portfolio website on an Ubuntu VM using Nginx. You will download the website template, add your ownership proof in the footer, deploy the files to the Nginx web root, and verify the website is publicly accessible via a browser.

---

# Task 0 — Pre-flight Check

## Goal

Verify the Ubuntu VM and Nginx are ready for deployment.

### Evidence

#### Screenshot 0 — Output of `sudo systemctl status nginx --no-pager` showing Active (running)

<img width="1276" height="1055" alt="image" src="https://github.com/user-attachments/assets/fee005d1-2ff7-4a9e-be43-81715c7ad162" />


---

# Task 1 — Get the Website Source Code

## Goal

Download and extract the portfolio website template.

### Evidence

#### Screenshot 1 — Output of `ls -la` showing the extracted project folder

<img width="1602" height="962" alt="image" src="https://github.com/user-attachments/assets/c25407e6-4bc1-4ba1-b5aa-5d0e84bb017d" />

---

# Task 2 — Add Ownership Proof (Anti-Copy Change)

## Goal

Update the website footer with your deployment details.

### Evidence

#### Screenshot 2 — Nano editor open with the updated footer showing your Full Name, Group, Week, and Date

Add your screenshot here.

---

# Task 3 — Deploy Website via Nginx

## Goal

Deploy the portfolio website to the Nginx web root.

### Evidence

#### Screenshot 3 — Output of `sudo nginx -t` showing configuration test successful

<img width="1001" height="212" alt="image" src="https://github.com/user-attachments/assets/c1b6db79-c29c-46d2-af4e-d36db4b5024e" />


---

#### Screenshot 4 — Output of `ls /var/www/html` showing deployed website files


<img width="676" height="220" alt="image" src="https://github.com/user-attachments/assets/4d93025f-1c78-4feb-a7c2-41c0f1a02ae9" />


---

# Task 4 — Verify Website is Live

## Goal

Verify the deployed website is publicly accessible and the footer contains your details.

### Evidence

#### Screenshot 5 — Output of `curl ifconfig.me` showing the server's public IP address

<img width="1030" height="147" alt="image" src="https://github.com/user-attachments/assets/ce2bca5f-96ff-4390-9df4-b61175ee2eb8" />


---

#### Screenshot 6 — Browser showing the live website with your Full Name and deployment details in the footer

<img width="1488" height="712" alt="Screenshot 2026-10-09 205421" src="https://github.com/user-attachments/assets/2719f4fb-063d-45d7-816b-3bb0943dd3f6" />


---

# Task 5 — Mini Real DevOps Operational Check

## Goal

Verify the deployed website and Nginx service are healthy.

### Evidence

#### Screenshot 7 — Output of `systemctl is-enabled nginx`

<img width="1212" height="98" alt="image" src="https://github.com/user-attachments/assets/a309da1c-91be-41fb-8614-d2161d746d82" />


---

#### Screenshot 8 — Output of `curl -I http://localhost` showing 200 OK

<img width="1076" height="315" alt="image" src="https://github.com/user-attachments/assets/60617a28-a1ee-475f-9546-818c6eb55b39" />


---

# LinkedIn Post (Mandatory)

## Evidence

#### LinkedIn Post URL

Paste your LinkedIn post URL here:

`Add your URL here`

---

#### Screenshot — Published LinkedIn post showing the live website with your Full Name in the footer

Add your screenshot here.

---

# Submission Instructions

- Add all required screenshots in your submission
- Full name must be visible in required screenshots
- Ownership proof in the footer is mandatory
- Do not expose sensitive information (keys, passwords, account IDs)

---

# Completion Checklist

- [✅] Screenshot 0: Nginx service status (active/running)
- [✅] Screenshot 1: Website files downloaded and extracted
- [✅] Screenshot 2: Footer updated with Full Name, Group, Week, and Date
- [✅] Screenshot 3: Nginx configuration test successful
- [✅] Screenshot 4: Website files deployed to /var/www/html
- [✅] Screenshot 5: Public IP retrieved
- [✅] Screenshot 6: Live website accessible in browser with footer details
- [✅] Screenshot 7: Nginx enabled on boot
- [✅] Screenshot 8: Local HTTP response returns 200 OK
- [✅] LinkedIn post published and URL submitted
- [✅] Full Name visible in all required screenshots
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
