# Learning Phases Overview

## Phase 1: Beginner (Projects 1-10)

**Duration:** 1-2 weeks  
**Assumed Knowledge:** Basic Linux, can SSH to a server, know what a process is

### Learning Objectives
- Understand Linux filesystem and disk management
- Master service management with systemd
- Read and interpret system logs
- Use basic monitoring commands (top, ps, df, etc.)
- Understand Linux permissions and ownership
- Troubleshoot connectivity and DNS issues
- Know how to cleanly restart services
- Understand backup and log rotation

### Key Skills Developed
- Using `systemctl` to manage services
- Reading `journalctl` and log files
- Disk analysis with `df` and `du`
- Process monitoring with `top` and `ps`
- Permission and ownership management
- Basic networking troubleshooting
- Understanding process lifecycle

### Why This Phase Matters
These are the most common production issues. 80% of alerts you'll handle in your first year will be from Phase 1 concepts.

---

## Phase 2: Junior Linux Admin (Projects 11-20)

**Duration:** 2-3 weeks  
**Prerequisites:** Complete Phase 1

### Learning Objectives
- Handle package management and system upgrades
- Troubleshoot boot failures
- Understand environment variables and process configuration
- Debug reverse proxy and load balancer issues
- Manage routing and cloud networking
- Understand privilege escalation (sudo)
- Manage system time and synchronization
- Understand storage architecture and mounting
- Set up and troubleshoot monitoring
- Understand observability infrastructure

### Key Skills Developed
- Package management (`apt`, `yum`)
- Boot process and GRUB
- /etc/fstab and mount management
- Cloud VPC and routing
- Health checks and service discovery
- Environment variable management
- Time synchronization (NTP, chrony)
- Logging infrastructure

### Why This Phase Matters
You move from "understanding the system" to "managing the system". Junior admins spend most time here.

---

## Phase 3: Intermediate (Projects 21-30)

**Duration:** 3-4 weeks  
**Prerequisites:** Complete Phase 2

### Learning Objectives
- Analyze performance bottlenecks (CPU, memory, disk, network)
- Understand resource limits and cgroups
- Debug TLS/certificate issues
- Analyze network behavior at TCP level
- Understand filesystem behavior deeply
- Debug security issues and access control
- Use advanced monitoring tools
- Design for security and compliance

### Key Skills Developed
- Performance analysis tools (iostat, vmstat, tcpdump)
- TCP state analysis
- File descriptor management
- TLS certificate debugging
- DNS deep-dive
- Memory pressure analysis
- Process state debugging
- Security auditing

### Why This Phase Matters
You become dangerous with your infrastructure. You can solve 90% of production issues without escalation.

---

## Phase 4: Advanced (Projects 31-40)

**Duration:** 4-5 weeks  
**Prerequisites:** Complete Phase 3

### Learning Objectives
- Understand kernel internals
- Troubleshoot complex networking
- Analyze system-level performance
- Understand security vulnerabilities
- Debug distributed system failures
- Manage infrastructure automation
- Recognize observability gaps
- Plan for high availability

### Key Skills Developed
- Kernel module management
- eBPF and advanced tracing
- Advanced firewall rules
- Load balancer troubleshooting
- Automation and infrastructure-as-code
- Observability design
- Incident investigation

### Why This Phase Matters
You're now at senior engineer level. You make architectural decisions and mentor others.

---

## Phase 5: Senior/SRE (Projects 41-50)

**Duration:** 5-6 weeks  
**Prerequisites:** Complete Phase 4, production experience strongly recommended

### Learning Objectives
- Design and troubleshoot production AI systems
- Understand distributed training and inference
- Manage cluster infrastructure
- Conduct incident response and RCA
- Design for resilience and chaos
- Understand business and cost implications
- Lead incident response
- Build observability systems

### Key Skills Developed
- GPU infrastructure management
- Distributed system troubleshooting
- LLM serving and inference
- Vector database operations
- Incident command
- Postmortem writing
- Resilience testing
- Cost optimization

### Why This Phase Matters
You're now an SRE/infrastructure expert. You design systems for reliability, not just fix broken ones.

---

## Time Investment

**Total Time:** 15-20 weeks (3-5 months) for all 50 projects

**Weekly Breakdown:**
- Phase 1: 10 hours/week
- Phase 2: 12 hours/week
- Phase 3: 15 hours/week
- Phase 4: 15 hours/week
- Phase 5: 20 hours/week (includes GPU labs)

**Per Project:** 4-8 hours depending on phase

---

## Success Metrics

### Phase 1 Success
- Can identify service issues in < 2 minutes
- Can check disk space, memory, CPU automatically
- Can read systemd logs and understand failures
- Can restart failed services safely

### Phase 2 Success
- Can troubleshoot package issues
- Understand cloud networking concepts
- Can debug environment issues
- Know observability basics

### Phase 3 Success
- Can identify performance bottlenecks
- Understand TLS/security issues
- Can trace network problems
- Know advanced monitoring

### Phase 4 Success
- Can design production systems
- Understand kernel behavior
- Can investigate complex failures
- Know when to optimize vs. scale

### Phase 5 Success
- Can operate AI infrastructure confidently
- Lead incident response
- Design for resilience
- Mentor junior engineers

---

## Portfolio Building

Each project should result in:
- Professional README on GitHub
- Architecture diagrams
- Investigation documentation
- Lessons learned
- Interview-ready explanations

By project 50, you'll have 50 portfolio pieces demonstrating expertise across all domains.
