# Section 2: Lab Setup

## GCP Infrastructure Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                     GCP Project (us-central1)               │
│                                                              │
│  ┌────────────────────────────────────────────────────────┐ │
│  │              VPC: linux-incidents-vpc                  │ │
│  │              Subnet: 10.0.0.0/24                       │ │
│  │                                                         │ │
│  │  ┌───────────────────────────────────────────────────┐ │ │
│  │  │  Compute Engine VM: project-01-nginx             │ │ │
│  │  │  ┌─────────────────────────────────────────────┐ │ │ │
│  │  │  │                                             │ │ │ │
│  │  │  │  Machine Type: e2-micro (2 vCPU, 1 GB RAM)│ │ │ │
│  │  │  │  OS: Ubuntu 22.04 LTS                      │ │ │ │
│  │  │  │  Disk: 20 GB Standard Persistent Disk      │ │ │ │
│  │  │  │  Internal IP: 10.0.0.2                     │ │ │ │
│  │  │  │  External IP: 34.x.x.x (assigned)          │ │ │ │
│  │  │  │                                             │ │ │ │
│  │  │  │  Pre-installed:                             │ │ │ │
│  │  │  │  ✓ Nginx web server                        │ │ │ │
│  │  │  │  ✓ curl for testing                        │ │ │ │
│  │  │  │  ✓ vim/nano for editing                    │ │ │ │
│  │  │  │                                             │ │ │ │
│  │  │  └─────────────────────────────────────────────┘ │ │ │
│  │  │                                                   │ │ │
│  │  │  Firewall Rules:                                 │ │ │
│  │  │  - Allow SSH (port 22)                          │ │ │
│  │  │  - Allow HTTP (port 80)                         │ │ │
│  │  │  - Allow HTTPS (port 443)                       │ │ │
│  │  └───────────────────────────────────────────────────┘ │ │
│  │                                                         │ │
│  └────────────────────────────────────────────────────────┘ │
│                                                              │
│  Cloud Monitoring: Enabled for metrics collection            │
│  Cloud Logging: Enabled for centralized logs                │
│                                                              │
└──────────────────────────────────────────────────────────────┘
```

---

## Step-by-Step GCP Setup

### Step 1: Set Environment Variables

```bash
# Set variables for your project
export PROJECT_ID="your-project-id"  # Replace with your actual project
export REGION="us-central1"
export ZONE="us-central1-a"
export VM_NAME="project-01-nginx"
export IMAGE_FAMILY="ubuntu-2204-lts"
export IMAGE_PROJECT="ubuntu-os-cloud"
export MACHINE_TYPE="e2-micro"

# Verify
echo "Project: $PROJECT_ID"
echo "Zone: $ZONE"
echo "VM Name: $VM_NAME"
```

### Step 2: Set Default Region/Zone

```bash
# Configure gcloud defaults
gcloud config set project $PROJECT_ID
gcloud config set compute/region $REGION
gcloud config set compute/zone $ZONE

# Verify
gcloud config list
```

### Step 3: Create VPC Network

```bash
# Create custom VPC
gcloud compute networks create linux-incidents-vpc \
  --subnet-mode=custom \
  --project=$PROJECT_ID

# Create subnet
gcloud compute networks subnets create linux-incidents-subnet \
  --network=linux-incidents-vpc \
  --range=10.0.0.0/24 \
  --region=$REGION \
  --project=$PROJECT_ID

# Verify
gcloud compute networks list
gcloud compute networks subnets list
```

### Step 4: Create Firewall Rules

```bash
# Allow SSH
gcloud compute firewall-rules create linux-incidents-allow-ssh \
  --network=linux-incidents-vpc \
  --allow=tcp:22 \
  --source-ranges=0.0.0.0/0 \
  --project=$PROJECT_ID

# Allow HTTP
gcloud compute firewall-rules create linux-incidents-allow-http \
  --network=linux-incidents-vpc \
  --allow=tcp:80 \
  --source-ranges=0.0.0.0/0 \
  --project=$PROJECT_ID

# Allow HTTPS
gcloud compute firewall-rules create linux-incidents-allow-https \
  --network=linux-incidents-vpc \
  --allow=tcp:443 \
  --source-ranges=0.0.0.0/0 \
  --project=$PROJECT_ID

# Verify
gcloud compute firewall-rules list --filter="network:linux-incidents-vpc"
```

### Step 5: Create Compute Instance

```bash
# Create VM with startup script that installs Nginx
gcloud compute instances create $VM_NAME \
  --zone=$ZONE \
  --machine-type=$MACHINE_TYPE \
  --network-interface=network-tier=PREMIUM,subnet=linux-incidents-subnet \
  --image-family=$IMAGE_FAMILY \
  --image-project=$IMAGE_PROJECT \
  --boot-disk-size=20GB \
  --boot-disk-type=pd-standard \
  --metadata-from-file startup-script=startup.sh \
  --project=$PROJECT_ID

# If startup script not available, use inline script:
gcloud compute instances create $VM_NAME \
  --zone=$ZONE \
  --machine-type=$MACHINE_TYPE \
  --network-interface=network-tier=PREMIUM,subnet=linux-incidents-subnet \
  --image-family=$IMAGE_FAMILY \
  --image-project=$IMAGE_PROJECT \
  --boot-disk-size=20GB \
  --boot-disk-type=pd-standard \
  --metadata=startup-script='#!/bin/bash
apt-get update
apt-get install -y nginx curl vim
sudo systemctl start nginx
sudo systemctl enable nginx' \
  --project=$PROJECT_ID
```

### Step 6: Get VM Details

```bash
# Get external IP
gcloud compute instances describe $VM_NAME \
  --zone=$ZONE \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)'

# Store IP in variable
export EXTERNAL_IP=$(gcloud compute instances describe $VM_NAME \
  --zone=$ZONE \
  --format='get(networkInterfaces[0].accessConfigs[0].natIP)')

echo "External IP: $EXTERNAL_IP"
```

### Step 7: SSH to VM and Verify Nginx

```bash
# SSH to VM
ssh -i ~/.ssh/google_compute_engine ubuntu@$EXTERNAL_IP

# Once SSH'd into VM, verify Nginx is running
sudo systemctl status nginx

# Check if port 80 is listening
sudo ss -tlnp | grep :80

# Test Nginx locally
curl http://localhost

# Exit SSH
exit
```

### Step 8: Enable Monitoring (Optional but Recommended)

```bash
# Enable Cloud Monitoring agent (for later projects)
gcloud compute instances create-with-monitoring-enabled $VM_NAME \
  --zone=$ZONE

# Or enable on existing instance:
gcloud compute ssh $VM_NAME \
  --zone=$ZONE \
  --command="curl -sSO https://dl.google.com/cloudagents/add-google-cloud-ops-agent-repo.sh
  sudo bash add-google-cloud-ops-agent-repo.sh --also-install"
```

---

## Verification Checklist

Before proceeding, verify:

```bash
# ✓ VM created and running
gcloud compute instances list --filter="name:$VM_NAME"

# ✓ VM has external IP
gcloud compute instances describe $VM_NAME --zone=$ZONE | grep -A3 accessConfigs

# ✓ SSH access works
ssh -i ~/.ssh/google_compute_engine ubuntu@$EXTERNAL_IP "echo 'SSH works'"

# ✓ Nginx is installed
ssh -i ~/.ssh/google_compute_engine ubuntu@$EXTERNAL_IP "sudo systemctl status nginx"

# ✓ Nginx responding on port 80
ssh -i ~/.ssh/google_compute_engine ubuntu@$EXTERNAL_IP "curl -s http://localhost | head -n 5"

# ✓ Firewall rules allow traffic
gcloud compute firewall-rules list --filter="network:linux-incidents-vpc"
```

---

## Expected Output

### VM Creation Success
```
Created [https://www.googleapis.com/compute/v1/projects/my-project/zones/us-central1-a/instances/project-01-nginx].
NAME           ZONE           MACHINE_TYPE  PREEMPTIBLE  INTERNAL_IP  EXTERNAL_IP    STATUS
project-01-nginx us-central1-a e2-micro                   10.0.0.2     34.x.x.x       RUNNING
```

### SSH Connection Test
```
$ ssh ubuntu@34.x.x.x
The authenticity of host '34.x.x.x (34.x.x.x)' can't be established.
ECDSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no)? yes
ubuntu@project-01-nginx:~$
```

### Nginx Status
```
$ sudo systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Sun 2026-06-22 14:30:45 UTC; 5min ago
```

### Port Listening
```
$ sudo ss -tlnp | grep :80
LISTEN    0      511          0.0.0.0:80        0.0.0.0:*    users:(("nginx",pid=1234,fd=6),("nginx",pid=1235,fd=6))
```

---

## Cost Estimate

| Resource | Type | Monthly Cost | 4-Hour Cost |
|----------|------|--------------|-------------|
| e2-micro VM | Compute Engine | $7 | ~$0.06 |
| 20 GB Disk | Standard PD | $0.80 | ~$0.01 |
| External IP | Network | $0.20 (if static) | ~$0 (dynamic) |
| Data transfer | Egress | $0.12/GB | ~$0.01 |
| **Total** | | | **$0.08** |

**Note:** e2-micro is eligible for GCP free tier (1 per month for 12 months).

---

## Cleanup After Lab

When finished with this project:

```bash
# Delete VM
gcloud compute instances delete $VM_NAME --zone=$ZONE --quiet

# Delete disks (if not auto-deleted)
gcloud compute disks list
gcloud compute disks delete <disk-name> --zone=$ZONE --quiet

# Delete firewall rules
gcloud compute firewall-rules delete linux-incidents-allow-ssh --quiet
gcloud compute firewall-rules delete linux-incidents-allow-http --quiet
gcloud compute firewall-rules delete linux-incidents-allow-https --quiet

# Delete subnet
gcloud compute networks subnets delete linux-incidents-subnet --region=$REGION --quiet

# Delete VPC
gcloud compute networks delete linux-incidents-vpc --quiet

# Verify cleanup
gcloud compute instances list
gcloud compute networks list
```

---

## Troubleshooting

### VM Doesn't Start
```bash
# Check VM status
gcloud compute instances list

# Check recent operations
gcloud compute operations list --limit=10

# Describe instance for details
gcloud compute instances describe $VM_NAME --zone=$ZONE
```

### Cannot SSH
```bash
# Verify SSH key exists
ls -la ~/.ssh/google_compute_engine

# Check firewall rule
gcloud compute firewall-rules describe linux-incidents-allow-ssh

# Try with verbose output
ssh -vvv -i ~/.ssh/google_compute_engine ubuntu@$EXTERNAL_IP
```

### Nginx Not Installed
```bash
# SSH to VM
ssh ubuntu@$EXTERNAL_IP

# Manually install
sudo apt-get update
sudo apt-get install -y nginx

# Start service
sudo systemctl start nginx
```

---

## Next Step

👉 **Go to [Section 3: Create Incident](03-CREATE_INCIDENT.md)**

Now we'll intentionally break Nginx to simulate the production failure.
