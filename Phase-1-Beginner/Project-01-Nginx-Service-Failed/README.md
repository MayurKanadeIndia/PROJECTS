# Project 1: Web Server Down Because Nginx Service Failed

## Quick Summary

**Incident:** Your web application became unreachable to users. Investigation revealed the Nginx service crashed and won't start.

**Difficulty:** Beginner  
**Duration:** 3-4 hours  
**Skills Learned:** systemd service management, systemctl commands, journalctl log analysis, port troubleshooting, basic incident response

**Real-World Relevance:** This is the #1 incident you'll encounter in your first week as a Linux engineer. Services crash constantly in production.

---

## Project Overview

In this project, you will:

1. **Provision** a GCP VM with Nginx web server
2. **Intentionally break** the Nginx service by corrupting its configuration
3. **Observe symptoms** - website becomes unreachable
4. **Investigate** using systemd, journalctl, and port monitoring
5. **Find root cause** - bad config syntax in nginx.conf
6. **Apply fix** - restore valid configuration
7. **Verify** - service starts and serves traffic
8. **Learn** - understand service lifecycle, systemd internals, port binding
9. **Document** - create portfolio-ready GitHub project

---

## Learning Path

```
Phase 1 Projects (1-10)
├─ Project 1: Service Failure ✓ (You are here)
├─ Project 2: Firewall Blocking
├─ Project 3: Disk Full
├─ Project 4: SSH Permissions
├─ Project 5: Cron Job Failed
├─ Project 6: File Ownership
├─ Project 7: CPU Runaway
├─ Project 8: Memory Exhausted
├─ Project 9: DNS Failure
└─ Project 10: Log Rotation
```

---

## Table of Contents

1. [Incident Overview](01-INCIDENT_OVERVIEW.md) - Business impact and scenario
2. [Lab Setup](02-LAB_SETUP.md) - GCP infrastructure and deployment
3. [Create Incident](03-CREATE_INCIDENT.md) - How we break Nginx
4. [Observe Symptoms](04-OBSERVE_SYMPTOMS.md) - What users and systems see
5. [Investigation](05-INVESTIGATION.md) - Troubleshooting methodology
6. [Incident Analysis Table](06-INCIDENT_ANALYSIS_TABLE.md) - Evidence and findings
7. [Fix](07-FIX.md) - Step-by-step remediation
8. [Verification](08-VERIFICATION.md) - Confirm resolution
9. [Deep Linux Internals](09-DEEP_LINUX_INTERNALS.md) - systemd, process model, sockets
10. [AI Infrastructure Connection](10-AI_INFRASTRUCTURE_CONNECTION.md) - Why this matters for AI
11. [Lessons Learned](11-LESSONS_LEARNED.md) - Best practices and prevention
12. [Interview Questions](12-INTERVIEW_QUESTIONS.md) - 15 Q&A (all levels)
13. [Production Runbook](13-PRODUCTION_RUNBOOK.md) - Professional response guide
14. [GitHub Portfolio](14-GITHUB_PORTFOLIO.md) - How to document this
15. [Professional README](15-PROFESSIONAL_README.md) - Portfolio-quality documentation

---

## Before You Start

### Prerequisites
- GCP account with billing enabled
- gcloud CLI installed
- SSH key pair configured
- Basic Linux knowledge (what is a service, what is a port)
- Can SSH to a remote server
- Estimated budget: $2-3 for this project

### Knowledge Required
- Basic Linux commands (cd, ls, cat, grep)
- Understanding of web servers (they listen on ports)
- SSH basics
- Text file editing (nano or vim)

### What You'll Learn
- systemctl commands (status, start, stop, restart)
- journalctl to read systemd logs
- ss command to check ports
- nginx.conf configuration syntax
- How service startup works
- How to diagnose service failures

---

## Success Criteria

You've completed this project when:

✅ VM created and Nginx deployed  
✅ Nginx service broken intentionally  
✅ Symptoms observed and documented  
✅ Investigation performed without jumping to fix  
✅ Root cause identified correctly  
✅ Fix applied and verified  
✅ Nginx serving traffic again  
✅ All 15 sections documented  
✅ Professional README created on GitHub  
✅ Able to explain incident in interview  

---

## Estimated Timeline

| Phase | Duration | Tasks |
|-------|----------|-------|
| Setup | 20 min | Create VM, install Nginx |
| Create Incident | 10 min | Corrupt config, break service |
| Observe | 10 min | Check symptoms, gather evidence |
| Investigate | 45 min | Use tools, find root cause |
| Fix | 15 min | Repair config, start service |
| Verify | 10 min | Test service, confirm working |
| Document | 60 min | Write all 15 sections |
| **Total** | **~3.5 hours** | Complete learning |

---

## Getting Started

**Start here:** [Section 1: Incident Overview](01-INCIDENT_OVERVIEW.md)

---

## Key Concepts

### systemd
Modern Linux init system. Services managed via `systemctl`.

### journalctl
Systemd journal viewer. Where systemd logs everything.

### Port Binding
Services listen on ports. Nginx uses port 80 (HTTP) and 443 (HTTPS).

### Service Configuration
Nginx reads `/etc/nginx/nginx.conf`. Bad syntax = service won't start.

### systemctl Status States
- **active (running)** - Service is running
- **failed** - Service tried to start but failed
- **inactive (dead)** - Service is stopped
- **enabled** - Service will start on boot
- **disabled** - Service won't start on boot

---

## Quick Reference Commands

```bash
# Check service status
sudo systemctl status nginx

# View recent logs
journalctl -u nginx -n 50

# Check if port 80 is listening
sudo ss -tlnp | grep :80

# Start/stop/restart service
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx

# Check nginx config syntax
sudo nginx -t

# View nginx config
cat /etc/nginx/nginx.conf

# Test website
curl http://localhost
```

---

## Cost Estimate

**VM:** e2-micro (free tier eligible)  
**Disk:** 20 GB standard  
**Duration:** ~4 hours  
**Estimated Cost:** $0-2 (free tier or minimal)

---

## Next Steps

1. Read [Section 1: Incident Overview](01-INCIDENT_OVERVIEW.md)
2. Follow [Section 2: Lab Setup](02-LAB_SETUP.md) to create VM
3. Work through project sequentially
4. Complete all 15 sections
5. Move to Project 2

---

**Ready? Let's go!** 🚀
