# Docker Lab: CMD vs ENTRYPOINT

**Goal:** By the end of this, you should be able to explain — and prove with a running container — the difference between `CMD` and `ENTRYPOINT`, when to use each, and how runtime overrides work.

Do each task in order. Don't skip the "predict first" steps — that's where the actual learning happens.

---

## Setup

Create a working folder:
```bash
mkdir docker-lab && cd docker-lab
```

You'll create several small Dockerfiles in this lab. Name them as instructed (`Dockerfile.1`, `Dockerfile.2`, etc.) so you can compare them side by side later.

---

## Task 1 — CMD only

Create `Dockerfile.1`:
```dockerfile
FROM alpine:latest
CMD ["echo", "Hello from CMD"]
```

Build and run:
```bash
docker build -t lab:cmd -f Dockerfile.1 .
docker run lab:cmd
```

**Now override it:**
```bash
docker run lab:cmd echo "Overridden!"
```

**Question to answer in your notes:** What happened to the original `CMD` when you passed an argument on the command line?

---

## Task 2 — ENTRYPOINT only

Create `Dockerfile.2`:
```dockerfile
FROM alpine:latest
ENTRYPOINT ["echo", "Hello from ENTRYPOINT"]
```

Build and run:
```bash
docker build -t lab:entry -f Dockerfile.2 .
docker run lab:entry
```

**Now try to "override" it the same way as before:**
```bash
docker run lab:entry echo "Overridden?"
```

**Predict before running:** Do you think this will replace the entrypoint, like it did with CMD? Run it and see. Write down what actually happened — most people guess wrong the first time.

**Now override it properly using the flag made for this:**
```bash
docker run --entrypoint echo lab:entry "Now it's really overridden"
```

---

## Task 3 — CMD + ENTRYPOINT together (the real-world pattern)

Create `Dockerfile.3`:
```dockerfile
FROM alpine:latest
ENTRYPOINT ["echo"]
CMD ["Default message"]
```

Build and run:
```bash
docker build -t lab:combo -f Dockerfile.3 .
docker run lab:combo
```

Now pass a custom argument:
```bash
docker run lab:combo "Custom message"
```

**Question:** Explain in your own words what's happening here. Which part stayed fixed, and which part got replaced?

This pattern — `ENTRYPOINT` as the fixed command, `CMD` as the default argument — is how most production images (like `nginx`, `postgres`, `python`) are actually built. Worth sitting with this one until it clicks.

---

## Task 4 — Shell form vs Exec form

Create `Dockerfile.4`:
```dockerfile
FROM alpine:latest
ENTRYPOINT echo "Shell form entrypoint"
```

Build and run:
```bash
docker build -t lab:shellform -f Dockerfile.4 .
docker run lab:shellform
```

Now inspect the running process:
```bash
docker run lab:shellform &
docker ps
docker exec -it <container_id> ps aux
```

Compare against Task 2's exec-form image (`lab:entry`) using the same `ps aux` check.

**Question:** In the shell-form container, what is PID 1? Why does that matter for signal handling (e.g. `docker stop`, Ctrl+C)?

*(Hint: shell form wraps your command inside `/bin/sh -c "..."`, so your actual process isn't PID 1 — this affects how SIGTERM gets forwarded.)*

---

## Task 5 — Practical scenario: build a "real" entrypoint image

Build an image that behaves like a simple CLI tool:

- `ENTRYPOINT` should always run a script called `greet.sh`
- `CMD` should provide a **default name** to greet, but the user should be able to override just the name at `docker run` time

Steps:
1. Write `greet.sh`:
   ```bash
   #!/bin/sh
   echo "Hello, $1!"
   ```
2. Write a Dockerfile that:
   - Copies `greet.sh` in
   - Makes it executable
   - Sets it as `ENTRYPOINT`
   - Sets a default `CMD` (e.g. `["World"]`)
3. Build it, then test:
   ```bash
   docker run <image>                  # should print "Hello, World!"
   docker run <image> Junior           # should print "Hello, Junior!"
   ```

This is the pattern used by tools like `git`, `kubectl`-wrapped images, migration tools, etc. — fixed binary, flexible arguments.

---

## Task 6 — Break it and fix it (debugging exercise)

Take the image from Task 5 and deliberately try these two mistakes. For each, predict what error you'll get *before* running it, then run it and compare:

1. Change `ENTRYPOINT` to use **shell form** instead of exec form (`ENTRYPOINT greet.sh "$1"` — this won't work as expected). Why does this break the `CMD` argument passing?
2. Forget `RUN chmod +x greet.sh`. What error does Docker give you when it tries to run it?

---

## Wrap-up questions (answer without looking anything up)

1. If both `CMD` and `ENTRYPOINT` are defined, what does the extra text after `docker run <image> ...` actually replace?
2. If only `CMD` is defined, what does that extra text replace?
3. Which one requires a special flag (`--entrypoint`) to override, and why do you think Docker designed it that way?
4. Why does exec form (`["cmd", "arg"]`) matter for signal handling, compared to shell form?
5. Give one real example (from Docker Hub, if you want to go look) of an image that uses the ENTRYPOINT + CMD combo pattern, and explain what each part does for that image.

---

## Done?

Have your junior walk you through Task 3 and Task 5 out loud, explaining what's happening at each step. If they can teach it back clearly, they've got it.
