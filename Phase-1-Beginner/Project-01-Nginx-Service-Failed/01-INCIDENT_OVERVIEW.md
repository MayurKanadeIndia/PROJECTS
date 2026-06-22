# Section 1: Incident Overview

## Executive Summary

**Incident:** Nginx web server crashed and is refusing to start  
**Severity:** Critical  
**Business Impact:** All users unable to access web application  
**Detection Time:** 5 minutes (automated health check)  
**Resolution Time:** ~30 minutes (typical)  
**Root Cause:** Configuration syntax error in nginx.conf

---

## Business Impact

### User Experience
- Users trying to access website see: **Connection refused** or **Unable to connect**
- No error page displays (server not responding at all)
- Mobile app fails to load
- API requests timeout
- Users attempt refresh, creating additional load
- Support team flooded with tickets within seconds

### Financial Impact
- E-commerce site: ~$5,000 per minute revenue loss
- SaaS platform: Customer accounts locked, penalty clauses triggered
- Ad-supported site: Lost impressions and clicks
- Reputation damage: Social media complaints

### Operational Impact
- Oncall engineer woken at 3 AM
- Incident command activated
- Multiple teams investigating
- Production alert fatigue

---

## Incident Scenario

### Timeline

```
09:30 AM  → Deployment team deploys new Nginx config to production
           (Config file has typo: "serber" instead of "server")

09:31 AM  → Monitoring system detects HTTP endpoint returning 0 bytes
           → Alert fires: "Website Health Check Failed"

09:32 AM  → Customer calls support: "Your site is down"
           → Support creates P1 incident

09:33 AM  → On-call engineer gets paged
           → Engineer SSH's to web server

09:34 AM  → Engineer runs: systemctl status nginx
           → Output: "failed (Result: exit-code)"

09:35 AM  → Engineer checks logs: journalctl -u nginx
           → Error: "[emerg] unexpected "serber" in /etc/nginx/nginx.conf:10"

09:37 AM  → Root cause identified: typo in config
           → Fix: Correct typo, restart service

09:38 AM  → Service starts successfully
           → Website responding to health checks

09:40 AM  → Status page updated: "Resolved"
           → Engineering team begins postmortem
```

---

## Architecture Involved

```
┌─────────────────────────────────────────────────────┐
│                    Users/Clients                    │
└────────────────────────┬────────────────────────────┘
                         │
                    HTTP Request
                    Port 80/443
                         │
                         ▼
┌─────────────────────────────────────────────────────┐
│                  GCP Cloud Load Balancer            │
│              (Health Checks Every 10s)             │
└────────────────────────┬────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────┐
│              Compute Engine VM (e2-micro)          │
│  ┌──────────────────────────────────────────────┐  │
│  │    Operating System: Ubuntu 22.04 LTS       │  │
│  │    ┌────────────────────────────────────┐   │  │
│  │    │  Nginx Web Server (Process)        │   │  │
│  │    │  - Listens on port 80              │   │  │
│  │    │  - Reads /etc/nginx/nginx.conf     │   │  │
│  │    │  - Serves static content           │   │  │
│  │    │  - Managed by systemd              │   │  │
│  │    └────────────────────────────────────┘   │  │
│  │    ┌────────────────────────────────────┐   │  │
│  │    │  systemd (Init System)             │   │  │
│  │    │  - Starts/stops services           │   │  │
│  │    │  - Manages logs (journald)         │   │  │
│  │    │  - Monitors service health         │   │  │
│  │    └────────────────────────────────────┘   │  │
│  └──────────────────────────────────────────────┘  │
│                                                     │
│  Network Interfaces:                              │
│  - eth0 (Internal IP: 10.128.0.2)                │
│  - Port 80 listening for HTTP                     │
│  - Port 443 listening for HTTPS (if config)      │
└─────────────────────────────────────────────────────┘
```

---

## Severity Classification

### This Incident = CRITICAL (P1)

| Severity | Definition | Example |
|----------|-----------|----------|
| **Critical (P1)** | Production completely down, all users affected | **This incident** - website unreachable |
| **High (P2)** | Partial outage, major feature broken | Checkout not working, 50% users affected |
| **Medium (P3)** | Feature degraded, workaround exists | Page slow, users can retry |
| **Low (P4)** | Minor issue, no workaround needed | Typo in error message |

### Response Requirements
- Immediate page oncall engineer
- Escalate to incident commander
- Notify stakeholders
- Maximum resolution time: 60 minutes
- Status updates every 5 minutes

---

## Why This Happens In Production

### Root Cause Categories

1. **Configuration Errors** (Most Common)
   - Typo in config file
   - Copy-paste mistakes
   - Forgot to reload after change
   - Environment variables not set

2. **Deployment Issues**
   - Config deployed without testing
   - Wrong file pushed (binary instead of config)
   - Permissions changed during deployment

3. **Resource Exhaustion**
   - System ran out of file descriptors
   - Out of memory during startup
   - No disk space for logs

4. **External Dependencies**
   - SSL certificate not readable
   - Required upstream service down
   - DNS resolution failing

5. **Operator Error**
   - Service disabled by accident
   - Manual config edit left incomplete
   - Forgot to restart after change

### Why It's Hard to Catch

✗ Config looks correct to human eyes  
✗ Typo is one character different  
✗ Syntax checker might not run  
✗ No pre-deployment testing  
✗ Service fails silently (just won't start)  

---

## Learning Objectives

After this incident, you will understand:

### Technical Skills
1. How to check if a service is running
2. How to read systemd logs
3. How to validate Nginx configuration
4. How to check if a port is listening
5. How to restart a service safely
6. How to perform basic incident response

### Incident Response Skills
1. Don't panic - follow a methodology
2. Investigate before fixing
3. Check logs first, always
4. Use appropriate commands for each layer
5. Verify fix actually works
6. Document what happened

### Production Thinking
1. Configuration is as critical as code
2. Changes need validation
3. Services fail silently sometimes
4. Monitoring is your friend
5. Logs tell the whole story

---

## What We're Simulating

### Real-World Example 1: GitHub (2015)
Nginx configuration syntax error in production deployment caused 15-minute outage. Impact: ~3 million users affected.

### Real-World Example 2: Lambda Function Cold Starts
Service deployed with bad environment variables. Service starts, but can't connect to database. API returns 503 for 20 minutes until diagnosed.

### Real-World Example 3: Kubernetes Config
Incorrect YAML indentation in deployment. Pod never starts, looks like infrastructure issue. Actually it's config syntax.

---

## Next Step

👉 **Go to [Section 2: Lab Setup](02-LAB_SETUP.md)**

We'll create the GCP infrastructure for this incident.
