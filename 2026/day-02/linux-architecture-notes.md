# Day 02 – Linux Architecture, Processes, and systemd

## Today's Task

The goal of Day 02 is to understand how Linux works internally and how processes and services are managed.

I will focus on:
- Linux kernel and user space
- How processes are created and managed
- Process states
- What `systemd` does
- Basic commands used for troubleshooting

---

## 1. Basic Linux Architecture

Linux can be understood in a few main layers:

- **Hardware** – CPU, RAM, disk, network devices, etc.
- **Kernel** – The core of Linux. It manages CPU, memory, processes, devices, networking, and system calls.
- **User Space** – Where applications and utilities run. Examples: Bash, SSH, Python, Nginx, Ansible, etc.
- **Init / systemd** – The first major userspace process started by the kernel. On most modern Linux systems, this is `systemd` (PID 1).

Simple flow:

`Hardware → Kernel → systemd / User Space → Applications`

---

## 2. Processes

A **process** is a running instance of a program.

Example:
- Running `nginx` starts one or more processes.
- Running `python app.py` creates a process.
- Every process has a **PID (Process ID)**.

Processes are normally created using mechanisms such as `fork()` and then `exec()` to start another program.

Important process concepts:
- **PID** – Unique process ID.
- **PPID** – Parent process ID.
- **Parent process** – Process that created another process.
- **Child process** – Process created by a parent.
- **PID 1** – Usually `systemd`, responsible for managing the userspace system.

---

## 3. Process States

A process can move through different states during its lifetime:

- **Running (R)** – Currently running or ready to run on the CPU.
- **Sleeping (S)** – Waiting for an event or resource.
- **Disk sleep (D)** – Usually waiting for I/O and cannot be interrupted normally.
- **Stopped (T)** – Process has been stopped, for example by a signal.
- **Zombie (Z)** – Process has finished, but its parent has not yet collected its exit status.

Useful command:

```bash
ps aux
```

The `STAT` column helps identify the process state.

---

## 4. What is systemd?

`systemd` is the service and system manager used by many modern Linux distributions.

It runs as **PID 1** and is responsible for things such as:

- Starting services during boot
- Stopping and restarting services
- Managing service dependencies
- Monitoring service status
- Managing targets and system startup
- Providing access to logs through `journalctl`

For example:

```bash
systemctl status ssh
systemctl restart ssh
systemctl enable ssh
```

For DevOps, `systemd` is important because many production applications run as services. If a service fails, `systemctl status` and `journalctl` are often the first places to check.

---

## 5. 5 Commands I Can Use Daily

1. `ps aux` – Check running processes and resource information.
2. `top` – Monitor CPU, memory, and processes in real time.
3. `systemctl status <service>` – Check whether a service is running.
4. `journalctl -u <service>` – Check logs for a systemd service.
5. `kill <PID>` – Send a signal to a process when it needs to be stopped or managed.

---

## 6. What I Need to Practice Today

- [ ] Run `ps aux` and identify 5 processes.
- [ ] Find the PID and PPID of a process.
- [ ] Run `top` and observe CPU and memory usage.
- [ ] Check the status of a service using `systemctl`.
- [ ] Read recent service logs using `journalctl`.
- [ ] Identify different process states from the `STAT` column.
- [ ] Understand why PID 1 is important.
- [ ] Practice restarting a non-critical service in my lab environment.

---

## Key Takeaway

Linux troubleshooting becomes easier when I understand what is happening below the application level.

For a production issue, I should be able to answer:
**Is the process running? Is it consuming too much CPU/memory? Is the service active? Did systemd restart it? What do the logs say?**
