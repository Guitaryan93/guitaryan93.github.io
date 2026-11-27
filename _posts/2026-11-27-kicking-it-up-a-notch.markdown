---
layout: post
title: "Kicking it up a notch"
categories: python programming
---
Still buzzing with enthusiasm from all the things I learnt from my [first project](/python/programming/2026/11/25/day-one.html) I jumped head first into the next task! This time ChatGPT decided on a persistent task manager application. We will be saving and loading data from JSON files this time around! Very exciting!

Requirement 1 — Add a task
The user must be able to create a new task.

Each task must contain:
Field               Description Example
-----               -------------------
ID                  Unique task identifier  1
Title               Short task description  Fix printer
Description         Additional details  Investigate paper feed issue
Created Date/time   task was created  2026-11-20 14:30
Due date            Optional deadline   2026-11-25
Completed           Whether task is finished    False


Requirement 2 — View all tasks
Display all tasks in a readable format.

Example:
==============================
TASK LIST
==============================

ID    TITLE                STATUS

1     Fix printer          OPEN
2     Update documentation COMPLETE
3     Review Python code   OPEN

==============================
Total tasks: 3
Completed:  1
Outstanding: 2


Requirement 3 — View an individual task
The user should be able to enter a task ID and see its complete details.

For example:
TASK DETAILS
------------------------------

ID:          1
Title:       Fix printer
Description: Investigate paper feed issue
Created:     2026-11-20 14:30
Due:         2026-11-25
Status:      OPEN


Requirement 4 — Complete a task
The user should be able to mark a task as completed using its ID.

For example:
Enter task ID: 1

Task 'Fix printer' marked as completed!


Requirement 5 — Delete a task
The user should be able to delete a task using its ID.

I want you to consider confirmation before deleting.

For example:
Enter task ID: 1

Are you sure you want to delete 'Fix printer'? (Y/N):


Requirement 6 — Save and load tasks
This is the most important new requirement.

Your application must:
- Load existing tasks when it starts.
- Save tasks whenever they are created, completed, or deleted.
- Create the JSON file if it doesn't already exist.
- Handle missing or invalid JSON files gracefully.
We'll use a file called:
tasks.json


3. Menu requirements
Your menu should contain these options:
================================
TASK MANAGEMENT SYSTEM
================================

1. Add task
2. View all tasks
3. View task details
4. Complete task
5. Delete task
6. Exit

Select an option:


PROJECT STRUCTURE
project_002/
│
├── main.py
├── tasks.py
├── storage.py
├── tasks.json
│
└── tests/
    ├── __init__.py
    └── test_tasks.py

What are these files for?
File                Responsibility
----                --------------
main.py             Application startup and menu
tasks.py            Task operations and business logic
storage.py          Loading and saving JSON
tasks.json          Persistent task data
tests/              Automated tests


6. Rules and constraints
These are the ground rules for this project.
1. No external Python libraries. Use the standard library.
2. No databases. JSON files only.
3. No graphical interface. Command-line application.
4. No global task collections. Pass data between functions.
5. Don't use eval() or exec().
6. Handle invalid user input.
7. Don't silently discard or overwrite existing tasks.
8. Keep functions focused on individual responsibilities.

