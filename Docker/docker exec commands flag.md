# `docker exec` Command Flags (Complete Reference)

The `docker exec` command runs a new command inside an already running container.

---

## All `docker exec` Flags

|Short Flag|Long Flag|Description|
|---|---|---|
|`-d`|`--detach`|Run the command in the background (detached mode).|
|`-i`|`--interactive`|Keep STDIN open even if not attached.|
|`-t`|`--tty`|Allocate a pseudo-terminal (TTY).|
|`-e`|`--env`|Set an environment variable for the executed command. Can be specified multiple times.|
|`--env-file`||Load environment variables from a file for the executed command.|
|`-u`|`--user`|Run the command as a specific user or UID/GID inside the container.|
|`-w`|`--workdir`|Set the working directory for the executed command.|
|`--detach-keys`||Override the default key sequence used to detach from an interactive session.|
|`--privileged`||Run the command with extended Linux privileges inside the container.|

---

# Detailed Description of Each Flag

## `-d`, `--detach`

Runs the command in the background without attaching your terminal to it.

---

## `-i`, `--interactive`

Keeps the command's standard input (STDIN) open so you can interact with it.

Commonly used for:

- Bash shell
- Database CLI
- Python REPL

---

## `-t`, `--tty`

Allocates a terminal (TTY), allowing interactive programs to display correctly.

Usually combined with `-i`.

---

## `-it`

This is **not a separate flag**.

It is simply a combination of:

- `-i`
- `-t`

This is the most commonly used option with `docker exec`.

---

## `-e`, `--env`

Sets one or more environment variables **only for the command being executed**.

It does **not** permanently change the container's environment.

---

## `--env-file`

Loads environment variables from a file and makes them available only to the executed command.

---

## `-u`, `--user`

Runs the command as a different user inside the container.

You can specify:

- Username
- UID
- UID:GID
- Username:Group

---

## `-w`, `--workdir`

Changes the current working directory before executing the command.

If the directory does not exist, Docker returns an error.

---

## `--detach-keys`

Changes the keyboard shortcut used to detach from an interactive session.

By default:

```
Ctrl + PCtrl + Q
```

This flag allows you to customize that sequence.

---

## `--privileged`

Runs the command with extended Linux capabilities.

This gives the executed process more permissions than a normal container process.

Use it only when necessary because it reduces isolation.

---

# Most Common Flag Combinations

| Flags          | Purpose                                         |
| -------------- | ----------------------------------------------- |
| `-it`          | Open an interactive shell inside the container. |
| `-d`           | Run a command in the background.                |
| `-u`           | Execute as another user.                        |
| `-w`           | Execute from a specific directory.              |
| `-e`           | Provide temporary environment variables.        |
| `--privileged` | Run the command with elevated privileges.       |