## Process

**Definition:**  
A **process is a program that is currently running on a computer.** When you open an application, the operating system creates a process for that application. A process has its own memory, resources, and execution environment, so it works independently from other processes.

**Easy explanation:**  
A process is like a **separate workspace** where a program runs. For example, when you open a browser, music player, or code editor, each one runs as a different process.

**Example:**

- Opening Chrome → Chrome becomes a process
- Opening VS Code → VS Code becomes a process
- Running a Python program → Python program becomes a process

---

## Thread

**Definition:**  
A **thread is a small unit of execution inside a process that performs a specific task.** A single process can have multiple threads, and these threads share the same memory and resources of the process.

**Easy explanation:**  
A thread is like a **worker inside a workspace**. The process is the main program, and threads are the workers that perform different tasks at the same time.

**Example:**  
A browser process may have:

- One thread to display the user interface
- One thread to load web pages
- One thread to download files

---

## Simple Relationship

```
Process = Application/Program running        Process           |   ----------------   |       |       |Thread  Thread  Thread
```

Example:

```
Chrome (Process)      |      ├── Display page (Thread)      ├── Download file (Thread)      └── Play video (Thread)
```

**In short:**

- **Process → A running program**
- **Thread → A task/execution path inside a running program**