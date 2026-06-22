# Section 3: Create the Incident

## Overview

Now we intentionally break the Nginx service to simulate a real production failure. This is where we introduce the root cause: a configuration syntax error.

---

## What We're Breaking

**Current State:** Nginx is running fine, serving traffic normally.

**What We'll Do:** Introduce a typo in `/etc/nginx/nginx.conf` that will cause Nginx to fail at startup.

**The Typo:**
```
# WRONG (what we'll introduce)
serber {
  listen 80;
  server_name _;
}

# CORRECT (what should be there)
server {
  listen 80;
  server_name _;
}
```

This simple typo will cause:
1. Nginx configuration parser to fail
2. Service to exit on startup
3. Website to become completely unreachable
4. Users to see connection refused

---

## Why This Breaks

```
Nginx Startup Process:

1. systemd executes: /usr/sbin/nginx -g 'daemon off;'
   ↓
2. Nginx reads /etc/nginx/nginx.conf
   ↓
3. Parser encounters "serber" (invalid keyword)
   ↓
4. Parser returns: [emerg] unexpected "serber"
   ↓
5. Nginx exits with error code 1
   ↓
6. systemd marks service as "failed"
   ↓
7. Port 80 has no listener
   ↓
8. Connection attempts get: "Connection refused"
```

---

## Step-by-Step: Breaking Nginx

### Step 1: Connect to VM

```bash
# Set variables if not already set
export EXTERNAL_IP=$(gcloud compute instances describe project-01-nginx \
  --zone=us-central1-a \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)')

echo "Connecting to: $EXTERNAL_IP"

# SSH to VM
ssh -i ~/.ssh/google_compute_engine ubuntu@$EXTERNAL_IP
```

### Step 2: Verify Nginx is Currently Running

```bash
# Check service status
sudo systemctl status nginx

# Expected output:
# ● nginx.service - A high performance web server and a reverse proxy server
#      Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enabled)
#      Active: active (running) since ... UTC; ...
```

### Step 3: Verify Port 80 is Listening

```bash
# Check port
sudo ss -tlnp | grep :80

# Expected output:
# LISTEN 0 511 0.0.0.0:80 0.0.0.0:* users:(("nginx",pid=1234,...))
```

### Step 4: Test Nginx is Working

```bash
# Curl localhost
curl http://localhost

# Expected output:
# <!DOCTYPE html>
# <html>
# <head>
# <title>Welcome to nginx!</title>
```

### Step 5: Backup the Current Config

```bash
# Backup original config (IMPORTANT!)
sudo cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup

# Verify backup exists
ls -la /etc/nginx/nginx.conf*
```

### Step 6: View Current Config

```bash
# View current config
sudo cat /etc/nginx/nginx.conf

# You should see something like:
# user www-data;
# worker_processes auto;
# pid /run/nginx.pid;
# ...
# http {
#   ...
#   server {
#     listen 80 default_server;
#     ...
#   }
# }
```

### Step 7: Introduce the Typo

**Option A: Using sed (automated)**

```bash
# Replace "server {" with "serber {" (the typo)
sudo sed -i 's/^\s*server {/\tserber {/g' /etc/nginx/nginx.conf

# Verify the typo was introduced
sudo grep -n "serber {" /etc/nginx/nginx.conf
# Output: 31:  serber {  <-- This line number might differ
```

**Option B: Using nano (manual)**

```bash
# Open config in editor
sudo nano /etc/nginx/nginx.conf

# Find the line: server {
# Change it to: serber {
# Save: Ctrl+O, Enter, Ctrl+X

# Verify
sudo grep "serber" /etc/nginx/nginx.conf
```

**Option C: Using vim (manual)**

```bash
# Open config
sudo vim /etc/nginx/nginx.conf

# In vim:
# Press: /server<Enter> (search for "server")
# Move cursor over the word "server"
# Press: ciw (change inner word)
# Type: serber
# Press: Esc
# Type: :wq (save and exit)

# Verify
sudo grep "serber" /etc/nginx/nginx.conf
```

### Step 8: Validate the Typo is in Place

```bash
# Show the lines around the typo
sudo grep -B2 -A2 "serber" /etc/nginx/nginx.conf

# Expected output:
#    }
#
#    serber {
#        listen 80 default_server;
#        listen [::]:80 default_server;
```

### Step 9: Stop Nginx (for cleaner test)

```bash
# Stop the service
sudo systemctl stop nginx

# Verify it's stopped
sudo systemctl status nginx
# Should show: "inactive (dead)"
```

### Step 10: Try to Start Nginx (This Will Fail)

```bash
# Attempt to start - this should fail
sudo systemctl start nginx

# Check if it failed
sudo systemctl status nginx

# You should see:
# ● nginx.service - A high performance web server and a reverse proxy server
#      Loaded: loaded (...; enabled; preset: enabled)
#      Active: failed (Result: exit-code) since ... UTC; ...
#      Process: 12345 ExecStart=/usr/sbin/nginx -g ... (code=exited, status=1)
```

### Step 11: Check the Error

```bash
# Try to manually start Nginx to see the error
sudo nginx

# Expected output:
# nginx: [emerg] unexpected "serber" in /etc/nginx/nginx.conf:31
# nginx: configuration file /etc/nginx/nginx.conf test failed
```

---

## Verification: Incident Successfully Created

```bash
# Verify service is failed
sudo systemctl is-active nginx
# Output: failed

# Verify port 80 is NOT listening
sudo ss -tlnp | grep :80
# Output: (nothing - port not listening)

# Verify curl fails
curl http://localhost
# Output: curl: (7) Failed to connect to localhost port 80: Connection refused

# Verify Nginx process count is zero
sudo ps aux | grep "[n]ginx"
# Output: (no processes running)
```

---

## Symptoms Checklist

✅ Service status shows "failed"  
✅ Port 80 not listening  
✅ curl fails with "Connection refused"  
✅ Nginx process not running  
✅ journalctl shows error  
✅ nginx -t command fails  

---

## Expected Incident Behavior

From this point on:
- Any attempt to `curl http://localhost` will fail
- `systemctl status nginx` will show failed state
- `ss -tlnp | grep :80` will show no listener
- `journalctl -u nginx -n 20` will show the error
- Website is completely down

---

## Important Notes

⚠️ **You can still SSH to the VM** - only Nginx is broken, not the system  
⚠️ **Keep the backup** - we'll need it to verify our fix later  
⚠️ **This is a REAL incident** - everything that happens next is exactly like production  

---

## Next Step

👉 **Go to [Section 4: Observe Symptoms](04-OBSERVE_SYMPTOMS.md)**

Now we'll document exactly what we see as users and systems would see it.
