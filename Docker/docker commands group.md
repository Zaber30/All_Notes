1. docker build (The Blueprint)
You use this command when you have written a `Dockerfile` and want to turn it into a reusable template called an **image**.
2. docker run(the launch)
You use this command when you want to take an image and launch it as a fresh, brand-new process called a container.
3. docker exec (The Inspection)
You use this command when a container is already running and you want to jump inside it to look around, run a quick test, or fix a bug.

4. docker pull / docker push (The Post Office)

These commands are used to download or share Docker images via the internet (usually using Docker Hub).
5. docker stop / docker start (The Pause & Resume)

These commands manage containers that **already exist** on your computer so you do not have to keep creating new ones.
3. docker ps / docker system prune (The Housekeeping)

These commands help you see what is running and clean up wasted disk space. 

The absolute **main commands** of Docker are grouped into ==**13 high-level Management Commands** (the nouns) and **10 Everyday Action Commands** (the verbs)==.

While Docker has hundreds of commands, you only need to know these core commands to do 99% of your work.

---

📁 1. The 13 Core Management Commands (The Objects)

In modern Docker, everything is treated as an object. These are the main categories used to organize all Docker features: 
1. **`docker container`**: Manages the lifecycle of containers (run, stop, start, delete).
2. **`docker image`**: Manages your build templates and downloads.
3. **`docker network`**: Manages how containers talk to each other and the outside world.
4. **`docker volume`**: Manages persistent storage (keeps your data from disappearing).
5. **`docker compose`**: Manages multi-container applications using a single configuration file.
6. **`docker system`**: Manages engine-wide resources, disk space, and cleanup.
7. **`docker context`**: Manages different Docker environments (e.g., switching between local and cloud).
8. **`docker buildx`**: The modern tool used to build advanced images (like building for Intel and Apple M-chips at once).
9. **`docker manifest`**: Manages image multi-architecture information.
10. **`docker secret`**: Manages sensitive data like passwords (used mostly in Swarm clusters).
11. **`docker config`**: Manages application configuration files without rebuilding images.
12. **`docker plugin`**: Manages external add-ons and extensions for Docker.
13. **`docker swarm / service / node`**: Manages clusters of multiple servers running Docker together.

---

🏃 2. The 10 Everyday Action Commands (The Verbs)

Instead of typing the full management commands (like `docker container run`), developers use these short, direct commands daily: 

- **`docker build`**: Turns a `Dockerfile` into a reusable image.
- **`docker run`**: Creates and starts a brand-new container from an image.
- **`docker exec`**: Jumps inside a container that is already running.
- **`docker ps`**: Lists all running containers on your machine.
- **`docker images`**: Lists all images downloaded or built on your machine.
- **`docker stop`**: Safely turns off a running container.
- **`docker start`**: Wakes up a container that you previously stopped.
- **`docker rm`**: Deletes a container completely.
- **`docker rmi`**: Deletes an image to save disk space.
- **`docker logs`**: Shows you the print statements and errors coming from your container.

---

🎯 Summary: How They Work Together

text

```
               [ Dockerfile ]
                     │
                     ▼  (docker build)
                 [ Image ] ◄────── (docker pull / push) ──────► [ Docker Hub ]
                     │
                     ▼  (docker run)
              [ Container ] ◄───── (docker exec / stop / start / logs / ps)
```


## Complete Docker CLI Flag Reference Plan

I'll cover **every flag** command by command:

1. **Part 1:** `docker run` (100+ flags)
2. **Part 2:** `docker build`
3. **Part 3:** `docker exec`
4. **Part 4:** `docker ps`, `docker container`
5. **Part 5:** `docker image`
6. **Part 6:** `docker network`
7. **Part 7:** `docker volume`
8. **Part 8:** `docker compose`
9. **Part 9:** `docker swarm`
10. **Part 10:** Remaining Docker commands (`system`, `context`, `plugin`, `secret`, `config`, `manifest`, `buildx`, etc.)