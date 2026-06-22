# The Linux Troubleshooting Mindset

## Core Principles

### 1. Symptoms ≠ Root Cause

**Common Mistake:**
- Symptom: "Website is slow"
- Wrong fix: "Add more servers"

**Correct Approach:**
- Symptom: "Website is slow"
- Investigation: Is it CPU, memory, disk, network, or database?
- Root cause: Database queries take 5s each (not infrastructure)
- Fix: Optimize queries, add indexes, or cache results

---

### 2. Use Hypothesis-Driven Investigation

**The Method:**

```
1. Observe symptom
   ↓
2. Form hypothesis (WHY is this happening?)
   ↓
3. Test hypothesis (What commands will prove/disprove?)
   ↓
4. Gather evidence (What does the output tell us?)
   ↓
5. Accept/reject hypothesis
   ↓
6. Repeat or move to root cause
```

**Example:**

Symptom: "Server unreachable"

Hypothesis 1: Network is down
- Test: `ping 8.8.8.8` from the server
- Evidence: No response
- Verdict: Network likely down

Hypothesis 2: Only specific IPs are blocked
- Test: `traceroute 8.8.8.8`
- Evidence: Route hangs at specific hop
- Verdict: Firewall at hop blocking

---

### 3. Separate Layers

**Network Model Approach:**

```
Application Layer    ← Is the service running?
Transport Layer      ← Is the port listening?
Network Layer        ← Can I reach the IP?
Link Layer           ← Is the NIC up?
Physical Layer       ← Is the cable plugged in?
```

**When troubleshooting, test each layer:**

```bash
# Layer 5 (App): Is service running?
sudo systemctl status nginx

# Layer 4 (Transport): Port listening?
ss -tlnp | grep :80

# Layer 3 (Network): Can reach IP?
ping 10.0.0.5

# Layer 2 (Link): NIC up?
ip link show

# Layer 1 (Physical): Check cable 😄
```

---

### 4. Check Logs First

**The Log Investigation Sequence:**

```bash
# 1. systemd journal (recent logs)
journalctl -u nginx -n 50 --no-pager

# 2. Application-specific logs
tail -f /var/log/nginx/error.log

# 3. System logs
tail -f /var/log/syslog

# 4. Messages with errors only
journalctl -p err -n 100
```

**Log Reading Tips:**
- Timestamps tell you when failures started
- Error messages often tell you EXACTLY what's wrong
- PID tells you which process to monitor
- Don't ignore warnings—they often precede errors

---

### 5. Think About Time

**Time-based Investigation:**

```
"When did this start happening?"

If: Very recently (last few minutes)
   → Look at recent changes (deployments, config changes, updates)
   → Check recent logs
   → Likely: Application or configuration issue

If: Gradually degrading over hours/days
   → Look at resource consumption (disk, memory, connections)
   → Check for leaks
   → Likely: Resource exhaustion or slow growth of problem

If: Always fails at same time daily
   → Look at scheduled jobs (cron, backups, batch processing)
   → Check for time-based resource spikes
   → Likely: Scheduled process interference
```

---

### 6. The Five Why's Technique

**Dig Deeper:**

```
Problem: Website is slow

Why 1: Why is the website slow?
       → Database queries take 5s

Why 2: Why do queries take 5s?
       → The queries scan millions of rows

Why 3: Why scan millions of rows?
       → No index on the filter column

Why 4: Why is there no index?
       → Developer didn't know it was needed

Why 5: Why didn't they know?
       → No database performance training

Root Cause: Lack of training, not infrastructure
Fix: Add index immediately (quick fix) + training (prevention)
```

---

### 7. Know Your Tools

**The Troubleshooter's Toolkit:**

| Category | Tools | What They Tell |
|----------|-------|----------------|
| Processes | top, ps, htop | CPU, memory, state |
| Disk | df, du, iostat | Space, usage, I/O |
| Network | ss, netstat, tcpdump | Ports, connections, packets |
| Logs | journalctl, tail, grep | Events, errors, warnings |
| System | uname, lsb_release | Version, kernel, distro |
| Files | ls, stat, lsattr | Permissions, ownership, attributes |
| Performance | perf, flame graphs | CPU hotspots |
| Tracing | strace, ltrace | System/library calls |

---

### 8. Document Your Findings

**Investigation Template:**

```markdown
## Incident: [Title]

### Symptom
[What users/systems reported]

### Initial Hypothesis
[What you thought was wrong]

### Investigation Steps
1. [Command 1] → [Finding 1]
2. [Command 2] → [Finding 2]
3. [Command 3] → [Finding 3]

### Evidence
[Key logs, outputs, metrics]

### Root Cause
[The actual problem]

### Fix
[What was done]

### Verification
[How confirmed it's fixed]

### Prevention
[How to avoid in future]
```

---

### 9. Stay Calm Under Pressure

**During Incidents:**

✅ DO:
- Take a deep breath
- Focus on stability first (prevent further damage)
- Ask for help if needed
- Document what you're doing
- Communicate status to team
- Work methodically, not frantically

❌ DON'T:
- Make random changes hoping something works
- Delete/modify without understanding
- Forget to commit changes
- Ignore previous changes in git history
- Work alone if overwhelmed

---

### 10. After Every Incident

**Postmortem Process:**

```
1. What happened? (Timeline)
2. Why did it happen? (Root cause)
3. What did we do well? (Celebrate wins)
4. What could we improve? (Blameless)
5. What are action items? (Prevent)
6. When will we verify fixes? (Follow-up)
```

---

## Quick Reference

### When You're Lost

```
1. Check if service is running:
   sudo systemctl status <service>

2. Check if port is listening:
   sudo ss -tlnp

3. Check disk space:
   df -h /

4. Check memory:
   free -h

5. Check recent errors:
   sudo journalctl -p err -n 50

6. Check process details:
   ps aux | grep <process>

7. Check network:
   ping 8.8.8.8
   curl -v http://localhost:80

8. Check logs:
   tail -f /var/log/syslog
```

---

## Remember

**Linux systems are logical:**
- Everything is a file
- Errors are usually in logs
- Symptoms have causes
- Tools exist to find those causes
- You can learn by doing

**You've got this.** 🚀
