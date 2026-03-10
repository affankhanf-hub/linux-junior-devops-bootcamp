10 points summary

[Working Directory] → [Staging Area] → [Local Repository] → [Remote Repository (GitHub)]
        |                    |                |                       |
    You edit files      git add .       git commit            git push

Step	Command	What Happens
1. Modify	(edit files)	Change your files
2. Stage	git add	Tell Git which changes to save
3. Commit	git commit	Save changes locally with a message
4. Push	git push	Upload to GitHub
Commits

A commit is a snapshot of your files at a point in time.

Commit 1: "Initial setup"
Commit 2: "Add day1 lab"
Commit 3: "Fix typo in README"
Commit 4: "Day 2 completed"

Branches

A branch is a separate line of development.

main:     A---B---C---D
              \
feature:       E---F

PR Mindset (Pull Request)

A Pull Request (PR) is how you propose changes in a team:

    Create a branch

    Make changes

    Open a PR

    Team reviews

    Merge to main

Markdown Runbooks

Markdown is a simple way to format text. Files end in .md.

Practical lab

# Day 10 - Git and Professional Documentation

## Date: March 10, 2026

## What I Learned Today
- Git workflow: add → commit → push
- Importance of small, meaningful commits
- Branching and PR mindset
- Markdown syntax for documentation
- How to structure a professional repo

## Commands Used
- `git init` - Initialize repository
- `git add .` - Stage all changes
- `git commit -m "message"` - Commit with message
- `git remote add origin <url>` - Connect to GitHub
- `git push -u origin main` - Push to github

Quiz

Question 1: Why write small meaningful commits?

Answer:

    Small commits make it easy to track changes and find when a bug was introduced

    Meaningful messages tell others (and future you) what changed without looking at the code

    Makes it easy to revert a specific change if something breaks

    Shows clear progress and disciplined work habits

Question 2: What should a runbook include?

Answer (from 7.11.4):
Section	What It Contains
Objective	What does this runbook accomplish?
Assumptions	Prerequisites needed before starting
Steps	Exact commands in order
Validation	How to confirm it worked
Rollback	How to undo changes
Failure notes	Common problems and solutions
Question 3: Why architecture diagrams matter?

Answer:

    Visualize complex setups that are hard to explain in words

    Help others understand your infrastructure quickly

    Show planning and design skills to employers

    Serve as documentation for future troubleshooting

    Make it easier to spot potential issues before they happen

Question 4: Difference between git fetch and git pull?

Answer:
Command	What It Does
git fetch	Downloads changes from remote but does NOT merge them into your working files
git pull	Downloads changes AND automatically merges them into your current branch

Simple rule: pull = fetch + merge
Question 5: Why public GitHub portfolio helps job search?

Answer:

    Employers can see your actual work, not just hear about it

    Shows discipline (regular commits = consistent work ethic)

    Demonstrates technical skills in real projects

    Proves you can document and organize your work

    Sets you apart from candidates with no public code

    Your commit history shows learning progress over time



Interview 

DAY 10 - Interview Questions Answered
Strictly From Curriculum (Sections 4.2.9 and 7.11)
Question 1: Why write small meaningful commits?

Answer:

    Small commits make it easy to track changes and find when a bug was introduced

    Meaningful messages tell others (and future you) what changed without looking at the code

    Makes it easy to revert a specific change if something breaks

    Shows clear progress and disciplined work habits

    From curriculum: "Git is not only source control; it is proof of operational thinking"

Question 2: What should a runbook include?

Answer (From 7.11.4):

A runbook must include these 6 sections:
Section	Purpose
Objective	What does this runbook accomplish?
Assumptions	Prerequisites needed before starting
Steps	Exact commands in order
Validation	How to confirm it worked
Rollback	How to undo changes if something fails
Failure notes	Common problems and their solutions
Question 3: Why architecture diagrams matter?

Answer:

    Visualize complex setups that are hard to explain in words

    Help others understand your infrastructure quickly

    Show planning and design skills to employers

    Serve as documentation for future troubleshooting

    Make it easier to spot potential issues before they happen

Question 4: Difference between git fetch and git pull?

Answer:
Command	What It Does
git fetch	Downloads changes from remote but does NOT merge them into your working files
git pull	Downloads changes AND automatically merges them into your current branch

Simple rule: git pull = git fetch + git merge
Question 5: Why public GitHub portfolio helps job search?

Answer:

    Employers can see your actual work, not just hear about it

    Shows discipline (regular commits = consistent work ethic)

    Demonstrates technical skills in real projects

    Proves you can document and organize your work

    Sets you apart from candidates with no public code

    Your commit history shows learning progress over time

Question 6: What are good repo habits according to the curriculum?

Answer (From 7.11.2):

The curriculum lists these good repo habits:
Habit	Meaning
Small commits	One logical change per commit
Meaningful messages	Clear descriptions of what changed
Clear README	Front page explaining the project
Reproducible runbooks	Steps others can follow
Question 7: What 6 sections must every runbook include?

Answer (From 7.11.4):

    Objective

    Assumptions

    Steps

    Validation

    Rollback

    Failure notes

Question 8: What are the 5 Git commands you use daily?

Answer (From 7.11.3):
#	Command	Purpose
1	git init	Create a new repository
2	git add .	Stage all changes for commit
3	git commit -m "message"	Save changes with description
4	git remote add origin <url>	Connect to GitHub
5	git push -u origin main	Upload changes to GitHub


