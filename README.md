# Coding-Raja-Technologies-Internship
#Task 1
import os
import json
from datetime import datetime

# Global task list
tasks = []

# File to store tasks
task_file = "tasks.json"

# Load tasks from the file if it exists
if os.path.exists(task_file):
    with open(task_file, 'r') as file:
        tasks = json.load(file)

def save_tasks():
    with open(task_file, 'w') as file:
        json.dump(tasks, file)

def display_tasks():
    if not tasks:
        print("No tasks found.")
        return

    print("Task List:")
    for idx, task in enumerate(tasks):
        print(f"{idx + 1}. {task['title']} [Priority: {task['priority']}, Due: {task['due_date']}] - {'Completed' if task['completed'] else 'Not Completed'}")

def add_task():
    title = input("Enter task title: ")
    priority = int(input("Enter priority (1-High, 2-Medium, 3-Low): "))
    due_date = input("Enter due date (YYYY-MM-DD): ")

    try:
        due_date = datetime.strptime(due_date, "%Y-%m-%d").date()
    except ValueError:
        print("Invalid date format. Please use YYYY-MM-DD.")
        return

    tasks.append({
        'title': title,
        'priority': priority,
        'due_date': due_date.strftime("%Y-%m-%d"),
        'completed': False
    })

    print(f"Task '{title}' added successfully.")
    save_tasks()

def remove_task():
    display_tasks()
    task_idx = int(input("Enter the task number to remove: ")) - 1

    if 0 <= task_idx < len(tasks):
        removed_task = tasks.pop(task_idx)
        print(f"Task '{removed_task['title']}' removed successfully.")
        save_tasks()
    else:
        print("Invalid task number. Please try again.")

def mark_completed():
    display_tasks()
    task_idx = int(input("Enter the task number to mark as completed: ")) - 1

    if 0 <= task_idx < len(tasks):
        tasks[task_idx]['completed'] = True
        print(f"Task '{tasks[task_idx]['title']}' marked as completed.")
        save_tasks()
    else:
        print("Invalid task number. Please try again.")

def main():
    while True:
        print("\nTodo List Application")
        print("1. Add Task")
        print("2. Remove Task")
        print("3. Mark Task as Completed")
        print("4. Display Tasks")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == '1':
            add_task()
        elif choice == '2':
            remove_task()
        elif choice == '3':
            mark_completed()
        elif choice == '4':
            display_tasks()
        elif choice == '5':
            save_tasks()
            print("Exiting the application.")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == '__main__':
    main()
