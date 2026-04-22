## Theory Summary

### Git Workflow (4 Steps)
[Working Directory] → [Staging Area] → [Local Repository] → [Remote Repository (GitHub)]
| | | |
You edit files git add . git commit git push

text

| Step | Command | What Happens |
|------|---------|--------------|
| 1. Modify | (edit files) | Change your files |
| 2. Stage | `git add` | Tell Git which changes to save |
| 3. Commit | `git commit` | Save changes locally with a message |
| 4. Push | `git push` | Upload to GitHub |

### Commits

A commit is a snapshot of your files at a point in time.
Commit 1: "Initial setup"
Commit 2: "Add day1 lab"
Commit 3: "Fix typo in README"
Commit 4: "Day 2 completed"

text

### Branches

A branch is a separate line of development.
main: A---B---C---D

feature: E---F

text

### PR Mindset (Pull Request)

A Pull Request (PR) is how you propose changes in a team:

    Create a branch

    Make changes

    Open a PR

    Team reviews

    Merge to main

### Markdown Runbooks

Markdown is a simple way to format text. Files end in .md.

## Practical Lab
Day 10 - Git and Professional Documentation
Date: March 10, 2026
What I Learned Today
Git workflow: add → commit → push

Importance of small, meaningful commits

Branching and PR mindset

Markdown syntax for documentation

How to structure a professional repo

Commands Used
git init - Initialize repository

git add . - Stage all changes

git commit -m "message" - Commit with message

git remote add origin <url> - Connect to GitHub

git push -u origin main - Push to github

text

## Commands Used

| Command | Purpose |
|---------|---------|
| `git init` | Initialize repository |
| `git add .` | Stage all changes |
| `git commit -m "message"` | Commit with message |
| `git remote add origin <url>` | Connect to GitHub |
| `git push -u origin main` | Push to GitHub |

## Repository Structure (Professional Standard)

For a professional portfolio, your repository should have this structure:
linux-junior-devops-bootcamp/
├── README.md # Top-level project overview
├── ARCHITECTURE.md # System design documentation
├── day01/
│ └── day01-report.md
├── day02/
│ └── day02-report.md
├── day04/
│ └── day04-report.md
├── day08/
│ └── day08-report.md
├── day10/
│ └── day10-report.md
└── runbooks/ # Reusable operation guides
├── ufw-setup.md
├── nginx-troubleshoot.md
└── git-workflow.md

text

### README.md (Top-level Project Overview)

Create this file in your repository root:

```markdown
# Linux Junior DevOps Bootcamp

## Project Overview
This repository documents my journey learning Linux fundamentals, CLI tools, processes, services, networking, firewalls, and Git.

## Repository Structure
├── README.md              # You are here
├── ARCHITECTURE.md        # System design documentation
├── day01/                 # Linux Foundations
├── day02/                 # CLI and Filesystem
├── day04/                 # Processes, Services, Logs
├── day08/                 # Routing, NAT, Firewall
├── day10/                 # Git and Documentation
└── runbooks/              # Reusable operation guides

## Learning Progress
| Day | Topic | Status |
|-----|-------|--------|
| 01 | Linux Foundations | ✅ Complete |
| 02 | CLI and Filesystem | ✅ Complete |
| 04 | Processes, Services, Logs | ✅ Complete |
| 08 | Routing, NAT, Firewall | ✅ Complete |
| 10 | Git and Documentation | ✅ Complete |

## Key Skills Demonstrated
- Linux command-line proficiency
- System troubleshooting methodology
- Firewall configuration (UFW)
- NAT and routing concepts
- Git workflow and documentation
ARCHITECTURE.md (System Design Documentation)
Create this file in your repository root:

markdown
# Architecture Documentation

## Lab Environment Overview
Windows Host → WSL2/VirtualBox → Ubuntu 24.04 LTS VM

## Network Topology (From Day 8 Lab)
Internet → Home Router (NAT) → Windows Host → VirtualBox NAT → Ubuntu VM

## Firewall Rules (From Day 8 Lab)
- Default: deny incoming, allow outgoing
- Allowed ports: 22/tcp (SSH), 80/tcp (HTTP), 443/tcp (HTTPS)

## Git Workflow Used
Working Directory → git add → Staging Area → git commit → Local Repo → git push → Remote (GitHub)
Runbook: UFW Firewall Setup
Create runbooks/ufw-setup.md:

markdown
# Runbook: UFW Firewall Setup

## Objective
Configure UFW firewall on Ubuntu with default deny incoming, default allow outgoing.

## Assumptions
- Ubuntu 24.04 LTS
- sudo privileges
- SSH access (port 22)

## Steps
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw enable

## Validation
sudo ufw status verbose
# Should show: Status: active, Default: deny (incoming), allow (outgoing)

## Rollback
sudo ufw disable

## Failure Notes
- If SSH disconnects → forgot to allow 22/tcp before enable
- If website down → forgot to allow 80/tcp
Runbook: Daily Git Workflow
Create runbooks/git-workflow.md:

markdown
# Runbook: Daily Git Workflow

## Objective
Stage, commit, and push changes to GitHub.

## Assumptions
- Git installed
- Repository cloned locally
- GitHub remote configured

## Steps
git add .
git commit -m "Description of changes"
git push origin main

## Validation
git status  # Should show: nothing to commit, working tree clean

## Rollback
git reset --soft HEAD~1  # Undo last commit, keep changes

## Failure Notes
- If push rejected → run git pull origin main first
Quiz
Question 1: Why write small meaningful commits?
My Answer:

Small commits make it easy to track changes and find when a bug was introduced

Meaningful messages tell others (and future you) what changed without looking at the code

Makes it easy to revert a specific change if something breaks

Shows clear progress and disciplined work habits

Question 2: What should a runbook include?
My Answer (from 7.11.4):

Section	What It Contains
Objective	What does this runbook accomplish?
Assumptions	Prerequisites needed before starting
Steps	Exact commands in order
Validation	How to confirm it worked
Rollback	How to undo changes
Failure notes	Common problems and solutions
Question 3: Why architecture diagrams matter?
My Answer:

Visualize complex setups that are hard to explain in words

Help others understand your infrastructure quickly

Show planning and design skills to employers

Serve as documentation for future troubleshooting

Make it easier to spot potential issues before they happen

Question 4: Difference between git fetch and git pull?
My Answer:

Command	What It Does
git fetch	Downloads changes from remote but does NOT merge them into your working files
git pull	Downloads changes AND automatically merges them into your current branch
Simple rule: pull = fetch + merge

Question 5: Why public GitHub portfolio helps job search?
My Answer:

Employers can see your actual work, not just hear about it

Shows discipline (regular commits = consistent work ethic)

Demonstrates technical skills in real projects

Proves you can document and organize your work

Sets you apart from candidates with no public code

Your commit history shows learning progress over time

Interview Questions
Question 1: Why write small meaningful commits?
My Answer:

Small commits make it easy to track changes and find when a bug was introduced

Meaningful messages tell others (and future you) what changed without looking at the code

Makes it easy to revert a specific change if something breaks

Shows clear progress and disciplined work habits

From curriculum: "Git is not only source control; it is proof of operational thinking"

Question 2: What should a runbook include?
My Answer (From 7.11.4):

A runbook must include these 6 sections:

Section	Purpose
Objective	What does this runbook accomplish?
Assumptions	Prerequisites needed before starting
Steps	Exact commands in order
Validation	How to confirm it worked
Rollback	How to undo changes if something fails
Failure notes	Common problems and their solutions
Question 3: Why architecture diagrams matter?
My Answer:

Visualize complex setups that are hard to explain in words

Help others understand your infrastructure quickly

Show planning and design skills to employers

Serve as documentation for future troubleshooting

Make it easier to spot potential issues before they happen

Question 4: Difference between git fetch and git pull?
My Answer:

Command	What It Does
git fetch	Downloads changes from remote but does NOT merge them into your working files
git pull	Downloads changes AND automatically merges them into your current branch
Simple rule: git pull = git fetch + git merge

Question 5: Why public GitHub portfolio helps job search?
My Answer:

Employers can see your actual work, not just hear about it

Shows discipline (regular commits = consistent work ethic)

Demonstrates technical skills in real projects

Proves you can document and organize your work

Sets you apart from candidates with no public code

Your commit history shows learning progress over time

Question 6: What are good repo habits according to the curriculum?
My Answer (From 7.11.2):

The curriculum lists these good repo habits:

Habit	Meaning
Small commits	One logical change per commit
Meaningful messages	Clear descriptions of what changed
Clear README	Front page explaining the project
Reproducible runbooks	Steps others can follow
Question 7: What 6 sections must every runbook include?
My Answer (From 7.11.4):

Objective

Assumptions

Steps

Validation

Rollback

Failure notes

Question 8: What are the 5 Git commands you use daily?
My Answer (From 7.11.3):

#	Command	Purpose
1	git init	Create a new repository
2	git add .	Stage all changes for commit
3	git commit -m "message"	Save changes with description
4	git remote add origin <url>	Connect to GitHub
5	git push -u origin main	Upload changes to GitHub
Lessons Learned
Small meaningful commits are proof of operational thinking, not just source control

Runbooks must have 6 sections: Objective, Assumptions, Steps, Validation, Rollback, Failure notes

Architecture diagrams help visualize complex setups and show planning skills to employers

git fetch downloads without merging; git pull downloads AND merges (fetch + merge)

Public GitHub portfolio is critical for job search - employers can see actual work and commit history

README.md is your front page - it should explain the project, structure, and how to use it

Good repo habits: small commits, meaningful messages, clear README, reproducible runbooks
