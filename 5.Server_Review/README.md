# Explanation of "Server Review"

## Overview of "Server Review"
"Server Review" involves checking the health, performance, and security of a Linux server. It includes:

*   **Uptime and Load**: Monitoring how long the server has been running and its current workload.
*   **Authentication Logs**: Reviewing logs to detect login attempts or security issues.
*   **Services Running**: Checking which services (e.g., web server, SSH) are active.
*   **Available Memory / Disk**: Ensuring there’s enough resources for operation.
*   **Network Connectivity**: Verifying network reachability and open ports.
*   **Process Monitoring**: Identifying resource-intensive or problematic processes.

These tasks help administrators maintain server stability, troubleshoot issues, and enhance security.

## Key Components

*   **Uptime and Load**:
    **Purpose**: Check how long the server has been running (`uptime`) and its current load average (`top` or `uptime`).
    **Command**: `uptime`
    *Example*: `uptime`

*   **Authentication Logs**:
    **Purpose**: Review login activity and security events in logs (e.g., `/var/log/auth.log` on Debian/Ubuntu, `/var/log/secure` on RHEL/CentOS).
    **Command**: `cat`, `less`, `grep`
    *Example*: `sudo tail /var/log/auth.log` (shows last 10 lines of authentication log)

*   **Services Running**:
    **Purpose**: List active services to ensure only intended ones are running.
    **Command**: `systemctl` (for systemd-based systems), `service` (for older init systems)
    *Example*: `systemctl list-units --type=service --state=running`

*   **Available Memory / Disk**:
    **Purpose**: Monitor free memory and disk space to prevent resource exhaustion.
    **Command**: `free` (memory), `df` (disk)
    *Example*: `free -h` (human-readable memory), `df -h` (human-readable disk space)

*   **Network Connectivity**:
    **Purpose**: Verify network reachability and check open ports.
    **Command**: `ping`, `ss` (or `netstat`)
    *Example*: `ping google.com -c 4` (check external connectivity), `ss -tuln` (list listening TCP/UDP ports)

*   **Process Monitoring**:
    **Purpose**: Identify running processes, their resource consumption, and potential issues.
    **Command**: `ps`, `top`, `htop`
    *Example*: `ps aux | head -n 5` (list top 5 processes by CPU/memory), `top -b -n 1 | head -n 10` (snapshot of top processes)

## Practical Example: Performing a Server Review

Let’s simulate a server review on a Linux system (e.g., Ubuntu). This example assumes you have administrative access and can run commands like `sudo` if needed.

### Step-by-Step Example

1.  **Check Uptime and Load**:
    ```bash
    uptime
    ```
    *Example Output*:
    ` 09:36:15 up 5 days, 3:12, 2 users, load average: 0.15, 0.20, 0.25`
    *Explanation*: Shows the server has been up for 5 days, 3 hours, with 2 users logged in and a load average (1, 5, 15-minute averages) indicating light usage.

2.  **Review Authentication Logs**:
    ```bash
    sudo tail /var/log/auth.log
    ```
    *Example Output*:
    ```
    Jul 15 09:30:01 server sshd[1234]: Accepted password for user from 192.168.1.1 port 12345
    Jul 15 09:31:02 server sshd[1235]: Failed password for invalid user from 10.0.0.2 port 54321
    ```
    *Explanation*: Shows the last 10 log entries. Look for successful logins or failed attempts to detect potential security issues.

3.  **Check Services Running**:
    ```bash
    systemctl list-units --type=service --state=running | grep ssh
    ```
    *Example Output*:
    `  ssh.service                               loaded active running OpenSSH server`
    *Explanation*: Lists running services and filters for `ssh`. Ensures services like SSH or a web server (e.g., Apache) are active as expected.

4.  **Monitor Available Memory and Disk**:
    ```bash
    free -h
    df -h /var
    ```
    *Example Output*:
    `free -h`:
    ```
                  total        used        free      shared  buff/cache   available
    Mem:          7.8G        2.1G        4.5G        0.2G        1.2G        5.6G
    ```
    `df -h /var`:
    ```
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda1        50G   15G   33G  32% /
    ```
    *Explanation*: `free -h` shows 5.6GB of memory available. `df -h /var` shows disk space for the `/var` partition. Ensures memory and disk aren’t critically low (e.g., <10% free).

5.  **Test Network Connectivity and Open Ports**:
    ```bash
    ping -c 3 8.8.8.8
    ss -tuln | grep 22
    ```
    *Example Output*:
    `ping`:
    ```
    PING 8.8.8.8 (8.8.8.8) 56(84) bytes of data.
    64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.3 ms
    ---
    3 packets transmitted, 3 received, 0% packet loss, time 2002ms
    ```
    `ss -tuln | grep 22`:
    ```
    tcp   LISTEN 0      128    0.0.0.0:22      0.0.0.0:* 
    ```
    *Explanation*: `ping` verifies external network reachability. `ss -tuln | grep 22` checks if port 22 (SSH) is listening.

6.  **Monitor Processes**:
    ```bash
    ps aux --sort=-%cpu | head -n 5
    ```
    *Example Output*:
    ```
    USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
    root           1  0.0  0.0 168000  9000 ?        Ss   Jul10   0:01 /sbin/init
    user       12345  0.5  1.2 123456 12345 ?        Sl   Jul15   0:05 /usr/bin/python3 /opt/my_app/app.py
    ```
    *Explanation*: Lists the top 5 processes by CPU usage. Helps identify any processes consuming excessive resources.

### Full Script for Reference

Save as `server_review.sh`:

```bash
#!/bin/bash

echo "--- Server Review Report ---"

echo "\n1. Uptime and Load:"
uptime

echo "\n2. Last 10 Authentication Logs:"
sudo tail /var/log/auth.log

echo "\n3. Running Services (SSH service example):"
systemctl list-units --type=service --state=running | grep ssh

echo "\n4. Memory Usage:"
free -h

echo "\n5. Disk Usage (for /var partition):"
df -h /var

echo "\n6. Network Connectivity (ping google.com):"
ping -c 3 8.8.8.8

echo "\n7. Open Ports (SSH port 22 example):"
ss -tuln | grep 22

echo "\n8. Top 5 CPU-consuming Processes:"
ps aux --sort=-%cpu | head -n 5

echo "\n--- End of Report ---"
```

Make executable: `chmod +x server_review.sh`.
Run: `./server_review.sh` (use `sudo` if needed for logs and some commands).
