# Day 29 – Introduction to Docker

## Task 1: What is Docker?

### What is a container and why do we need them?

A container is a lightweight, standalone package that bundles an application together with everything it needs to run — code, runtime, system libraries, and configuration. It runs as an isolated process on the host OS, using the host's kernel instead of shipping its own.

We need containers because:
- **Consistency** — "it works on my machine" goes away. Dev, staging, and prod run the exact same image.
- **Speed** — containers start in seconds, not minutes like a VM booting a full OS.
- **Density** — many containers can run on one host since they share the kernel, instead of each needing its own OS.
- **Portability** — an image built once runs anywhere Docker (or a compatible runtime) is installed.

From my SRE background, this maps directly to problems I already deal with: OS patching across ~100 servers, environment drift between app servers, and dependency conflicts. Containers push a lot of that pain to build time instead of runtime.

### Containers vs Virtual Machines — the real difference

| | Virtual Machine | Container |
|---|---|---|
| Isolation | Full OS + hypervisor | Process-level isolation (namespaces, cgroups) |
| Boot time | Minutes | Seconds |
| Size | GBs (full OS image) | MBs (just app + deps) |
| Resource overhead | High — each VM runs its own kernel | Low — containers share the host kernel |
| Density per host | Low (few VMs) | High (many containers) |
| Use case | Running different OS types, strong isolation needs | Fast, repeatable app deployment |

The key difference: a VM virtualizes hardware and runs a full guest OS on top of a hypervisor. A container virtualizes at the OS level — it shares the host kernel and is just an isolated process with its own filesystem, network, and process namespace. That's why containers are so much lighter.

### Docker Architecture

Docker follows a client-server model:

- **Docker Client** — the CLI (`docker` command) you type into. Sends commands to the daemon.
- **Docker Daemon (`dockerd`)** — runs on the host, does the actual work: builds images, runs containers, manages networks and volumes.
- **Docker Images** — read-only templates (layered filesystem) used to create containers. Built from a `Dockerfile`.
- **Docker Containers** — running instances of an image, with a writable layer on top.
- **Docker Registry** — where images are stored and pulled from (Docker Hub by default, or a private registry).

**Flow in my own words:**
`docker run nginx` → Client sends the request to the Daemon → Daemon checks if the `nginx` image exists locally → if not, it pulls it from the Registry (Docker Hub) → Daemon uses that image to create and start a Container → Client shows you the result.

```
 ┌──────────┐        ┌───────────────┐        ┌───────────────┐
 │  Docker  │  CLI   │    Docker     │  pull  │    Registry   │
 │  Client  │ ─────► │    Daemon     │ ─────► │  (Docker Hub) │
 └──────────┘        │  (dockerd)    │        └───────────────┘
                      │               │
                      │  ┌─────────┐  │
                      │  │ Images  │──┼──► used to create
                      │  └─────────┘  │
                      │       │       │
                      │       ▼       │
                      │  ┌─────────┐  │
                      │  │Container│  │
                      │  └─────────┘  │
                      └───────────────┘
```

---

## Task 2: Install Docker

**Steps taken:**
1. Installed Docker Engine on `<your machine / cloud instance — e.g. Ubuntu 22.04 EC2>`
2. Verified installation:
   ```bash
   docker --version
   docker info
   ```
3. Ran the hello-world container:
   ```bash
   docker run hello-world
   ```
4. Read the output — it confirms the daemon pulled the image, created a container from it, ran it, and the container printed a message before exiting.

**Screenshot:**
`[Insert screenshot: docker --version and hello-world output]`

---

## Task 3: Run Real Containers

**1. Nginx container, accessed in browser**
```bash
docker run -d -p 8080:80 nginx
```
Visited `http://localhost:8080` → saw the default Nginx welcome page.

`[Insert screenshot: Nginx welcome page in browser]`

**2. Ubuntu container in interactive mode**
```bash
docker run -it ubuntu bash
```
Explored it like a mini VM — ran `ls`, `apt update`, `whoami`, checked `/etc/os-release`. No systemd, minimal packages — just enough to run a shell.

**3. List running containers**
```bash
docker ps
```

**4. List all containers (including stopped)**
```bash
docker ps -a
```

**5. Stop and remove a container**
```bash
docker stop <container_id>
docker rm <container_id>
```

`[Insert screenshot: docker ps / docker ps -a output]`

---

## Task 4: Explore

**1. Detached mode**
```bash
docker run -d nginx
```
Difference from normal `docker run`: the container runs in the background and gives control of the terminal back immediately, instead of attaching to the container's output and blocking the shell.

**2. Custom container name**
```bash
docker run -d --name my-nginx nginx
```

**3. Port mapping**
```bash
docker run -d -p 8081:80 --name web1 nginx
```
`-p host_port:container_port` — maps a port on the host to a port inside the container so traffic reaches the containerized service.

**4. Check logs**
```bash
docker logs my-nginx
```

**5. Exec into a running container**
```bash
docker exec -it my-nginx bash
```
Opens an interactive shell inside the already-running container — useful for debugging without stopping the service.

`[Insert screenshot: docker logs and docker exec output]`

---

## Key Takeaways

- Containers isolate at the process level using the host kernel — much lighter than VMs, which virtualize hardware and run a full guest OS.
- Docker's client-daemon-registry model is the same pull-request pattern I already work with for Ansible/Jenkins pipelines — just applied to shipping application environments instead of config.
- `-d`, `-p`, `--name`, `logs`, and `exec` are the five flags/commands I'll use daily once this becomes part of a CI/CD pipeline.

## Why This Matters for DevOps

Docker is the foundation of modern deployment. Every CI/CD pipeline, Kubernetes cluster, and microservice architecture starts with containers. This is also directly relevant to my current Jenkins pipeline work — the next logical step is containerizing build/deploy stages instead of running them directly on VMs.

---
`#90DaysOfDevOps` `#DevOpsKaJosh` `#TrainWithShubham`
