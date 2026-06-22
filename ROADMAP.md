# Linux Incidents Learning Program - Complete 50-Project Roadmap

## All 50 Incidents

| # | Incident Title | Difficulty | Linux Topics | Why This Happens | AI Relevance |
|---|---|---|---|---|---|
| 1 | Web Server Down Because Nginx Service Failed | Beginner | systemctl, services, ports, logs | Service crashes after bad config changes, package upgrades, or restarts | AI dashboards and inference APIs run behind Nginx |
| 2 | Website Unreachable Due To Firewall Misconfiguration | Beginner | Firewall rules, ports, networking | Firewall blocks valid traffic, separating app failure from network | AI inference endpoints need controlled access |
| 3 | Application Fails Because Disk Is Full | Beginner | Disk usage, df, du, cleanup | Logs, cache, uploads, temp files fill disks | AI workloads produce huge logs, checkpoints, datasets |
| 4 | SSH Login Fails After Permission Mistake | Beginner | SSH, permissions, ownership | Broken permissions on .ssh or home directories | AI infrastructure depends on secure GPU access |
| 5 | Cron Job Did Not Run Backup Script | Beginner | cron, environment variables, scripts | Cron has different environment from interactive shell | Scheduled backups of models, datasets, snapshots |
| 6 | Application Cannot Write Because File Ownership Changed | Beginner | Ownership, permissions, service users | Deployments change ownership of app directories | Model servers need write access to cache/index |
| 7 | High CPU Usage From A Runaway Process | Beginner | top, ps, kill, signals | Bugs, infinite loops, stuck workers consume CPU | Inference workers can spin under bad requests |
| 8 | Server Slow Because Memory Is Exhausted | Beginner | RAM, swap, free, vmstat | Memory leaks, oversized apps, too many processes | Tokenizers, model loaders, vector indexes exhaust RAM |
| 9 | DNS Resolution Failure Breaks Application Connectivity | Beginner | DNS, resolvers, dig, nslookup | Apps fail because names cannot resolve | AI services connect to storage, APIs, registries by name |
| 10 | Log Files Missing After Incorrect Log Rotation | Beginner | Logs, logrotate, retention | Logs disappear or stop rotating from bad rules | AI workloads generate massive logs |
| 11 | Package Upgrade Breaks Running Application | Junior Admin | apt, package versions, dependencies | Packages upgraded without testing dependency impact | CUDA, Python runtime, driver compatibility |
| 12 | VM Cannot Boot Because /etc/fstab Is Wrong | Junior Admin | Boot process, mounts, fstab | Wrong disk UUIDs or mount options stop boot | AI nodes mount large model/data disks |
| 13 | Application Fails Because Environment Variables Are Missing | Junior Admin | systemd, environment, process env | Apps work manually but fail as services | AI services depend on model paths, API keys, CUDA |
| 14 | Website Returns 502 Bad Gateway From Reverse Proxy | Junior Admin | Nginx, upstreams, ports | Reverse proxies return 502 when backends down | AI inference APIs sit behind reverse proxies |
| 15 | Server Cannot Reach Internet Because Default Route Is Missing | Junior Admin | Routing, ip route, VPC | Cloud VMs lose outbound connectivity | AI nodes need internet for packages, model downloads |
| 16 | User Cannot Run Deployment Script Due To sudo Policy | Junior Admin | sudoers, privilege escalation | Deployment users lack permissions or have unsafe access | DevOps automation users for model deployment |
| 17 | Time Synchronization Failure - NTP Issues | Junior Admin | NTP, time sync, timedatectl | Servers with incorrect clocks fail TLS, auth, coordination | Distributed training needs synchronized time |
| 18 | Persistent Disk Mounted But Application Uses Wrong Directory | Junior Admin | Mount points, storage layout | Apps write to root disk instead of attached storage | AI workloads need large disks for datasets, models |
| 19 | Monitoring Agent Not Sending Metrics | Junior Admin | Monitoring agent, Cloud Monitoring | Agent fails due to firewall, IAM, or config | AI infrastructure needs GPU, CPU, latency metrics |
| 20 | Application Logs Not Appearing In Cloud Logging | Junior Admin | Logging agents, journald, permissions | Logs fail to appear due to permissions or wrong paths | AI incidents require logs from servers and workers |
| 21 | Load Balancer Shows Backend Unhealthy | Intermediate | LB health checks, firewall, ports | Backends healthy locally but fail cloud health checks | AI inference APIs need reliable health checks |
| 22 | Application Timeout Caused By Slow Disk I/O | Intermediate | iostat, disk latency, IOPS | Disk throughput exhausted, services become slow | Vector DBs and training are I/O-heavy |
| 23 | Too Many Open Files Causes Application Failure | Intermediate | File descriptors, ulimit, limits | High-traffic services hit FD limits | AI APIs, streaming inference open many files |
| 24 | Port Conflict Prevents Service Startup | Intermediate | Sockets, ss, process binding | Another process owns the required port | Multiple AI services on same GPU VM |
| 25 | Kernel OOM Killer Terminates Critical Process | Intermediate | OOM killer, cgroups, memory pressure | Under memory pressure, Linux kills processes | AI workloads commonly hit RAM limits |
| 26 | TLS Certificate Expiry Breaks HTTPS Endpoint | Intermediate | TLS, certificates, Nginx | Expired certificates are common outages | AI APIs and dashboards depend on TLS |
| 27 | Bad DNS Record Sends Traffic To Wrong Server | Intermediate | Cloud DNS, A records, TTL | DNS mistakes send users to wrong IPs | AI platforms route traffic to wrong clusters |
| 28 | Process Zombie Accumulation From Broken Parent Process | Intermediate | Process lifecycle, zombies, signals | Zombie processes appear when parents don't reap exits | AI batch workers spawn many subprocesses |
| 29 | SSH Brute Force Attack Detected On Public VM | Intermediate | SSH security, logs, fail2ban | Public VMs constantly scanned | GPU VMs are expensive attack targets |
| 30 | Application Fails After Secret File Permission Exposure | Intermediate | Permissions, secrets, audit | Secrets readable by unintended users | API keys, tokens, dataset credentials leaked |
| 31 | TCP Connection Exhaustion Under High Traffic | Advanced | TCP states, TIME_WAIT, ephemeral ports | High request volume exhausts sockets/ports | AI inference APIs under burst traffic |
| 32 | SYN Flood Or Connection Storm Degrades Service | Advanced | TCP backlog, DDoS, kernel | Traffic spikes overwhelm connection queues | Public AI APIs receive bot traffic |
| 33 | System Becomes Slow Due To Swap Thrashing | Advanced | Swap, paging, vmstat | Memory pressure causes heavy swapping | AI workloads memory-intensive, swap makes slow |
| 34 | Filesystem Corruption After Unsafe Disk Detach | Advanced | ext4, fsck, mount errors | Improper shutdowns corrupt filesystems | AI datasets, checkpoints, indexes corrupted |
| 35 | Inode Exhaustion Despite Free Disk Space | Advanced | inodes, df -i, small files | Systems fail creating files, inodes exhausted | AI pipelines generate millions of small files |
| 36 | Latency Spike Caused By CPU Steal Or Noisy Neighbor | Advanced | CPU steal, virtualization, cloud | Cloud VMs suffer performance from host contention | Unpredictable latency from noisy neighbors |
| 37 | IAM Permission Change Breaks VM Access To Cloud Resources | Advanced | GCP IAM, service accounts | Applications fail when service account permissions change | AI workloads need access to buckets, registries |
| 38 | Broken Startup Script Leaves Fleet In Half-Configured State | Advanced | startup scripts, cloud-init, idempotency | VM automation fails from non-idempotent scripts | AI clusters auto-create GPU nodes |
| 39 | Observability Blind Spot During Incident | Advanced | metrics, logs, alerts, SLOs | Teams lack right metrics/logs during incidents | AI systems need latency, tokens/sec, GPU metrics |
| 40 | Security Patch Reboot Causes Unexpected Service Downtime | Advanced | patching, reboot planning, HA | Patches/reboots expose manually started services | GPU nodes need safe patching |
| 41 | Regional Outage Simulation With Multi-Zone Failover Failure | Senior/SRE | Zones, load balancing, HA | Companies fail over poorly, assumptions about redundancy | AI inference needs zone-aware serving |
| 42 | Autoscaling Fails During Traffic Spike | Senior/SRE | Autoscaling, metrics, scaling | Autoscaling fails from wrong metrics or high startup time | AI inference autoscaling hard due to slow loading |
| 43 | GPU Worker Node Unusable After Driver/CUDA Mismatch | Senior/SRE | Kernel modules, drivers, CUDA | GPU workloads break on version misalignment | Core AI infrastructure dependency |
| 44 | Model Serving Latency Caused By CPU Bottleneck, Not GPU | Senior/SRE | CPU profiling, process scheduling | Teams blame GPUs while CPU is bottleneck | Tokenization, routing, batching bottleneck before GPU |
| 45 | Vector Database Slow Because Disk And Memory Were Mis-Sized | Senior/SRE | Memory mapping, disk I/O, sizing | Databases slow when working sets exceed memory | Vector DB sizing for RAG systems |
| 46 | Distributed Training Job Fails Due To Network MTU Mismatch | Senior/SRE | MTU, networking, NCCL | Network mismatches create intermittent failures | Distributed training communication breaks |
| 47 | Checkpoint Upload Failure Causes Training Progress Loss | Senior/SRE | Storage reliability, retries, cloud | Long-running jobs lose progress from checkpoint failures | Expensive GPU compute wasted |
| 48 | Inference Service Brownout Under Partial Dependency Failure | Senior/SRE | Graceful degradation, timeouts | Services degrade when dependencies slow but not down | Bad retries amplify outages |
| 49 | Cost Incident: Runaway Logs, Disks, And Idle GPU Resources | Senior/SRE | Cost observability, lifecycle | Uncontrolled cloud spend burns budget | Idle GPUs, huge logs, stale checkpoints expensive |
| 50 | Full Production-Style AI Platform Outage: Multi-Layer RCA | Senior/SRE | Multi-layer systems, cascading failures | Real incidents involve cascading failures across layers | AI inference platform complete outage |

---

## Phase Breakdown

### Phase 1: Beginner (Projects 1-10)
Focus: Linux fundamentals, basic troubleshooting, system basics

### Phase 2: Junior Linux Admin (Projects 11-20)
Focus: Advanced troubleshooting, system configuration, user management, security basics

### Phase 3: Intermediate (Projects 21-30)
Focus: Performance tuning, advanced networking, security hardening, kernel internals

### Phase 4: Advanced (Projects 31-40)
Focus: Kernel tuning, advanced security, observability, production hardening, complex architecture

### Phase 5: Senior Engineer / SRE (Projects 41-50)
Focus: AI infrastructure, distributed systems, chaos engineering, culture & processes

---

## Next Steps

Start with **Phase-1-Beginner/Project-01-Nginx-Service-Failed/**
