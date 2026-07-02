# DevOps Interview Practice Notes

## Question 1 -- Linux

**Question:** Your production server suddenly becomes very slow. How
would you troubleshoot it?

### Strong Answer

1.  Check overall system usage:

``` bash
top
# or
htop
```

-   CPU usage
-   Memory usage
-   Swap usage
-   High resource-consuming processes

2.  Check memory:

``` bash
free -h
```

3.  Check disk space:

``` bash
df -h
```

4.  Find large directories:

``` bash
du -sh /*
```

5.  Find high CPU processes:

``` bash
ps aux --sort=-%cpu
```

6.  Find high memory processes:

``` bash
ps aux --sort=-%mem
```

7.  Check application/system logs and then fix the root cause (restart
    service, free disk, fix memory leak, or scale the application).

------------------------------------------------------------------------

## Question 2 -- Docker

**Question:** What is the difference between a Docker Image and a Docker
Container?

### Strong Answer

A Docker image is a read-only package containing the application code,
runtime, libraries, dependencies, and configuration.

A Docker container is a running instance of an image.

When you run:

``` bash
docker run -itd myapp:v1
```

Docker:

1.  Checks whether the image exists locally.
2.  Pulls it if required.
3.  Creates a writable container layer.
4.  Starts the application's main process.

One image can create multiple containers.

### Build vs Run

Build an image:

``` bash
docker build -t myapp:v1 .
```

Run a container:

``` bash
docker run -itd myapp:v1
```

### Follow-up

If you execute:

``` bash
docker run -d nginx
docker run -d nginx
docker run -d nginx
```

Answer:

-   Images: **1**
-   Containers: **3**

------------------------------------------------------------------------

## Question 3 -- Git

**Question:** How do you bring the latest changes from `main` into your
`feature/login` branch?

### Using Merge

``` bash
git fetch origin
git checkout feature/login
git merge origin/main
```

### Using Rebase

``` bash
git fetch origin
git checkout feature/login
git rebase origin/main
```

If conflicts occur:

``` bash
git rebase --continue
```

### Merge vs Rebase

-   **Merge:** Preserves branch history and creates a merge commit.
-   **Rebase:** Creates a clean, linear commit history by replaying
    commits on top of the latest `main`.

A good interview answer is that you follow your team's Git workflow and
use rebase for a cleaner history when appropriate.

------------------------------------------------------------------------

## Interview Tips

-   Explain **why** you use each command, not just the command.
-   Follow the sequence: **Observe → Identify → Verify → Fix**
-   Think aloud during interviews so the interviewer understands your
    troubleshooting process.
