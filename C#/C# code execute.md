1. The Standard Workflow (JIT Compilation)

This is the default way .NET works. It translates code in **two stages**: first to a universal language (IL) when building, and then to machine code when running.

text

```
[ STAGE 1: BUILD TIME (On Your Developer Computer) ]
 
 ┌────────────────────────┐
 │   Your C# Source Code  │  <-- Human-readable text (.cs files)
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │    Roslyn Compiler     │  <-- Checks for errors and processes code
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │ Intermediate Language  │  <-- Universal, CPU-independent code
 │       (IL Code)        │      Stored in your bin/ folder (.dll)
 └────────────────────────┘
 
========================= DISTRIBUTE TO USERS =========================

[ STAGE 2: RUNTIME (On the User's Computer) ]

 ┌────────────────────────┐
 │  User Opens the App    │  <-- Triggers the .NET Runtime (CLR)
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │  JIT Compiler (CLR)    │  <-- Translates universal IL on-the-fly
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │      Machine Code      │  <-- Raw 0s and 1s customized for the 
 │   (Intel/AMD/ARM)      │      user's specific CPU chip
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │      Physical CPU      │  <-- Executes the code instantly!
 └────────────────────────┘
```

Use code with caution.

---

2. The Alternative Workflow (Native AOT)

This is the shortcut method. It skips the universal IL language completely and builds **straight to machine code** right on your development computer.

text

```
[ BUILD TIME ONLY ]

 ┌────────────────────────┐
 │   Your C# Source Code  │  <-- Human-readable text (.cs files)
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │   Native AOT Compiler  │  <-- Directly translates C# to machine code
 └───────────┬────────────┘
             │
             ▼
 ┌────────────────────────┐
 │  Native Machine Code   │  <-- A single, ready-to-run file (.exe)
 │   (0s and 1s Output)   │      No JIT compiler needed by the user.
 └────────────────────────┘
 
========================= DISTRIBUTE TO USERS =========================

[ RUNTIME ]

 ┌────────────────────────┐
 │ User Runs Executable   │ ➔ ➔ ➔ Goes straight to the Physical CPU!
 └────────────────────────┘       Starts up instantly.
```

Use code with caution.

Quick Summary of Key Terms

- **Roslyn:** The C# translator.
- **IL (Intermediate Language):** The universal middle-ground code.
- **CLR (Common Language Runtime):** The virtual engine that manages the running app.
- **JIT (Just-In-Time):** The converter that makes the final 0s and 1s as the app runs.