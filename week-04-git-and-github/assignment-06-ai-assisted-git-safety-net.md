# Assignment 6 — Building an AI-Assisted Git Safety Net (PR Ready Check)

Part of the DevOps Micro Internship (DMI) with Agentic AI

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

Add your screenshot here.

---

### Notes

**1. Why create a dedicated branch instead of doing this work on main?**

A dedicated branch allows developers to work on new features or changes independently without affecting the stable main branch.

1.Isolation: Keeps new changes separate from the main project code.

2.Safety: Prevents unfinished or incorrect changes from affecting the main branch.

3.Testing and Review: Allows developers to test and review changes before merging them into main.

4.Collaboration: Enables multiple developers to work on different features simultaneously.

---

# Task 1 — Stage a Change With Realistic Risk

## Goal

On your own fork of this repository (the one you've been submitting your DMI work in since onboarding), create a new branch and stage a change that a real reviewer should catch: a hardcoded-looking secret and a leftover debug statement.

### Evidence

#### Screenshot 1 — Output of  `git status` showing the staged file on feature/ai-pr-ready

Add your screenshot here.

---

### Notes

**1. Why does this assignment use an obviously fake key instead of a real one?**

An assignment uses an obviously fake key to demonstrate how API keys and secrets are handled safely without exposing real credentials.

1.Security: Prevents the accidental exposure or misuse of real API keys.

2.Learning Purpose: Helps students understand how applications use keys for authentication.

3.Safe Testing: Allows students to practice configuration and testing without connecting to real services.

4.Cost and Privacy Protection: Prevents unexpected charges and protects access to personal or organizational accounts.

---

# Task 2 — Write a Real Git Pre-Commit Hook

## Goal

Create a tracked, shareable pre-commit hook that blocks a commit containing secret-like patterns or files over 1MB.

### Evidence

#### Screenshot 2 — `hooks/pre-commit` open in VS Code showing the full script

Add your screenshot here.

---

#### Screenshot 3 — Output of `git config core.hooksPath` confirming it points to `hooks`

Add your screenshot here.

---

### Notes

**1. Why is `hooks/pre-commit` tracked in the repo instead of living only in `.git/hooks/`?**

The hooks/pre-commit file is tracked in the repository so that the pre-commit hook script can be shared with all team members. It allows developers to maintain, review, and update the script using Git version control. It also ensures consistency by applying the same checks and rules before committing code. In contrast, the .git/hooks/ directory is local to each repository, and its files are not tracked by Git.
---

**2. Compare this to `PreToolUse` from Week 2 Assignment 6. What does each one intercept, and what do they have in common?**

The hooks/pre-commit Git hook intercepts a Git commit before it is created, while PreToolUse intercepts an AI agent’s tool call before the tool executes. Both check actions before they happen and help enforce safety rules. The main difference is that pre-commit controls Git commits, whereas PreToolUse controls AI agent tool usage.
---

# Task 3 — Prove the Hook Blocks the Risky Commit

## Goal

Attempt to commit the staged file from Task 1 and show the hook rejecting it.

### Evidence

#### Screenshot 4 — Terminal showing `git commit` rejected with the hook's "BLOCKED" message naming the exact file

Add your screenshot here.

---

### Notes

**1. Which line in `hooks/pre-commit` matched your fake key, and why did it match?**

The line in hooks/pre-commit that matched the fake key was the pattern-checking line that searches for API keys or secret-like strings. It matched because the fake key followed the pattern defined in the script, making it look like a credential. This demonstrates how a pre-commit hook can detect potential secrets and prevent them from being committed to the repository.
---

**2. Could this hook have caught a poorly-named variable that stores a secret without the `AKIA` prefix? What does that tell you about the limits of a fixed rule like this?**

No, this hook might not catch a poorly named variable that stores a secret without the AKIA prefix if it only checks for that specific pattern. This shows that fixed rules can detect known secret formats but may miss secrets written in different formats or hidden in variables. Therefore, a pre-commit hook is useful for basic security checks, but it cannot guarantee that all secrets will be detected.

---

# Task 4 — Build the `/pr-ready` Skill

## Goal

Create a manually invoked Claude Code skill that reads your staged changes and produces a PR-readiness report and a draft PR description — without writing, committing, or pushing anything itself.

### Evidence

#### Screenshot 5 — `SKILL.md` frontmatter showing `allowed-tools: Bash, Read, Grep` (no `Write`) and `disable-model-invocation: true`

Add your screenshot here.

---

#### Screenshot 6 — `/pr-ready` output while the risky file is still staged, showing it flagged the secret and/or debug statement

Add your screenshot here.

---

### Notes

**1. Why does `/pr-ready` have `Bash` and `Read` but not `Write`?**

The /pr-ready skill has Bash and Read permissions so it can run verification commands and inspect files before a pull request is submitted. It does not have Write permission because its purpose is to check readiness, not modify project files. This follows the principle of least privilege, reducing the risk of accidental changes and keeping the review process safe and reliable.

---

**2. The pre-commit hook and `/pr-ready` both looked at the same staged diff. Did they flag the same things? What did one catch that the other didn't?**

The pre-commit hook and /pr-ready did not necessarily flag the same things because they perform different checks. The pre-commit hook detects secrets using fixed patterns, such as a fake API key with an AKIA prefix. The /pr-ready skill reviews the staged changes for broader issues, such as code quality and pull request readiness. The hook may catch a matching secret that the review overlooks, while /pr-ready may identify problems that the fixed rules cannot detect. Both complement each other to improve code quality and security.
---

# Task 5 — Fix the Issues and Re-Verify

## Goal

Remove the secret and debug statement, then prove both gates now pass clean.

### Evidence

#### Screenshot 7 — `git commit` succeeding after the fix (no BLOCKED message)

Add your screenshot here.

---

#### Screenshot 8 — Second `/pr-ready` run showing a clean risk report and a drafted PR title + description

Add your screenshot here.

---

### Notes

**1. What exactly did you change to satisfy the pre-commit hook?**

I removed the fake API key from the staged changes and replaced it with a harmless placeholder value. This satisfied the pre-commit hook because the file no longer contained a string matching the secret-detection pattern. After making the change, I staged the updated file again and retried the commit.
---

# Task 6 — Push and Open a Pull Request Using the AI Draft

## Goal

Push your branch and open a real Pull Request, using `/pr-ready`'s drafted title and description as your starting point — read it critically and edit before you use it.

**Important:** Open this Pull Request with base repository set to **your own fork** — not the shared upstream `pravinmishraaws/devops-micro-internship-pravinmishra` repository. This assignment's hook and skill files are your own practice work, not a change meant for the shared class repo.

### Evidence

#### Screenshot 9 — Your Pull Request showing the base repository is your own fork, plus the title and description, with the `/pr-ready` draft visible for comparison (paste it in the PR conversation or your notes below)

Add your screenshot here.

---

#### PR Link

Add your PR URL here...

---

### Notes

**1. What, if anything, did you edit in the AI's drafted PR description before using it? Why?**

I reviewed the AI-drafted PR description and corrected any inaccurate or unclear statements before using it. I ensured that it accurately described the changes made, the checks performed, and the purpose of the updates. This was important because AI-generated content may contain incorrect assumptions or information that does not match the actual work. Human review helps ensure the PR description is clear, accurate, and trustworthy.

---

**2. If you had blindly copy-pasted the AI's draft without reading it, what could go wrong?**

If I had blindly copy-pasted the AI's draft without reviewing it, the PR description might contain incorrect information, unsupported claims, or changes that were not actually made. It could mislead reviewers, hide security issues, and cause confusion during code review. Therefore, AI-generated content must always be checked for accuracy and relevance before submission. Human verification ensures that the final PR description is reliable and reflects the actual work completed.

---

**3. Why does this PR need to target your own fork instead of the shared upstream repository?**

This PR needs to target my own fork instead of the shared upstream repository because I may not have permission to make changes directly to the original repository. Working on my fork allows me to develop and test changes safely without affecting the shared project. After reviewing and testing the changes, I can submit a pull request to the upstream repository for approval. This protects the original codebase and supports safe collaboration
---

# Task 7 — Map the Workflow to the Agentic Loop

## Goal

Explain this assignment's workflow using the same Gather → Analyze → Human Act → Verify structure from Week 3.

### Notes

**1. Which step(s) represent Gather?**

Add your answer here.

---

**2. Which step(s) represent Analyze?**

Add your answer here.

---

**3. Which step is Human Act, and why must a human — not Claude — run `git commit`, `git push`, and open the PR?**

Add your answer here.

---

**4. Which step is Verify?**

Add your answer here.

---

**5. In one or two sentences: why do you need *both* the fixed-rule pre-commit hook and the AI skill? Isn't one enough?**

Add your answer here.

---

# Task 8 — LinkedIn Post

## Goal

Publish a LinkedIn post summarizing what you built and what you learned about combining fixed-rule safety checks with AI-assisted review.

### Evidence

#### LinkedIn Post URL

Add your LinkedIn post URL here...

---

## Key Learnings

Add 3-5 bullet points on what you learned this week.

-
-
-

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

`Add your URL here`

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

- 🌐 DMI Official Website: https://dmi.pravinmishra.com?utm_source=github&utm_medium=readme  
- 🎓 University: https://university.pravinmishra.com?utm_source=github&utm_medium=readme  
- 💬 Discord Community: https://discord.pravinmishra.com?utm_source=github&utm_medium=readme  
- 📝 Blog: https://dmi.pravinmishra.com/blog?utm_source=github&utm_medium=readme  
- ▶️ YouTube Playlist: https://www.youtube.com/playlist?list=PLFeSNDtI4Cho  
- 🔗 Pravin Mishra (LinkedIn): https://www.linkedin.com/in/pravin-mishra-aws-trainer/  
- 🏢 CloudAdvisory (LinkedIn): https://www.linkedin.com/company/thecloudadvisory/

---

*This submission is part of DevOps Micro Internship (DMI) — Agentic AI Track.*
