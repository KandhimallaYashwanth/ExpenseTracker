# 🚀 ExpenseTracker – DevOps Internship Deployment Project

This project was completed as part of a **DevOps Internship Task**.  
It demonstrates the full deployment workflow for a **React application** on a **Linux VPS**,  
featuring **CI/CD automation, domain management, HTTPS security, and zero-downtime releases**.

---

## 📦 Project Overview

**Project Name:** ExpenseTracker  
**Tech Stack:** React, Nginx, GitHub Actions, Certbot, GCP (Ubuntu VPS)  
**Deployment Type:** Automated (CI/CD)  
**Hosting Platform:** Google Cloud Platform (Compute Engine)  
**DNS Provider:** FreeDNS (afraid.org)

---

## 🌍 Live URLs

| Type | URL | Description |
|------|------|-------------|
| 🔒 Domain | [https://expensetracker.mooo.com](https://expensetracker.mooo.com) | Main live site |
| 🔒 Subdomain | [https://app.expensetracker.mooo.com](https://app.expensetracker.mooo.com) | Subdomain (mirrors main) |
| 🌐 IP Fallback | [http://34.93.136.44](http://34.93.136.44) | Direct VPS IP (non-secure fallback) |

> HTTPS certificates are configured via **Let’s Encrypt (Certbot)**  
> and auto-renew every 90 days.

---

## ⚙️ Architecture Summary

| Component | Description |
|------------|-------------|
| **Frontend Framework** | React |
| **Server** | Nginx (Reverse Proxy + Static Hosting) |
| **CI/CD** | GitHub Actions (Build → Deploy → Reload) |
| **Infrastructure** | GCP Ubuntu 22.04 VPS |
| **DNS & Domain** | FreeDNS (afraid.org) |
| **SSL/HTTPS** | Let’s Encrypt (Certbot) |
| **Zero Downtime** | Release folders with symlink switch |
| **Firewall** | `ufw allow 'Nginx Full'` |

---


## 🧩 Folder Structure (Server)

```bash
/var/www/reactapp/
├── releases/
│   ├── 1/
│   │   └── dist/                # First deployment build
│   ├── 2/
│   │   └── dist/                # Second build release
│   ├── 3/
│   │   └── dist/                # Third build release
│   ├── 4/
│   │   └── dist/                # Fourth build release
│   ├── 5/
│   │   └── dist/                # Latest (active) build served by Nginx
│   └── ...                      # Future deployments go here
│
├── current → releases/5         # Symbolic link pointing to latest release (zero downtime)
│
└── logs/
    ├── access.log               # Nginx access logs
    └── error.log                # Nginx error logs
```
- Each new deployment is uploaded as a new release (e.g., `releases/6/dist`)
- The `current` symlink points to the latest active version
- Nginx always serves from `/var/www/reactapp/current/dist`
- Swapping symlink → zero downtime 🎯


## 🔁 CI/CD Workflow Explanation

A **Continuous Integration / Continuous Deployment (CI/CD)** pipeline was implemented using **GitHub Actions**.

### ⚡ How the Pipeline Works

1. **Code Push** – Whenever new code is pushed to the `main` branch, the workflow is triggered.  
2. **Build Stage** – GitHub Actions installs dependencies and builds the React project.  
3. **Deployment Stage** – The built files are securely transferred to the VPS via SSH using GitHub Secrets.  
4. **Zero Downtime Switch** – The new version is uploaded to a release folder, and a symlink is updated to make the switch seamless.  
5. **Post Deployment** – Nginx reloads automatically, making the new version live instantly with **zero downtime**.

### 💡 CI/CD Benefits

- **Fully Automated** – Deployment happens on every code push.  
- **Consistent** – Reliable and repeatable process.  
- **Secure** – Uses encrypted GitHub Secrets for SSH access.  
- **Downtime-Free** – Live updates with no interruptions.

---

## 🔒 HTTPS Setup

Certbot automatically:

- Generates and installs SSL certificates  
- Configures Nginx for HTTPS and HTTP → HTTPS redirection  
- Sets up auto-renewal via cron/systemd (renewal every 60–90 days)

---

## 🧾 Task Completion Checklist

| # | Requirement | Status | Details |
|---|--------------|:------:|---------|
| 1 | Deploy any React app | ✅ | ExpenseTracker deployed successfully |
| 2 | Use GitHub Actions or equivalent CI/CD | ✅ | GitHub Actions automates build and deploy |
| 3 | Deploy on Linux VPS | ✅ | GCP Ubuntu 22.04 |
| 4 | Application publicly accessible | ✅ | Domain, subdomain, and IP |
| 5 | Add Domain/Subdomain | ✅ | FreeDNS (afraid.org) |
| 6 | Zero Downtime Deployment | ✅ | Versioned release + symlink switch |
| 7 | Enable HTTPS | ✅ | Certbot with auto-renewal |
| 8 | IP Fallback | ✅ | [http://34.93.136.44](http://34.93.136.44) (non-SSL fallback) |

---

## 📚 Verification Commands

| Purpose | Command | Expected Output |
|----------|----------|----------------|
| Check Nginx config | `sudo nginx -t` | syntax is ok |
| Reload Nginx | `sudo systemctl reload nginx` | Reload successful |
| Check SSL Certs | `sudo certbot certificates` | Shows valid certificate |
| Test Renewal | `sudo certbot renew --dry-run` | All renewals succeeded |
| Ping Domain | `ping expensetracker.mooo.com` | Resolves to 34.93.136.44 |

---
## 🧠 Deployment Flow Summary
Developer Pushes Code → GitHub Actions → Build React App → Upload to VPS →
Switch Symlink → Reload Nginx → Live Update (Zero Downtime)


---

## 🏁 Final Summary

The **ExpenseTracker** React app is deployed on an Ubuntu VPS (Google Cloud) using **Nginx** as a reverse proxy.  
A **GitHub Actions CI/CD pipeline** automates the build and deployment process, ensuring **zero downtime releases**.  
The app is publicly accessible via both **domain and subdomain**, with secure **HTTPS configuration** and a **public IP fallback**.

---

### 👨‍💻 Developed & Deployed by:
**Yashwanth Reddy Kandhimalla**  
*DevOps Internship Project – November 2025*

---
