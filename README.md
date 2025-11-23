# 🌈 **Cloud-Powered Linux Admin Project on AWS**

### 🚀 *A Full Production-Style Deployment by Abdullrahman Eissa*

---

## 🌟 **Overview**

This project is a complete, real-world **Linux System Administration + Cloud Infrastructure** setup built on **AWS EC2**, with:

* A secure **Ubuntu EC2 instance**.
* An attached **IAM Role** with S3 Full Access (no credentials stored on the server).
* Installed & configured **Docker**.
* A production-ready **Dockerfile** running an Nginx website.
* **Netdata Monitoring** accessible globally for remote troubleshooting.
* **Automated Cloud Backups** of the Dockerfile to S3 every night at *3:00 AM* via cronjob.

This repository documents every moving part of the system in a clean, structured, and colorful way.

---

# 🏗️ **Architecture Diagram**

```
                  ┌────────────────────────┐
                  │        User            │
                  └───────────┬────────────┘
                              │
                              ▼
                ┌─────────────────────────┐
                │     Nginx (Docker)      │
                └───────────┬─────────────┘
                            │
                            ▼
               ┌──────────────────────────┐
               │       Ubuntu EC2         │
               └───────────┬──────────────┘
                           │
            ┌──────────────┴──────────────┐
            ▼                             ▼
 ┌─────────────────────┐       ┌───────────────────────────┐
 │ IAM Role (S3 Access)│       │   Netdata Monitoring       │
 └───────────┬─────────┘       └───────────────────────────┘
             │
             ▼
 ┌─────────────────────────────┐
 │     Amazon S3 (Backups)     │
 └─────────────────────────────┘
```

---

# ☁️ **1. EC2 Instance Setup**

### ✔ Ubuntu 22.04 LTS

### ✔ Security Group

* **HTTP (80)** → Allow
* **SSH (22)** → Allow
* **Netdata (19999)** → Allow

### ✔ IAM Role

Role: **EC2-S3FullAccess**
Policy: **AmazonS3FullAccess**
Attached via: *EC2 → Actions → Security → Modify IAM Role*

Your instance now has secure, credential-free access to your S3 bucket.

---

# 🐳 **2. Docker Installation**

Installed Docker using:

```
sudo apt update
sudo apt install docker.io -y
sudo systemctl enable docker --now
```

### ✔ Dockerfile for Your Website

* Located at: `/home/ubuntu/project/html/Dockerfile`
* Uses **Nginx** to serve your website files.
* Built & run using:

```
docker build -t mysite .
docker run -d -p 80:80 mysite
```

Your website now lives inside a container — clean, isolated, and production-grade.

---

# 📡 **3. Global Monitoring with Netdata**

Installed Netdata to monitor:

* CPU
* RAM
* Disk I/O
* Network
* System logs
* Docker performance

Accessible globally:

```
http://YOUR_EC2_PUBLIC_IP:19999
```

This turns your EC2 into a remotely manageable server from anywhere on the planet.

---

# 🔐 **4. S3 Backup System**

### ✔ Backup Target

Only backing up your **Dockerfile**:

```
/home/ubuntu/project/html/Dockerfile
```

### ✔ The Script: `/home/ubuntu/backup.sh`

```bash
#!/bin/bash
SRC="/home/ubuntu/project/html/Dockerfile"
BUCKET="backupfordocker"
DATE=$(date +%Y-%m-%d)
DEST="dockerfile-$DATE.tar.gz"

# Create backup
tar -czf /tmp/$DEST $SRC

# Upload to S3
aws s3 cp /tmp/$DEST s3://$BUCKET/

rm /tmp/$DEST
```

Make executable:

```
chmod +x ~/backup.sh
```

---

# ⏰ **5. Cronjob (Daily automatic backup at 3 AM)**

Open cron:

```
crontab -e
```

Add:

```
0 3 * * * /home/ubuntu/backup.sh >> /home/ubuntu/backup.log 2>&1
```

Now your EC2 automatically uploads a fresh Dockerfile backup every night.

---

# 🖼️ **6. Screenshots (Project Proof)**

Add your screenshots here when committing:

```
assets/
  01-ec2-console.png
  02-iam-role.png
  03-dockerfile.png
  04-netdata.png
  05-s3-backup.png
  ...
```

---

# 🧩 **7. What This Project Demonstrates**

### ✔ Linux Administration

### ✔ Docker Deployment

### ✔ Cloud Architecture

### ✔ Monitoring & Troubleshooting

### ✔ Automation & Cronjobs

### ✔ S3 Backups

### ✔ IAM Security Best Practices

This is the kind of project that makes employers say:
👉 *"This guy is ready to manage real servers."*

---

# 🎉 **8. Future Improvements**

* Add CloudWatch metrics
* Route53 domain + HTTPS
* Multi-container Docker Compose
* Automatic restore script
* Off-site backup redundancy

---

# ❤️ **9. Author**

**Abdullrahman Eissa** — Linux Systems Administrator (LFCS)

---
