A **garbage collector (GC)** is an automatic memory management tool built into many programming languages that frees up computer memory (**RAM**) by destroying data the program no longer needs.

How it Works

A garbage collector constantly monitors a program's memory by following a simple rule: **If the program can no longer reach a piece of data, that data is garbage.**

The most common method it uses is called **Mark and Sweep**:

```
[ Step 1: Mark ]             [ Step 2: Sweep ]
Find all objects still       Delete any unmarked objects
connected to the program.    and free up their space.
  (Live Object)  🟢            (Dead Object)  🔴 ❌
  (Live Object)  🟢            (Dead Object)  🔴 ❌
```

1. **Mark:** The GC starts from the core of the running program (the "roots") and follows every link to see what data is currently in use. It "marks" these objects as alive.
2. **Sweep:** It scans the rest of the memory. Any data that was not marked is identified as unreachable, deleted, and its space is reclaimed for future use.

