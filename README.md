# Personal Task Manager

## Student Information

- **Project Code:** WST21-PM-2026-SF
- **Student Name:** Joshua Layos
- **Course & Year:** BSIT - 2nd Year
- **Database Used:** MySQL
- **Local Environment:** XAMPP

## Project Description

Personal Task Manager is a simple Laravel project made to help manage tasks. It allows the user to add tasks, view tasks, edit them, change their status, and delete them.

## Features

- Add Task
- View Task
- Edit Task
- Delete Task
- Change Status
- Add Description
- Add Due Date
- Dashboard Counters

## Technologies Used

- Laravel
- PHP
- MySQL
- Blade
- HTML
- CSS
- XAMPP

## How the System Works

### 1. Open the System

When the system is opened, the dashboard shows the task list and the task counters.

### 2. Add a Task

The user clicks **Add New Task** and enters the task name, description, status, and due date. After clicking the submit button, the task is saved in the database.

### 3. View Tasks

The added task will appear in the task list. The dashboard also shows the number of total, pending, and completed tasks.

### 4. Edit a Task

The user can click **Edit** to change the information of a task. After saving, the changes will appear in the task list.

### 5. Change Status

The user can change the task status from **Pending** to **Completed**.

### 6. Delete a Task

The user can click **Delete** to remove a task from the list.

## Laravel Flow

```text
User
 ↓
Blade
 ↓
Route
 ↓
Controller
 ↓
Model
 ↓
MySQL
 ↓
Blade
 ↓
User
```

## Laravel Code

### 1. Routes

**File:**

`routes/web.php`

**Code:**

```php
<?php

use Illuminate\Support\Facades\Route;
use App\Http\Controllers\TaskController;

Route::resource('tasks', TaskController::class);
```

The route connects the website pages to the `TaskController`.

### 2. Task Model

**File:**

`app/Models/Task.php`

**Code:**

```php
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;

class Task extends Model
{
    protected $fillable = [
        'task_name',
        'description',
        'status',
        'due_date',
    ];
}
```

The model is used to work with the task data in MySQL.

### 3. Task Controller

**File:**

`app/Http/Controllers/TaskController.php`

**Code:**

```php
<?php

namespace App\Http\Controllers;

use App\Models\Task;
use Illuminate\Http\Request;

class TaskController
{
    public function index()
    {
        $tasks = Task::orderByRaw(
            "CASE WHEN status = 'Pending' THEN 0 ELSE 1 END"
        )
        ->orderBy('due_date')
        ->get();

        $totalTasks = Task::count();
        $pendingTasks = Task::where('status', 'Pending')->count();
        $completedTasks = Task::where('status', 'Completed')->count();

        return view('tasks.index', compact(
            'tasks',
            'totalTasks',
            'pendingTasks',
            'completedTasks'
        ));
    }

    public function create()
    {
        return view('tasks.create');
    }

    public function store(Request $request)
    {
        $data = $request->validate([
            'task_name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'status' => 'required|in:Pending,Completed',
            'due_date' => 'required|date',
        ]);

        Task::create($data);

        return redirect()
            ->route('tasks.index')
            ->with('success', 'Task added successfully.');
    }

    public function show(Task $task)
    {
        return view('tasks.show', compact('task'));
    }

    public function edit(Task $task)
    {
        return view('tasks.edit', compact('task'));
    }

    public function update(Request $request, Task $task)
    {
        $data = $request->validate([
            'task_name' => 'required|string|max:255',
            'description' => 'nullable|string',
            'status' => 'required|in:Pending,Completed',
            'due_date' => 'required|date',
        ]);

        $task->update($data);

        return redirect()
            ->route('tasks.index')
            ->with('success', 'Task updated successfully.');
    }

    public function destroy(Task $task)
    {
        $task->delete();

        return redirect()
            ->route('tasks.index')
            ->with('success', 'Task deleted successfully.');
    }
}
```

The controller handles the main actions of the system like adding, viewing, editing, updating, and deleting tasks.

### 4. Database Migration

**File:**

`database/migrations/create_tasks_table.php`

**Code:**

```php
Schema::create('tasks', function (Blueprint $table) {
    $table->id();
    $table->string('task_name');
    $table->text('description')->nullable();
    $table->enum('status', ['Pending', 'Completed'])
          ->default('Pending');
    $table->date('due_date');
    $table->timestamps();
});
```

This code creates the `tasks` table in MySQL.

### 5. Blade View

**Main file:**

`resources/views/tasks/index.blade.php`

**Example:**

```blade
@foreach($tasks as $task)
    {{ $task->task_name }}
@endforeach
```

Blade is used to show the task information on the website.

## CRUD

The project uses CRUD:

| CRUD | What it does |
|---|---|
| Create | Add a task |
| Read | View tasks |
| Update | Edit a task |
| Delete | Delete a task |

## Database

The database used for this project is **MySQL**.

**Database name:**

```text
task_manager
```

**Table:**

```text
tasks
```

The table contains:

```text
id
task_name
description
status
due_date
created_at
updated_at
```

## System Screenshots

### Dashboard

[Dashboard](screenshots/dashboard.jpeg)

### Add New Task

[Add New Task](screenshots/add-task.jpeg)

### Task List

[Task List](screenshots/task-list.jpeg)

### Edit Task

[Edit Task](screenshots/edit-task.jpeg)

### Completed Task

[Completed Task](screenshots/completed-task'.jpeg)

### Delete Task

[Delete Task](screenshots/delete-task.png)

## System Outputs

### Add Task

The task is added and shown in the task list.

### View Task

The saved task is displayed in the task list with its description, status, and due date.

### Edit Task

The task information is updated after editing.

### Completed Task

The task status changes from **Pending** to **Completed**.

### Delete Task

The selected task is removed from the task list.

### Dashboard

The dashboard shows the total, pending, and completed tasks.

## Project Structure

```text
Personal-Task-Manager-Laravel/
│
├── app/
│   ├── Http/
│   │   └── Controllers/
│   │       └── TaskController.php
│   └── Models/
│       └── Task.php
│
├── database/
│   └── migrations/
│       └── create_tasks_table.php
│
├── resources/
│   └── views/
│       └── tasks/
│           ├── index.blade.php
│           ├── create.blade.php
│           ├── edit.blade.php
│           └── show.blade.php
│
├── routes/
│   └── web.php
│
├── composer.json
├── Dockerfile
└── README.md
```

## How to Run the Project

### Step 1: Start MySQL

Make sure the **MySQL267** service is running.

### Step 2: Open Command Prompt

Go to the project folder:

```cmd
cd /d "C:\Users\kings\OneDrive\Desktop\Joshua-Layos-Personal-Task-Manager"
```

### Step 3: Start Laravel

Run:

```cmd
set PATH=C:\xampp\php;%PATH% && php artisan serve
```

### Step 4: Open the Website

Open the browser and go to:

[http://127.0.0.1:8000/tasks](http://127.0.0.1:8000/tasks)

### Step 5: Use the System

After opening the website, the user can add, view, edit, change the status, and delete tasks.

## Laravel Flow

```text
Routes
   ↓
Controller
   ↓
Model
   ↓
MySQL Database
   ↓
Blade
```

- **Routes** connect the pages to the controller.
- **Controller** handles the actions of the system.
- **Model** works with the task data.
- **MySQL** stores the task information.
- **Blade** displays the information on the website.

## Notes

The `vendor` folder and `.env` file are not included in the repository.

The `vendor` folder is generated by Composer, while `.env` contains the local database and application settings.
