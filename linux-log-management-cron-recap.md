# Linux Log Management & Cron Jobs

This is a quick reference guide and interview prep sheet for handling Cron jobs and safely managing disk space on Linux servers (e.g., EC2 instances).

## 1. Cron Job Logging

**Concept:** Cron does *not* automatically save the standard output (stdout/stderr) of scripts to a file. By default, it tries to email it to the user. 

**Where to find logs:**
*   **Daemon Logs (System execution):** Tells you if the job started/stopped and who ran it.
    *   *Ubuntu/Debian:* `/var/log/syslog` or `journalctl -u cron`
    *   *RHEL/CentOS:* `/var/log/cron`
*   **Job Output Logs:** Explicit redirection is required in the crontab.
    *   *Example:* `* * * * * /path/to/script.sh >> /var/log/my_script.log 2>&1`
*   **Default Fallback:** If not redirected, check the user's local mail (e.g., `/var/mail/root` or `sudo mail`).

**Who is running the job?**
*   Look at system daemon logs: `CRON[123]: (username) CMD (...)`
*   Check user crontabs: `sudo crontab -u <username> -l`
*   System-wide crontab (`/etc/crontab`): User is explicitly defined in the 6th column.

---

## 2. Safe EC2/Linux Log Cleanup (Interview Focus)

**The Interview Trap:** 
If an interviewer asks "How do you free up space if logs are filling up the disk?", **DO NOT say `rm /var/log/syslog`**. 
*Why?* Removing an active log file creates a "zombie" file. The application (like Nginx or Syslog) holds the file handle open. The disk space is not actually freed until the service is restarted, and logging will break.

### The Safe Cleanup Plan

**Phase 1: Assess**
*   `df -h` : Check overall disk space.
*   `df -i` : Check inode usage (prevents the "disk has space but can't write" issue caused by millions of tiny files).
*   `sudo du -sh /var/log/* | sort -h` : Identify the largest log files/folders.

**Phase 2: Actionable Cleanup Commands**

| Goal | Command | Why it's Safe/Best Practice |
| :--- | :--- | :--- |
| **Clean Binary Logs** | `sudo journalctl --vacuum-time=7d`<br>`sudo journalctl --vacuum-size=500M` | Systemd `journald` logs grow massively but silently. Vacuuming safely removes older entries without breaking the daemon. |
| **Remove Archived Logs** | `sudo find /var/log -type f -name "*.gz" -mtime +14 -delete` | `logrotate` compresses old logs into `.gz` files. It is 100% safe to delete these inactive historical archives based on age. |
| **Empty Active Logs** | `sudo truncate -s 0 /var/log/syslog` | Instantly drops the file size to 0 bytes *without* deleting the file object. The running service keeps its file handle and continues logging seamlessly. |

## 3. Key Takeaways for Career/Interviews
*   **Always redirect cron output:** Silent failures in cron are a classic production incident cause.
*   **Logrotate is your friend:** Production servers should have `/etc/logrotate.d/` configured for all application logs to prevent disk exhaustion in the first place.
*   **Truncate > RM:** Truncating is the operational standard for clearing active text logs on a live system without causing downtime.