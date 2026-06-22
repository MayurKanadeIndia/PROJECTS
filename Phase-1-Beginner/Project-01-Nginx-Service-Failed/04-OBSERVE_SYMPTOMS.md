# Section 4: Observe Symptoms

## Overview

Before investigating, we need to document exactly what we observe. This is how production incidents start - with symptoms reported by monitoring or users.

---

## User-Facing Symptoms

### From Client Perspective

**What users see:**

```bash
# Attempting to access website
$ curl -v http://34.x.x.x
*   Trying 34.x.x.x:80...
* Connection timed out after 5000ms
* Failed to connect to 34.x.x.x port 80: Connection refused
* Closing connection

curl: (7) Failed to connect to localhost port 80: Connection refused
```

**Browser behavior:**

```
User visits: http://mywebsite.com

Browser displays:
"Unable to connect"
"The site isn't responding"
"Connection refused"
"ERR_CONNECTION_REFUSED"
```

**Mobile app behavior:**
```
App attempts API call to: GET /api/users
Timeout after 30 seconds
User sees: "Network Error - Please try again"
```

---

## Monitoring Symptoms

### Health Check Alerts

```
⛔ ALERT FIRED: HTTP Health Check Failed
Timestamp: 2026-06-22 14:35:00 UTC
Service: nginx-prod
Endpoint: http://10.0.0.2:80/health
Status Code: TIMEOUT (no response)
Message: Health check failed 3 consecutive times
Severity: CRITICAL
```

### Metrics Dashboard

```
Metric                  | Before     | After
────────────────────────┼────────────┼─────────
HTTP Requests/sec       | 1,250      | 0
Healthy Instances       | 1/1        | 0/1
Avg Response Time       | 45ms       | timeout
Error Rate              | 0.1%       | 100%
Uptime Percentage       | 99.9%      | 50% (declining)
```

### System Metrics

```
CPU Usage:     0% (nginx not running)
Memory Usage:  2% (no processes)
Disk I/O:      0 IOPS (not serving)
Network IN:    ~5 Mbps (requests arriving, no response)
Network OUT:   ~0 Kbps (can't send responses)
Open Ports:    22 (SSH only), 80 NOT listening
```

---

## System-Level Symptoms

### Service Status

```bash
$ sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: failed (Result: exit-code) since Sun 2026-06-22 14:32:10 UTC; 2min 50s ago
       Docs: man:nginx(8)
    Process: 1234 ExecStart=/usr/sbin/nginx -g daemon off; (code=exited, status=1/FAILURE)
   Main PID: 1234 (code=exited, status=1/FAILURE)
        CPU: 2ms
```

### Port Listening

```bash
$ sudo ss -tlnp
State   Recv-Q  Send-Q  Local Address:Port  Peer Address:Port  Process
────────────────────────────────────────────────────────────────────────
LISTEN  0       128      0.0.0.0:22        0.0.0.0:*         users:(("sshd",pid=789,fd=3))
LISTEN  0       128      [::]:22           [::]:*            users:(("sshd",pid=789,fd=4))

(Note: Port 80 is NOT listening)
```

### Process List

```bash
$ ps aux | grep nginx
ubuntu  12345  0.0  0.0  5012  612 ?  S  14:32  0:00  grep --color=auto nginx

(Note: No nginx processes running)
```

### Systemd Journal Logs

```bash
$ sudo journalctl -u nginx -n 30
-- Logs begin at Sun 2026-06-22 14:20:00 UTC, end at Sun 2026-06-22 14:35:00 UTC --
Jun 22 14:32:10 project-01-nginx systemd[1]: nginx.service: Main process exited, code=exited, status=1/FAILURE
Jun 22 14:32:10 project-01-nginx systemd[1]: nginx.service: Failed with result 'exit-code'.
Jun 22 14:32:10 project-01-nginx systemd[1]: Failed to start A high performance web server and a reverse proxy server.
Jun 22 14:32:15 project-01-nginx systemd[1]: nginx.service: Scheduled restart job, restart counter is at 1.
Jun 22 14:32:15 project-01-nginx systemd[1]: nginx.service: Main process exited, code=exited, status=1/FAILURE
Jun 22 14:32:15 project-01-nginx systemd[1]: nginx.service: Failed with result 'exit-code'.
```

---

## Symptom Collection Commands

Run these commands to document all symptoms:

### 1. Service Status

```bash
#!/bin/bash
echo "=== SERVICE STATUS ==="
sudo systemctl status nginx
echo ""
echo "=== SERVICE STATE ==="
sudo systemctl is-active nginx
sudo systemctl is-enabled nginx
```

**Expected Output:**
```
=== SERVICE STATUS ===
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (...)
     Active: failed (Result: exit-code) since ...

=== SERVICE STATE ===
failed
enabled
```

### 2. Port Listening

```bash
echo "=== PORT LISTENING ==="
sudo ss -tlnp | grep -E '^LISTEN|:80|:443'
echo ""
echo "=== NETSTAT ALTERNATIVE ==="
sudo netstat -tlnp 2>/dev/null | grep -E 'Proto|:80|:443'
```

**Expected Output:**
```
=== PORT LISTENING ===
LISTEN  0  128  0.0.0.0:22  0.0.0.0:*  users:(("sshd",...))

(Port 80 should NOT appear)
```

### 3. Process List

```bash
echo "=== NGINX PROCESSES ==="
ps aux | grep nginx | grep -v grep
echo ""
echo "=== PROCESS COUNT ==="
pgrep -c nginx || echo "0 nginx processes"
```

**Expected Output:**
```
=== NGINX PROCESSES ===
(no output - no processes)

=== PROCESS COUNT ===
0 nginx processes
```

### 4. Recent Logs

```bash
echo "=== NGINX JOURNALCTL ==="
sudo journalctl -u nginx -n 20 --no-pager
echo ""
echo "=== SYSTEM ERRORS ==="
sudo journalctl -p err -n 10 --no-pager
```

**Expected Output:**
```
=== NGINX JOURNALCTL ===
Jun 22 14:32:10 project-01-nginx systemd[1]: nginx.service: Main process exited, code=exited, status=1/FAILURE
Jun 22 14:32:10 project-01-nginx systemd[1]: nginx.service: Failed with result 'exit-code'.
...
```

### 5. Connectivity Test

```bash
echo "=== CURL LOCALHOST ==="
curl -v http://localhost 2>&1 | head -20
echo ""
echo "=== CURL WITH TIMEOUT ==="
curl --max-time 5 http://localhost 2>&1 || echo "Connection refused"
```

**Expected Output:**
```
=== CURL LOCALHOST ===
*   Trying 127.0.0.1:80...
* Connection refused

curl: (7) Failed to connect to localhost port 80: Connection refused
```

### 6. Configuration Validation

```bash
echo "=== NGINX CONFIG TEST ==="
sudo nginx -t 2>&1
echo ""
echo "=== CONFIG FILE CHECK ==="
sudo ls -la /etc/nginx/nginx.conf
sudo head -30 /etc/nginx/nginx.conf | grep -E 'server|serber'
```

**Expected Output:**
```
=== NGINX CONFIG TEST ===
nginx: [emerg] unexpected "serber" in /etc/nginx/nginx.conf:31
nginx: configuration file /etc/nginx/nginx.conf test failed
```

---

## Complete Symptoms Document

Create a file to capture all symptoms:

```bash
# Create symptoms file
sudo bash -c 'cat > /tmp/symptoms-captured.txt << "EOF"
=== INCIDENT SYMPTOMS CAPTURED ===
Timestamp: $(date)

1. SERVICE STATUS
$(sudo systemctl status nginx)

2. ACTIVE STATE
$(sudo systemctl is-active nginx)

3. PORTS LISTENING
$(sudo ss -tlnp 2>/dev/null)

4. PROCESSES
$(ps aux | grep nginx | grep -v grep)

5. RECENT LOGS
$(sudo journalctl -u nginx -n 30)

6. CURL TEST
$(curl -v http://localhost 2>&1 || echo "Connection failed")

7. CONFIG TEST
$(sudo nginx -t 2>&1)

EOF
'

# View captured symptoms
sudo cat /tmp/symptoms-captured.txt
```

---

## Symptoms Summary Table

| Symptom | Status | Significance |
|---------|--------|---------------|
| Service State | **failed** | ❌ Service not running |
| Active Status | **No (failed)** | ❌ Systemd reports failure |
| Port 80 Listening | **No** | ❌ No HTTP listener |
| Nginx Process Count | **0** | ❌ No processes |
| curl http://localhost | **Connection refused** | ❌ Can't connect |
| nginx -t output | **[emerg] unexpected "serber"** | ⚠️ **This is the clue!** |
| journalctl logs | **exit-code failure** | ❌ Service exited with error |
| CPU usage | **0%** | ✓ Not consuming resources |
| Memory usage | **minimal** | ✓ Not a memory issue |
| Disk space | **normal** | ✓ Not a disk issue |

---

## What This Tells Us

### Clear Indicators

✅ **Service is definitely broken** - not just slow, completely non-functional  
✅ **It's a startup failure** - service tried to start and failed  
✅ **It's not a resource issue** - CPU and memory are fine  
✅ **It's not a network issue** - SSH works, so networking is up  
✅ **The error message is specific** - "unexpected serber" points to config  

### Hypothesis Formation

Based on these symptoms:

**Most Likely:** Configuration syntax error preventing Nginx from starting

**Evidence:**
- nginx -t shows: `[emerg] unexpected "serber"`
- Service exits immediately
- No processes running
- Port not listening

---

## Next Step

👉 **Go to [Section 5: Investigation](05-INVESTIGATION.md)**

Now we'll systematically investigate to confirm root cause.
