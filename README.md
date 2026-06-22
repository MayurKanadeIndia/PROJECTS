# Linux Reverse Engineering Learning Program: 50 Production Incidents

A comprehensive, hands-on Linux learning program through real-world production incidents. Master Linux administration, SRE practices, and AI infrastructure operations by solving 50 real incidents from beginner to senior engineer level.

## 📋 Program Overview

**Duration:** 50 projects (self-paced)  
**Target Audience:** Linux engineers, SREs, DevOps engineers, AI infrastructure engineers  
**Learning Format:** Incident-driven, reverse-engineering methodology  
**Infrastructure:** Google Cloud Platform (GCP)  
**Focus Areas:**
- Linux Administration & Troubleshooting
- Incident Response & RCA
- SRE Practices
- AI Infrastructure Operations
- Networking & Security
- Performance Tuning

---

## 📁 Repository Structure
```
Projects/
├── README.md                          # This file
├── ROADMAP.md                         # All 50 incidents roadmap
├── LEARNING_PHASES.md                 # Phase descriptions and objectives
├── GCP_SETUP.md                       # GCP prerequisites and cost estimates
├── Phase-1-Beginner/                 # Projects 1-10
│   ├── Project-01-Nginx-Service-Failed/
│   ├── Project-02-Firewall-Misconfiguration/
│   ├── Project-03-Disk-Full/
│   ├── Project-04-SSH-Permission-Denied/
│   ├── Project-05-Cron-Job-Failed/
│   ├── Project-06-File-Ownership-Broken/
│   ├── Project-07-High-CPU-Runaway/
│   ├── Project-08-Memory-Exhausted/
│   ├── Project-09-DNS-Resolution-Failure/
│   └── Project-10-Log-Rotation-Broken/
├── Phase-2-Junior-Admin/             # Projects 11-20
│   ├── Project-11-Package-Upgrade-Breaks-App/
│   ├── Project-12-VM-Boot-Fstab-Error/
│   ├── Project-13-Missing-Environment-Variables/
│   ├── Project-14-502-Bad-Gateway/
│   ├── Project-15-Missing-Default-Route/
│   ├── Project-16-Sudo-Policy-Broken/
│   ├── Project-17-Time-Drift-TLS-Failure/
│   ├── Project-18-Wrong-Disk-Path/
│   ├── Project-19-Monitoring-Agent-Failure/
│   └── Project-20-Logs-Missing-Cloud-Logging/
├── Phase-3-Intermediate/             # Projects 21-30
│   ├── Project-21-LB-Backend-Unhealthy/
```

```
│   ├── Project-22-Disk-IO-Bottleneck/
```

```
│   ├── Project-23-FD-Limit-Exhausted/
│   ├── Project-24-Port-Conflict/
│   ├── Project-25-OOM-Killer-Terminates/
│   ├── Project-26-TLS-Certificate-Expiry/
```

```
│   ├── Project-27-Bad-DNS-Record/
│   ├── Project-28-Zombie-Processes/
│   ├── Project-29-SSH-Brute-Force-Attack/
│   └── Project-30-Secret-Permission-Exposed/
├── Phase-4-Advanced/                 # Projects 31-40
│   ├── Project-31-TCP-Connection-Exhaustion/
```

```
│   ├── Project-32-Connection-Storm/
```

```
│   ├── Project-33-Swap-Thrashing/
```

```
│   ├── Project-34-Filesystem-Corruption/
```

```
│   ├── Project-35-Inode-Exhaustion/
```

```
│   ├── Project-36-CPU-Steal-Noisy-Neighbor/
```

```
│   ├── Project-37-IAM-Permission-Breaks-Access/
│   ├── Project-38-Broken-Startup-Script/
│   ├── Project-39-Observability-Blind-Spot/
│   └── Project-40-Security-Patch-Reboot-Downtime/
├── Phase-5-Senior-SRE/                # Projects 41-50
│   ├── Project-41-Regional-Outage-Failover/
│   ├── Project-42-Autoscaling-Failure/
```

```
│   ├── Project-43-GPU-Driver-CUDA-Mismatch/
```

```
│   ├── Project-44-Model-Serving-CPU-Bottleneck/
```

```
│   ├── Project-45-Vector-DB-Slow-Sizing/
```

```
│   ├── Project-46-Distributed-Training-MTU/
│   ├── Project-47-Checkpoint-Upload-Failure/
```

```
│   ├── Project-48-Service-Brownout/
│   ├── Project-49-Cost-Incident-RCA/
│   └── Project-50-Full-Platform-Outage/
├── templates/                         # Templates for all projects
│   ├── PROJECT_TEMPLATE.md            # Standard project structure
│   ├── INVESTIGATION_TABLE.md         # Symptom investigation table
│   └── RUNBOOK_TEMPLATE.md            # Production runbook template
```

```
├── scripts/                           # Utility scripts
```

- `│   ├── gcp-setup.sh                   # GCP infrastructure provisioning` 

- `│   ├── cleanup.sh                     # Cleanup and cost control` 

- `│   └── verify-setup.sh                # Verify GCP setup` 

- `├── docs/                              # Supporting documentation` 

- `│   ├── TROUBLESHOOTING_MINDSET.md     # How to think like an incident responder` 

- `│   ├── LINUX_FUNDAMENTALS.md          # Core Linux concepts referenced throughout` 

- `│   ├── AI_INFRASTRUCTURE_GLOSSARY.md  # AI/ML infrastructure terms` 

- `│   └── INTERVIEW_PREP_GUIDE.md        # Interview question bank` 


---

## 🎯 Project Structure (Per Project)
```
Project-NN-Title/
├── 01-INCIDENT_OVERVIEW.md            # Section 1: Business impact, severity,
scenario
```

- `├── 02-LAB_SETUP.md                    # Section 2: GCP lab setup, infrastructure diagram` 

- `├── 03-CREATE_INCIDENT.md              # Section 3: Break the system (exact commands)` 

- `├── 04-OBSERVE_SYMPTOMS.md             # Section 4: User complaints, alerts, logs` 

- `├── 05-INVESTIGATION.md                # Section 5: Troubleshooting methodology ├── 06-INCIDENT_ANALYSIS_TABLE.md      # Section 6: Symptoms, investigation, findings` 

- `├── 07-FIX.md                          # Section 7: Step-by-step remediation` 

- `├── 08-VERIFICATION.md                 # Section 8: Confirm issue is fixed` 

- `├── 09-DEEP_LINUX_INTERNALS.md         # Section 9: Kernel, process, network internals` 

- `├── 10-AI_INFRASTRUCTURE_CONNECTION.md # Section 10: LLM, GPU, training, inference relevance` 

- `├── 11-LESSONS_LEARNED.md              # Section 11: Lessons & best practices table` 

- `├── 12-INTERVIEW_QUESTIONS.md          # Section 12: 15 interview Q&A (3 levels) ├── 13-PRODUCTION_RUNBOOK.md           # Section 13: Professional runbook` 

- `├── 14-GITHUB_PORTFOLIO.md             # Section 14: Portfolio project guidance ├── 15-PROFESSIONAL_README.md          # Section 15: Portfolio-quality README ├── README.md                          # Main project documentation` 

- `├── cost-estimate.md                   # GCP cost for this project` 

- `├── architecture/                      # Infrastructure diagrams` 

- `│   ├── diagram.txt                    # ASCII architecture diagram` 

- `│   └── diagram.png                    # Visual diagram (if available) ├── incident/                          # Incident creation & setup` 

- `│   ├── create-incident.sh             # Bash script to break the system` 

- `│   ├── incident-log.txt               # Expected error messages` 

- `│   └── symptoms-checklist.md          # What to expect after breaking` 

- `├── investigation/                     # Investigation tools & commands` 

- `│   ├── commands-checklist.md          # All commands to run during investigation` 

- `│   ├── command-output-examples.md     # Example outputs (expected results)` 

- `│   └── investigation-flowchart.md     # Decision tree for troubleshooting ├── fixes/                             # Remediation scripts` 

- `│   ├── fix.sh                         # Automated fix script` 

- `│   ├── manual-fix-steps.md            # Step-by-step manual fix` 

- `│   └── verification.sh                # Verification script` 

- `├── screenshots/                       # Evidence of incident & resolution` 

- `│   ├── 01-incident-created.png` 

- `│   ├── 02-symptoms-observed.png` 

- `│   ├── 03-investigation-process.png` 

- `│   ├── 04-root-cause-identified.png` 

- `│   ├── 05-fix-applied.png` 

- `│   └── 06-verification-complete.png` 

- `└── lessons-learned/                   # Knowledge artifacts` 

```
    ├── summary.md                     # Quick reference
```

- `├── cheat-sheet.md                 # Commands and concepts quick reference └── common-mistakes.md             # What NOT to do` 



---

## 🚀 Getting Started

### Prerequisites
- Google Cloud Platform (GCP) account with billing enabled
- Basic Linux knowledge (can be very beginner)
- gcloud CLI installed locally
- Text editor or IDE

### Phase 1 Objectives (Projects 1-10)

By the end of Phase 1, you will:
- ✅ Understand basic Linux filesystem, processes, users, networking
- ✅ Master system monitoring and basic troubleshooting commands
- ✅ Know how to read and interpret system logs
- ✅ Understand Linux permissions and ownership
- ✅ Know how to manage services with systemd
- ✅ Understand basic networking and DNS

### Phase 2 Objectives (Projects 11-20)

By the end of Phase 2, you will:
- ✅ Handle package management and system upgrades
- ✅ Troubleshoot boot failures and filesystem issues
- ✅ Understand environment and configuration management
- ✅ Debug application integration issues
- ✅ Know observability infrastructure
- ✅ Understand cloud platform concepts (IAM, networking, monitoring)

### Phase 3 Objectives (Projects 21-30)

By the end of Phase 3, you will:
- ✅ Troubleshoot performance bottlenecks (CPU, memory, disk, network)
- ✅ Understand resource limits and constraints
- ✅ Debug security issues (SSL/TLS, permissions, access control)
- ✅ Analyze network issues and connectivity problems
- ✅ Understand cloud infrastructure (load balancers, health checks)

### Phase 4 Objectives (Projects 31-40)

By the end of Phase 4, you will:
- ✅ Analyze complex system performance issues
- ✅ Understand kernel tuning and advanced networking
- ✅ Troubleshoot security vulnerabilities and attacks
- ✅ Know cloud IAM and permissions deeply
- ✅ Understand automation, infrastructure-as-code, boot failures
- ✅ Recognize observability gaps

### Phase 5 Objectives (Projects 41-50)

By the end of Phase 5, you will:
- ✅ Design and troubleshoot production systems
- ✅ Architect AI/ML infrastructure (LLM serving, training, vector DBs)
- ✅ Understand SRE practices (incident response, postmortems, RCA)
- ✅ Master incident command and escalation
- ✅ Know chaos engineering and resilience testing
- ✅ Understand cost management and business impact

---

## 💡 How to Use This Repository

### Week 1-2: Phase 1 Completion
1. Read `LEARNING_PHASES.md`
2. Review `GCP_SETUP.md`
3. Start with `Phase-1-Beginner/Project-01-Nginx-Service-Failed/`
4. Work through all 15 sections
5. Create GitHub portfolio artifacts
6. Move to next project only when confident

### Workflow Per Project
1. Read Incident Overview & Lab Setup ↓
2. Provision GCP infrastructure ↓
3. Create the incident (break the system) ↓
4. Observe symptoms (like a real customer) ↓
5. Investigate (use methodology, not just fix) ↓
6. Apply fix & verify ↓
7. Review deep internals & AI relevance ↓
8. Prepare for interview questions ↓
9. Document in portfolio-quality README ↓
10. Clean up GCP resources


---

## 📊 Learning Progression
Phase 1 (Projects 1-10) ├─ Beginner Linux Skills ├─ System Fundamentals └─ Observable Failures

Phase 2 (Projects 11-20) ├─ Junior Admin Tasks ├─ Configuration Management └─ Cloud Basics

Phase 3 (Projects 21-30) ├─ Intermediate Troubleshooting ├─ Performance Analysis └─ Security Hardening

Phase 4 (Projects 31-40) ├─ Advanced Systems Thinking ├─ Kernel & Cloud Deep Dives └─ Complex Architectures

Phase 5 (Projects 41-50) ├─ SRE & Operations ├─ AI Infrastructure └─ Senior-Level Decision Making



---

## 🎓 What You'll Learn

### Linux Fundamentals
- File permissions, users, groups, ownership
- Processes, signals, process lifecycle
- Filesystem basics, disk management, mounting
- Systemd service management
- Basic networking concepts

### Troubleshooting Methodology
- Hypothesis-driven investigation
- Separating symptoms from root causes
- Reading and interpreting logs
- Using monitoring and observability tools
- Thinking like an incident responder

### Production Operations
- How real systems fail
- Security implications of failures
- Cost and business impact
- Automation and infrastructure-as-code
- Incident response and RCA

### AI Infrastructure Operations
- GPU and CUDA fundamentals
- Distributed training concepts
- LLM serving and inference
- Vector databases and embeddings
- Model checkpointing and recovery

---

## 💰 Cost Estimates

Each project includes estimated GCP costs. Budget approximately:
- **Phase 1:** $20-40 total
- **Phase 2:** $30-60 total
- **Phase 3:** $40-80 total
- **Phase 4:** $50-100 total
- **Phase 5:** $100-200 total (includes GPU for some projects)

**Total budget:** ~$240-480 for all 50 projects

*Costs vary by region, resource choices, and how long you leave resources running.*

---

## 🛡️ Important Safety Notes

1. **Use Dedicated Project:** Always work in a GCP project separate from production
2. **Budget Alerts:** Set up GCP billing alerts to prevent surprise charges
3. **Cleanup:** Run cleanup scripts after each project
4. **No Real Production:** Never practice these incidents on production systems
5. **Destroy After Learning:** Delete GCP resources when finished with a project

---

## 🤝 Contributing & Feedback

- Found an error in a project? File an issue
- Have a better explanation? Submit a PR
- Incident to add? Create an issue for Phase 6+
- Feedback on learning structure? Open a discussion

---

## 📚 Reference Documents

Before starting, review:
- `TROUBLESHOOTING_MINDSET.md` - How to think about incidents
- `LINUX_FUNDAMENTALS.md` - Core concepts you'll need
- `AI_INFRASTRUCTURE_GLOSSARY.md` - AI/ML terminology
- `INTERVIEW_PREP_GUIDE.md` - Interview question bank

---

## 🎯 Success Criteria

By project 50, you will be able to:

✅ Troubleshoot production incidents systematically  
✅ Read and analyze logs, metrics, and system state  
✅ Understand Linux kernel, process, networking behavior  
✅ Make architecture and performance decisions  
✅ Communicate clearly with teams during incidents  
✅ Design for observability and resilience  
✅ Operate AI infrastructure confidently  
✅ Explain incidents in interviews with depth  
✅ Show portfolio projects demonstrating expertise  

---

## 📞 Support

- **Questions?** Check the project FAQ sections
- **Stuck?** Review the investigation methodology
- **Need help?** Check if another project covers similar concepts
- **Not understanding?** Go back to fundamentals docs

 

