the OS gives each process its own **virtual address space**.

**Virtual memory** is a memory-management system that lets a program use **virtual addresses instead of directly accessing physical RAM**.
```
Application
     ↓
Virtual Memory
     ↓
Operating System
     ↓
Physical RAM
```

This provides isolation between processes.

For example:

```
Process A                 Process B
Virtual Memory            Virtual Memory
     ↓                         ↓
     OS manages physical memory
              ↓
            RAM
```
The OS creates a **virtual address space for each process** and maps parts of that space to physical memory when needed.
Why do we need virtual memory?
#### 1. Process isolation 🔐

Without virtual memory, two programs could potentially access the same physical memory.

#### 2. Each program gets the illusion of having its own memory
3. Programs don't need contiguous physical RAM
 4. Programs can use more memory than physical RAM
