# Explanation of "Server Review"

## Overview of "Server Review"
"Server Review" involves checking the health, performance, and security of a Linux server. It includes:

*   **Uptime and Load**: How long the server has been running and its current workload.
*   **Authentication Logs**: Checking who logged in and if there were any failed attempts.
*   **Services Running**: Seeing which programs are actively running on the server (like a web server).
*   **Available Memory / Disk**: Making sure there's enough space and memory for the server to work well.
*   **Network Connectivity**: Checking if the server can reach the internet and if its important ports are open.
*   **Process Monitoring**: Finding out which programs are using a lot of the server's resources.

These checks help keep the server healthy, fix problems, and make it more secure.

## Key Components

*   **Uptime and Load**:
    **Purpose**: See how long the server has been on and how busy it is.
    **Command**: `uptime`
    *Example*: `uptime`

*   **Authentication Logs**:
    **Purpose**: Look at login records to spot unusual activity.
    **Command**: `tail` (to see the end of a file), `cat` (to see the whole file), `grep` (to search for words).
    *Example*: `sudo tail /var/log/auth.log` (shows the last few login attempts).

*   **Services Running**:
    **Purpose**: Check which background programs are active.
    **Command**: `systemctl` (for newer Linux systems), `service` (for older ones).
    *Example*: `systemctl list-units --type=service --state=running` (lists all active services).

*   **Available Memory / Disk**:
    **Purpose**: Check how much free memory and disk space the server has.
    **Command**: `free` (for memory), `df` (for disk space).
    *Example*: `free -h` (shows memory in an easy-to-read format), `df -h` (shows disk space in an easy-to-read format).

*   **Network Connectivity**:
    **Purpose**: Test if the server can connect to other computers and if its network doors (ports) are open.
    **Command**: `ping` (to test connection), `ss` (to see open ports).
    *Example*: `ping google.com -c 3` (sends 3 test messages to Google), `ss -tuln` (lists all open network ports).

*   **Process Monitoring**:
    **Purpose**: See what programs are running and how much CPU or memory they are using.
    **Command**: `ps` (lists processes), `top` (shows live process usage).
    *Example*: `ps aux` (lists all running processes), `top` (shows a live view of processes).

## Practical Example: Performing a Server Review

Let’s do a quick check-up on a Linux server. You might need `sudo` for some commands.

### Step-by-Step Example

1.  **Check Server Uptime and Load**:
    ```bash
    uptime
    ```
    *What you'll see*: Something like `10:30:00 up 2 days, 5 users, load average: 0.50, 0.45, 0.40`.
    *Meaning*: The server has been on for 2 days, 5 people are logged in, and it's not very busy.

2.  **Look at Recent Login Attempts**:
    ```bash
    sudo tail /var/log/auth.log
    ```
    *What you'll see*: Lines showing successful or failed logins. Look for anything suspicious.
    *Meaning*: Helps you catch unauthorized access attempts.

3.  **See What Services Are Running**:
    ```bash
    systemctl list-units --type=service --state=running | head -n 5
    ```
    *What you'll see*: A list of active services, like `ssh.service` or `apache2.service`.
    *Meaning*: Confirms that important services are up and running.

4.  **Check Free Memory and Disk Space**:
    ```bash
    free -h
    df -h
    ```
    *What you'll see*: For `free -h`, how much RAM is used/free. For `df -h`, how much disk space is used/free on your drives.
    *Meaning*: Ensures your server isn't running out of resources.

5.  **Test Network Connection and Open Ports**:
    ```bash
    ping -c 2 8.8.8.8
    ss -tuln
    ```
    *What you'll see*: For `ping`, if messages are sent and received. For `ss`, a list of network ports that are open and listening.
    *Meaning*: Confirms the server can connect to the internet and its services are accessible.

6.  **Identify Busy Processes**:
    ```bash
    top -b -n 1 | head -n 10
    ```
    *What you'll see*: A snapshot of the processes using the most CPU or memory.
    *Meaning*: Helps you find programs that might be slowing down your server.

### Full Script for Reference

Save this as `simple_server_review.sh`:

```bash
#!/bin/bash

echo "--- Simple Server Health Check ---"

echo "\n1. Server Uptime and Load:"
uptime

echo "\n2. Recent Login Attempts:"
sudo tail /var/log/auth.log

echo "\n3. Top 5 Running Services:"
systemctl list-units --type=service --state=running | head -n 5

echo "\n4. Memory Usage:"
free -h

echo "\n5. Disk Space Usage:"
df -h

echo "\n6. Network Connectivity Test (ping Google):"
ping -c 2 8.8.8.8

echo "\n7. Open Network Ports:"
ss -tuln

echo "\n8. Top Processes by CPU Usage:"
top -b -n 1 | head -n 10

echo "\n--- Check Complete ---"
```

Make it executable: `chmod +x simple_server_review.sh`.
Run it: `./simple_server_review.sh` (use `sudo` if prompted).