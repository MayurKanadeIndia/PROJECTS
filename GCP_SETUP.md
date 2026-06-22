# Google Cloud Platform Setup Guide

## Prerequisites

### Required
- GCP account with billing enabled
- gcloud CLI installed and configured
- Basic familiarity with GCP console
- $500+ budget for all 50 projects

### Recommended
- terraform or gcloud scripts knowledge
- SSH key pair generated
- VPC and networking understanding

---

## Initial Setup

### 1. Create a Dedicated GCP Project

```bash
# Set project name
PROJECT_NAME="linux-incidents-learning"
PROJECT_ID="linux-incidents-$(date +%s)"

# Create project
gcloud projects create $PROJECT_ID --name=$PROJECT_NAME

# Set as current project
gcloud config set project $PROJECT_ID

# Enable required APIs
gcloud services enable compute.googleapis.com
gcloud services enable monitoring.googleapis.com
gcloud services enable logging.googleapis.com
gcloud services enable cloudresourcemanager.googleapis.com
```

### 2. Set Billing Budget

```bash
# Enable billing alerts (important!)
gcloud billing budgets create \
  --billing-account=<YOUR_BILLING_ACCOUNT> \
  --display-name="Linux Incidents Learning" \
  --budget-amount=500
```

### 3. Configure Default Region/Zone

```bash
# Set default region (choose closest to you)
gcloud config set compute/region us-central1
gcloud config set compute/zone us-central1-a
```

---

## Per-Project Infrastructure

### Standard VM Specs (Phase 1-3)

```
Machine Type: e2-standard-2
CPU: 2 vCPU
Memory: 8 GB
Disk: 50 GB standard persistent disk
OS: Ubuntu 22.04 LTS
Cost per month: ~$30
```

### Intermediate VM Specs (Phase 4)

```
Machine Type: n2-standard-4
CPU: 4 vCPU
Memory: 16 GB
Disk: 100 GB standard persistent disk
OS: Ubuntu 22.04 LTS
Cost per month: ~$90
```

### Advanced VM Specs (Phase 5)

```
Machine Type: n2-standard-8
CPU: 8 vCPU
Memory: 32 GB
Disk: 200 GB SSD persistent disk
GPU: 1x NVIDIA T4 (for GPU projects)
OS: Ubuntu 22.04 LTS
Cost per month: ~$400+ (with GPU)
```

---

## Cost Breakdown

| Phase | Projects | Avg Cost/Project | Phase Total | Notes |
|-------|----------|-----------------|-------------|-------|
| 1 | 10 | $2-4 | $20-40 | Small VMs, short duration |
| 2 | 10 | $3-6 | $30-60 | Slightly larger VMs |
| 3 | 10 | $4-8 | $40-80 | More disk I/O testing |
| 4 | 10 | $5-10 | $50-100 | Larger VMs, longer investigation |
| 5 | 10 | $10-20 | $100-200 | GPU projects, larger disks |
| **Total** | **50** | **$5 avg** | **$240-480** | Estimated for full program |

---

## Cost Optimization Tips

### 1. Clean Up After Each Project

```bash
# Delete VM when done
gcloud compute instances delete <instance-name> --zone=us-central1-a

# Delete persistent disks
gcloud compute disks delete <disk-name> --zone=us-central1-a

# Delete snapshots
gcloud compute snapshots delete <snapshot-name>
```

### 2. Use Preemptible VMs for Phase 1-3

```bash
gcloud compute instances create project-01 \
  --preemptible \
  --image-family=ubuntu-2204-lts \
  --image-project=ubuntu-os-cloud
```

**Cost Savings:** 60-70% cheaper, but can be interrupted
**Best for:** Learning projects where interruption acceptable

### 3. Use Standard Disks, Not SSD

**Cost Difference:** SSD ~3x more expensive
**Best for:** Phase 1-4 (adequate for learning)

### 4. Delete Snapshots Regularly

```bash
# List snapshots
gcloud compute snapshots list

# Delete old snapshots
gcloud compute snapshots delete <old-snapshot>
```

### 5. Monitor Costs

```bash
# View current spending
gcloud billing accounts list

# View costs by resource
gcloud compute project-info describe --project=$PROJECT_ID
```

---

## Networking Setup

### Create VPC for Projects

```bash
# Create VPC
gcloud compute networks create linux-incidents-vpc \
  --subnet-mode=custom

# Create subnet
gcloud compute networks subnets create linux-incidents-subnet \
  --network=linux-incidents-vpc \
  --range=10.0.0.0/24 \
  --region=us-central1

# Create firewall rule (allow SSH)
gcloud compute firewall-rules create linux-incidents-allow-ssh \
  --network=linux-incidents-vpc \
  --allow=tcp:22 \
  --source-ranges=0.0.0.0/0
```

### Create Additional Firewall Rules

```bash
# Allow HTTP/HTTPS
gcloud compute firewall-rules create linux-incidents-allow-http \
  --network=linux-incidents-vpc \
  --allow=tcp:80,tcp:443 \
  --source-ranges=0.0.0.0/0

# Allow internal traffic
gcloud compute firewall-rules create linux-incidents-allow-internal \
  --network=linux-incidents-vpc \
  --allow=tcp:0-65535,udp:0-65535 \
  --source-ranges=10.0.0.0/24
```

---

## Monitoring & Logging Setup

### Enable Cloud Monitoring

```bash
gcloud services enable monitoring.googleapis.com

# Verify
gcloud monitoring dashboards list
```

### Enable Cloud Logging

```bash
gcloud services enable logging.googleapis.com

# View logs
gcloud logging read "severity>=WARNING" --limit 50
```

---

## SSH Key Setup

### Generate SSH Key Pair

```bash
ssh-keygen -t rsa -b 4096 -f ~/.ssh/linux-incidents-key

# Set permissions
chmod 700 ~/.ssh
chmod 600 ~/.ssh/linux-incidents-key
chmod 644 ~/.ssh/linux-incidents-key.pub
```

### Add to GCP Project

```bash
# Method 1: gcloud CLI
gcloud compute os-login ssh-keys add \
  --key-file=~/.ssh/linux-incidents-key.pub

# Method 2: GCP Console
# Go to Compute Engine > Metadata > SSH Keys > Add Item
```

---

## Verification Checklist

- [ ] GCP project created
- [ ] Billing enabled with budget alert
- [ ] Compute Engine API enabled
- [ ] Monitoring API enabled
- [ ] Logging API enabled
- [ ] VPC created
- [ ] Firewall rules configured
- [ ] SSH keys uploaded
- [ ] Can SSH to test VM
- [ ] gcloud CLI working

---

## Troubleshooting

### Cannot SSH to VM

```bash
# Check instance is running
gcloud compute instances list

# Check firewall allows SSH
gcloud compute firewall-rules list --filter="name:allow-ssh"

# Try verbose SSH
ssh -vv -i ~/.ssh/linux-incidents-key <user>@<instance-ip>
```

### High Unexpected Costs

```bash
# List all instances
gcloud compute instances list --project=$PROJECT_ID

# List all disks
gcloud compute disks list --project=$PROJECT_ID

# Delete unused resources
gcloud compute instances delete <name>
gcloud compute disks delete <name>
```

### Cannot Connect to Internet

```bash
# Check if Cloud NAT is needed
# For outbound internet, VMs need:
# 1. External IP, OR
# 2. Cloud NAT configured

# Create Cloud NAT (if needed)
gcloud compute routers create linux-incidents-router \
  --network=linux-incidents-vpc \
  --region=us-central1

gcloud compute routers nats create linux-incidents-nat \
  --router=linux-incidents-router \
  --region=us-central1 \
  --nat-all-subnet-ip-ranges \
  --auto-allocate-nat-external-ips
```

---

## Next Steps

1. Complete this setup
2. Create a test VM to verify connectivity
3. Read Phase 1 project overview
4. Start Project 1: Nginx Service Failed

---

**Cost Tracking:** Monitor your project regularly to avoid surprise bills!
