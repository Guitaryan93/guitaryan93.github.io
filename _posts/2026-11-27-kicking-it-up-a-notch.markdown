---
layout: post
title: "Kicking it up a notch"
categories: python programming
---
Still buzzing with enthusiasm from all the things I learnt from my [first project](/python/programming/2026/11/25/day-one.html) I jumped head first into the next task! This time ChatGPT decided on a persistent task manager application. We will be saving and loading data from JSON files this time around! Very exciting!

# Requirements
Requirement 1 — Add a task
The user must be able to create a new task.

Each task must contain:
```
Field               Description Example
-----               -------------------
ID                  Unique task identifier  1
Title               Short task description  Fix printer
Description         Additional details  Investigate paper feed issue
Created Date/time   task was created  2026-11-20 14:30
Due date            Optional deadline   2026-11-25
Completed           Whether task is finished    False
```

Requirement 2 — View all tasks
Display all tasks in a readable format.

Example:
```
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
```

Requirement 3 — View an individual task
The user should be able to enter a task ID and see its complete details.

For example:
```
TASK DETAILS
------------------------------

ID:          1
Title:       Fix printer
Description: Investigate paper feed issue
Created:     2026-11-20 14:30
Due:         2026-11-25
Status:      OPEN
```

Requirement 4 — Complete a task
The user should be able to mark a task as completed using its ID.

For example:
```
Enter task ID: 1

Task 'Fix printer' marked as completed!
```

Requirement 5 — Delete a task
The user should be able to delete a task using its ID.

I want you to consider confirmation before deleting.

For example:
```
Enter task ID: 1

Are you sure you want to delete 'Fix printer'? (Y/N):
```

Requirement 6 — Save and load tasks
This is the most important new requirement.

Your application must:
- Load existing tasks when it starts.
- Save tasks whenever they are created, completed, or deleted.
- Create the JSON file if it doesn't already exist.
- Handle missing or invalid JSON files gracefully. We'll use a file called: `tasks.json`

Menu requirements
Your menu should contain these options:
```
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
```

PROJECT STRUCTURE
```
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
```
What are these files for?
```
File                Responsibility
----                --------------
main.py             Application startup and menu
tasks.py            Task operations and business logic
storage.py          Loading and saving JSON
tasks.json          Persistent task data
tests/              Automated tests
```

Rules and constraints - These are the ground rules for this project.
1. No external Python libraries. Use the standard library.
2. No databases. JSON files only.
3. No graphical interface. Command-line application.
4. No global task collections. Pass data between functions.
5. Don't use `eval()` or `exec()`.
6. Handle invalid user input.
7. Don't silently discard or overwrite existing tasks.
8. Keep functions focused on individual responsibilities.

# Modular Code
So, to start out this project I realized that a lot of the code from the previous project could be reused. That included the display.py module for printing to the screen:

```python
# Contains all print functions for terminal UI

def header(header):
    print("================================")
    print(header.upper())
    print("================================")


def subheader(header):
    print(header.upper())
    single_hr()
    print()


def options(options):
    ''' print numbered list of options from a python list '''
    print()
    for i, opt in enumerate(options, start=1):
        print(f"{i}.  {opt['name']}")
    print()


def single_hr():
    print("------------------------------")


def double_hr():
    print("==============================")


def task_contents(task):
    print(f"ID         : {task['id']}")
    print(f"Title      : {task['title']}")
    print(f"Description: {task['description']}")
    print(f"Created    : {task['date_created']}")
    print(f"Due        : {task['date_due']}")
    print(f"Status     : {'COMPLETE' if task['completed'] else 'OPEN'}")
    print()


def status(status):
    ''' display status text '''
    print(f"\n{status}\n")
```

And the menu system where each function is stored in a dictionary and we select it by taking user input and validating it before using it to access the dictionary we're looking for:

```python
def main():
    ''' main program code goes in here '''
    run_program = True

    # add user options to dictonary inside a list that holds
    # the matching function
    options = [
        {"name": "Add task", "func": tasks.add_task},
        {"name": "View all tasks", "func": tasks.view_all_tasks},
        {"name": "View task details", "func": tasks.view_task},
        {"name": "Complete task", "func": tasks.complete_task},
        {"name": "Delete task", "func": tasks.delete_task},
        {"name": "Exit", "func": exit_program},
    ]

. . . .

    display.header("TASK MANAGEMENT SYSTEM")

    while run_program:
        display.options(options)

        user_choice = input("Select an option: ")
        print()

        try:
            selected_menu_item = int(user_choice)
            if selected_menu_item <= 0 or selected_menu_item > len(options):
                raise ValueError()
        except ValueError:
            display.status(f"'{user_choice}' is not a valid option...")
        else:
            status = options[selected_menu_item - 1]["func"](task_list)
            if not status is None:
                display.status(status)
```


# Return Statements
What should a function return? That is a great question! Initially, I was printing my error messages inside my functions and returning False, or a blank string, or 0, or whatever. But a voice in my head kept nagging at me: "*psssst*... not here... never heeeeerrrreeee *shhhhh*...". That voice didn't have the answers either though, so...

ChatGPT however did (unsurprisingly) have a suggestion! Returning a status or a string that could be used to inform the user or the rest of the program whether the function succeeded or failed, and why/how it failed. What a great idea! Here's an example. This function to add a task can fail in many ways thanks to the wonders of user input and human ingenuity. So returning a string that gives some feedback is probably most helpful.

```python
def add_task(task_list, file_path=storage.DEFAULT_FILE_PATH):
    ''' get user input for task data and create task '''
    display.subheader("TASK CREATE")

    title = input("TASK TITLE           : ")

    # validate title
    if title.strip() == "":
        return "Cannot create a task without a title"

    description = input("TASK DESCRIPTION     : ")
    date_due    = input("DUE DATE (YYYY-MM-DD): ").strip()

    # validate the due date (blank is also fine!)
    if date_due != "" and valid_future_date(date_due) == False:
        return f"'{date_due}' is not a valid date...\nDue date must be a future date in iso format YYYY-MM-DD or blank."

    # create the task record
    task = {"id": get_uid(task_list),
            "title": title.strip(),
            "description": description.strip(),
            "date_created": get_timestamp(),
            "date_due": date_due,
            "completed": False,
           }

    task_list.append(task)

    # save the new list of tasks
    storage.save_tasks(task_list)

    return f"Task successfully created!\nAssigned Task ID: {task['id']}"
```
Here's how the program flow works:
- Display the menu options to the user
- Wait for user to select an option
- Run the selected function
- Return to the menu, printing any status message that the function returned
- Repeat until the user chooses to exit

So any/all functions can return a string if they want and the user will see it after the function has returned. So simple, but so effective.

# Working with files
There are many considerations when it comes to working with files.
- Does the file contain the data the program expects?
- What should the program do if the file doesn't exist?
- What should the program do if the file being loaded is corrupted?
- How should the program save the data to the file?

# Automated Testing




