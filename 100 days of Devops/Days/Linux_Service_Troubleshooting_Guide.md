# Linux Service Troubleshooting Guide

## General Troubleshooting Workflow

``` text
Problem Reported
       │
       ▼
Understand the Problem
       │
       ▼
Check Service Status
       │
       ▼
Read Logs
       │
       ▼
Identify Root Cause
       │
       ▼
Verify Related Components
       │
       ▼
Apply the Fix
       │
       ▼
Restart Service
       │
       ▼
Validate the Fix
       │
       ▼
Monitor
```

## Step 1: Understand the Problem

Before making any changes, understand exactly what is failing.

Ask yourself:

-   Is the service not starting?
-   Is the service crashing?
-   Is the application unreachable?
-   Is the database refusing connections?
-   Is the service slow?

Example:

> MariaDB service is not running.

------------------------------------------------------------------------

## Step 2: Check the Service Status

Always begin by checking the service status.

``` bash
systemctl status <service-name>
```

Example:

``` bash
systemctl status mariadb
```

Look for:

-   Active (running)
-   inactive
-   failed
-   exit code
-   permission denied
-   dependency failure

If the service is stopped:

``` bash
systemctl start <service-name>
```

------------------------------------------------------------------------

## Step 3: Read the Logs

Logs are the most reliable source for identifying the root cause.

System logs:

``` bash
journalctl -xeu <service-name>
```

Application-specific logs:

**MariaDB**

``` bash
cat /var/log/mariadb/mariadb.log
```

**Apache**

``` bash
cat /var/log/httpd/error_log
```

**Nginx**

``` bash
cat /var/log/nginx/error.log
```

**Docker**

``` bash
docker logs <container-name>
```

Never guess the problem before reading the logs.

------------------------------------------------------------------------

## Step 4: Understand the Error

Read the error message carefully.

Examples:

**Permission denied**

Think:

-   Which file?
-   Which directory?
-   Which user?

**Can't create PID file**

Think:

-   Does the directory exist?
-   Does the service user have permission?

**No space left on device**

Think:

-   Check available disk space.

------------------------------------------------------------------------

## Step 5: Check Common Root Causes

### 1. Service Status

``` bash
systemctl status <service-name>
```

### 2. Logs

``` bash
journalctl -xeu <service-name>
```

### 3. Permissions

``` bash
ls -l
```

Fix ownership:

``` bash
chown
```

Fix permissions:

``` bash
chmod
```

### 4. Missing Directory

Check:

``` bash
ls
```

Create if required:

``` bash
mkdir -p
```

### 5. Disk Space

``` bash
df -h
```

### 6. Memory

``` bash
free -h
```

### 7. CPU Usage

``` bash
top
```

or

``` bash
htop
```

### 8. Port Conflicts

``` bash
ss -tulpn
```

or

``` bash
netstat -tulpn
```

### 9. Configuration Files

Examples:

-   MariaDB: `/etc/my.cnf`
-   Apache: `apachectl configtest`
-   Nginx: `nginx -t`

### 10. Dependencies

``` bash
systemctl list-dependencies <service-name>
```

------------------------------------------------------------------------

## Step 6: Apply the Fix

Fix only the identified root cause.

Examples:

-   Correct file ownership (`chown`)
-   Fix file permissions (`chmod`)
-   Create missing directories (`mkdir`)
-   Correct configuration
-   Resolve port conflicts
-   Free disk space

------------------------------------------------------------------------

## Step 7: Restart the Service

``` bash
systemctl restart <service-name>
```

------------------------------------------------------------------------

## Step 8: Verify the Service

``` bash
systemctl status <service-name>
```

Additional checks:

Running process:

``` bash
ps -ef
```

Listening ports:

``` bash
ss -tulpn
```

Database connection:

``` bash
mysql -u root -p
```

------------------------------------------------------------------------

## Step 9: Validate the Application

Verify that the application works correctly.

Examples:

-   Website loads successfully.
-   Database accepts connections.
-   API responds correctly.
-   No new errors appear in the logs.

------------------------------------------------------------------------

# Universal DevOps Troubleshooting Checklist

  Check                  Command
  ---------------------- -------------------------------
  Service Status         `systemctl status <service>`
  Start Service          `systemctl start <service>`
  Restart Service        `systemctl restart <service>`
  Service Logs           `journalctl -xeu <service>`
  Application Logs       `cat /var/log/...`
  Disk Space             `df -h`
  Memory                 `free -h`
  CPU Usage              `top` / `htop`
  Running Processes      `ps -ef`
  Listening Ports        `ss -tulpn`
  File Permissions       `ls -l`
  Change Ownership       `chown`
  Change Permissions     `chmod`
  Configuration          Check `/etc/...`
  Network Connectivity   `ping`, `curl`, `nc`
  Firewall               `firewall-cmd`, `iptables`
  SELinux                `getenforce`, `sestatus`

------------------------------------------------------------------------

# DevOps Troubleshooting Mindset

Whenever you troubleshoot a production issue:

1.  Identify the exact symptom.
2.  Reproduce the issue if possible.
3.  Read the logs before making changes.
4.  Determine which component is failing.
5.  Check for recent changes.
6.  Fix the root cause, not just the symptom.
7.  Verify that the service and application are healthy.
8.  Document the issue and its resolution for future reference.
