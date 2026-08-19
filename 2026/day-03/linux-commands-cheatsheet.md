# Day 03 – Linux Commands Cheat Sheet

## Today's Goal

Today I am practicing common Linux commands that I can use during day-to-day troubleshooting.

I am focusing on:
- Process management
- File system
- Logs
- Networking

The aim is not to memorize every option, but to understand when I would use each command.

---

## 1. Process Management

| Command | What I use it for |
|---|---|
| `ps aux` | List running processes with CPU and memory details. |
| `top` | Monitor processes and system resource usage in real time. |
| `pgrep <name>` | Find the PID of a process by its name. |
| `kill <PID>` | Send a signal to a process. |
| `pkill <name>` | Send a signal to processes matching a name. |
| `nice` | Start a process with a specific priority. |
| `uptime` | Quickly check system uptime and load average. |

Example:

```bash
ps aux | grep nginx
```

I can use this when checking whether an application process is running.

---

## 2. File System Commands

| Command | What I use it for |
|---|---|
| `pwd` | Show the current directory. |
| `ls -lh` | List files with readable sizes and details. |
| `cd <directory>` | Move to another directory. |
| `find` | Search for files and directories. |
| `du -sh <directory>` | Check how much space a directory is using. |
| `df -h` | Check filesystem disk usage. |
| `cp` | Copy files or directories. |
| `mv` | Move or rename files and directories. |
| `rm` | Remove files or directories carefully. |

Useful troubleshooting example:

```bash
df -h
du -sh /var/log/*
```

I can use these when investigating a disk-space alert.

---

## 3. Logs and Text

| Command | What I use it for |
|---|---|
| `cat <file>` | Display the contents of a file. |
| `less <file>` | Read a large file page by page. |
| `tail -f <file>` | Watch a log file as new entries are added. |
| `head <file>` | View the beginning of a file. |
| `grep <pattern> <file>` | Search for a specific word or pattern. |
| `journalctl` | View logs collected by systemd. |

Example:

```bash
tail -f /var/log/application.log
```

For a systemd service:

```bash
journalctl -u nginx
```

---

## 4. Networking Troubleshooting

| Command | What I use it for |
|---|---|
| `ping <host>` | Check basic network connectivity to a host. |
| `ip addr` | Check IP addresses and network interfaces. |
| `ip route` | Check the system routing table. |
| `ss -tuln` | Check listening TCP/UDP ports. |
| `dig <domain>` | Check DNS resolution and DNS records. |
| `curl <url>` | Test HTTP/HTTPS connectivity and application responses. |

Examples:

```bash
ping google.com
ip addr
dig google.com
curl -I https://example.com
```

If an application is not reachable, I can start by checking the interface, route, DNS, port, and HTTP response.

---

## 5. Commands I Want to Remember

A few commands that I expect to use frequently in DevOps/SRE work:

```bash
ps aux
top
df -h
du -sh *
grep "ERROR" app.log
tail -f app.log
systemctl status <service>
journalctl -u <service>
ss -tuln
curl -I <url>
```

---

## Today's Practice

- [ ] Run `ps aux` and identify a few important processes.
- [ ] Use `top` and observe CPU and memory usage.
- [ ] Check disk usage with `df -h`.
- [ ] Find a large directory using `du`.
- [ ] Search a log file using `grep`.
- [ ] Follow a log using `tail -f`.
- [ ] Check my IP address using `ip addr`.
- [ ] Check listening ports using `ss -tuln`.
- [ ] Test DNS using `dig`.
- [ ] Test an HTTP endpoint using `curl`.

---

## Key Takeaway

Linux commands are mainly troubleshooting tools for me.

When something goes wrong, I should first collect information instead of guessing:

**Process → Files/Disk → Logs → Network → Service**

The more I practice these commands, the faster I should become at finding the actual cause of a production issue.
