Linux file permissions determine **who can read, modify, or execute a file or directory**. Every file and directory has three permission sets:

- **Owner (User)** – the user who owns the file.
- **Group** – users who belong to the file's group.
- **Others** – everyone else.


-rwxr-xr--  
│││ │││ │││  
│││ │││ └┴┴── Others  
│││ └┴┴────── Group  
└┴┴────────── Owner

The first character indicates the file type:

| Symbol | Meaning           |
| ------ | ----------------- |
| `-`    | Regular file      |
| `d`    | Directory         |
| `l`    | Symbolic link     |
| `c`    | Character device  |
| `b`    | Block device      |
| `s`    | Socket            |
| `p`    | Named pipe (FIFO) |
# Common Permission Values

|Numeric|Symbolic|Typical Use|
|---|---|---|
|`777`|`rwxrwxrwx`|Everyone has full access (generally not recommended).|
|`755`|`rwxr-xr-x`|Executable programs and directories.|
|`750`|`rwxr-x---`|Private directories shared with a group.|
|`700`|`rwx------`|Private scripts or directories.|
|`644`|`rw-r--r--`|Regular text and configuration files.|
|`600`|`rw-------`|Sensitive files such as private keys or credentials.|

Understanding these permission bits is essential for administering Linux systems securely and for working with tools like Git, Docker, web servers, and application deployments.
```text
How Linux Checks Permissions

                 User Requests Access
                         │
                         ▼
              Are you the file owner?
                  │             │
                Yes            No
                 │              │
     Use owner's permissions    ▼
                     Are you in the file's group?
                         │              │
                       Yes             No
                        │               │
           Use group's permissions     ▼
                     Use others' permissions

Linux stops at the first matching category.

Example:
- If you are the file owner → Linux uses the Owner permissions.
- It does NOT check the Group or Others permissions.
- If you are not the owner but belong to the file's group → Linux uses the Group permissions.
- Otherwise → Linux uses the Others permissions.
```

