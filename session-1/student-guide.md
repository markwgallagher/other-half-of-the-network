# student-guide.md

# The Other Half of the Network

## Session 1 — Standing Up the Stack

**Duration:** ~90 minutes
**Format:** Follow-along, hands-on

---

# Welcome

By the end of this session, you will:

* Provision a real cloud VM
* SSH into a public Linux server
* Install and verify a complete LAMP stack
* Serve a web page from your own public IP address
* Make your first commit to the shared course repository

This course is not a Linux certification prep class.

We are building real infrastructure while learning the operational thinking behind it.

---

# Pre-Session Checklist

Before class, make sure you have:

* Personal credit/debit card available (~$6 charge)
* SSH client installed

  * macOS/Linux: Terminal
  * Windows: Windows Terminal preferred
* Git installed locally
* Azure DevOps access confirmed

Verify Git:

```bash
git --version
```

Open these sites in your browser:

* digitalocean.com
* cloudflare.com

---

# 0. Welcome & Framing

## Why We’re Here

Traditional networking courses focus heavily on:

* routers
* switches
* protocols
* diagrams

This course focuses on the operational side students often never touch:

* Linux systems
* web servers
* DNS
* TLS
* cloud infrastructure
* deployment
* observability

The network is only half the picture.

This course covers the other half.

---

## Ground Rules

* Follow along on your own machine
* Falling behind is normal — ask questions immediately
* CLI only
* No cPanel or GUI deployment tools
* There are many valid ways to do everything shown here
* Questions are always welcome

---

## Cost Transparency

Today’s infrastructure cost:

* ~$6 DigitalOcean Droplet

Total estimated course cost:

* ~$10–15 depending on your domain registration

This infrastructure belongs to you.

You can:

* keep it running
* continue building on it
* destroy it after the course

---

# 1. DigitalOcean Account Setup

## Create Your Account

Create a DigitalOcean account using a personal email address.

Use personal infrastructure for this course — not employer-owned systems.

---

## Configure Billing Safety Limits

Before creating resources:

1. Add a payment method
2. Navigate to:

```text
Settings → Billing → Spending Limits
```

3. Set a spending alert at:

```text
$15/month
```

This protects against accidental charges.

---

## Why DigitalOcean?

DigitalOcean is intentionally simpler than AWS for a first infrastructure course.

Advantages:

* predictable pricing
* simpler UI
* faster onboarding
* easier operational visibility

The same concepts still transfer to:

* AWS
* Azure
* GCP
* VMware
* on-prem virtualization

---

# SSH Key Authentication

## Why SSH Keys Matter

SSH key authentication removes password-based remote login.

| Component   | Purpose                 |
| ----------- | ----------------------- |
| Private Key | Stays on your machine   |
| Public Key  | Installed on the server |

Benefits:

* stronger security
* reduced brute-force exposure
* standard operational practice

---

## Generate an SSH Key

Run:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

Accept the default location:

```text
~/.ssh/id_ed25519
```

Set a passphrase.

---

## Display Your Public Key

### macOS/Linux

```bash
cat ~/.ssh/id_ed25519.pub
```

### Windows

```powershell
type %USERPROFILE%\.ssh\id_ed25519.pub
```

Copy the full output.

---

## Add the Key to DigitalOcean

Navigate to:

```text
Settings → Security → SSH Keys
```

Paste your public key and give it a meaningful name.

---

# 2. Provision Your Droplet

## Create the VM

Use these settings:

| Setting        | Value                      |
| -------------- | -------------------------- |
| Image          | Ubuntu 24.04 LTS           |
| Size           | Basic → Regular → $6/month |
| Authentication | SSH Key                    |
| Region         | Closest to you             |

---

## What LTS Means

LTS stands for:

```text
Long Term Support
```

Production environments prefer LTS releases because they prioritize:

* stability
* security updates
* long-term maintenance

---

## Hostname

Use a meaningful hostname.

Example:

```text
mark-lab-01
```

---

# First SSH Connection

Copy the Droplet public IP.

Connect:

```bash
ssh root@YOUR_IP_ADDRESS
```

Accept the host fingerprint when prompted.

---

## Understanding the Prompt

You will initially see something similar to:

```text
root@your-hostname:~#
```

| Symbol | Meaning      |
| ------ | ------------ |
| #      | Root user    |
| $      | Regular user |

---

# 3. Linux CLI Orientation

## The Mental Model Shift

Traditional network operating systems are:

* vendor-controlled
* mode-based
* restricted

Linux is:

* file-oriented
* flexible
* automation-friendly
* operationally transparent

---

# Navigation Commands

```bash
pwd
ls
ls -la
cd /etc
cd ~
cd -
```

---

# Important Directories

| Path     | Purpose             |
| -------- | ------------------- |
| /etc     | Configuration files |
| /var/log | Logs                |
| /var/www | Web content         |
| /home    | User directories    |
| /tmp     | Temporary files     |

---

# Reading Files

```bash
cat /etc/hostname
less /etc/os-release
tail -f /var/log/syslog
```

Stop running commands with:

```text
Ctrl+C
```

---

# Help Systems

```bash
man ls
ls --help
```

---

# Pipes and Filtering

```bash
cat /etc/os-release | grep VERSION
ps aux | grep apache
```

This is conceptually similar to using:

```text
| include
```

on network equipment.

---

# 4. Create a Non-Root User

## Why This Matters

Running permanently as root is dangerous.

Best practice:

* operate as a normal user
* elevate privileges temporarily with sudo

---

## Create the User

```bash
adduser yourname
```

---

## Grant sudo Access

```bash
usermod -aG sudo yourname
```

---

## Test the User

Open a second terminal.

Connect:

```bash
ssh yourname@YOUR_IP_ADDRESS
```

Test sudo:

```bash
sudo apt update
```

---

# File Permissions Basics

View permissions:

```bash
ls -la /etc/passwd
```

Permission format:

```text
rwxr-xr-x
```

Commands to remember:

```bash
chmod
chown
```

---

# 5. Update the System

Before installing software:

```bash
sudo apt update
sudo apt upgrade -y
```

---

# What apt Is

`apt` is Ubuntu’s package manager.

We will use it to install:

* Apache
* PHP
* MySQL

Equivalent ecosystems:

| Platform      | Package Manager |
| ------------- | --------------- |
| Ubuntu/Debian | apt             |
| RHEL/CentOS   | yum / dnf       |

---

# 6. Install the LAMP Stack

## What LAMP Means

| Letter | Component |
| ------ | --------- |
| L      | Linux     |
| A      | Apache    |
| M      | MySQL     |
| P      | PHP       |

---

# Install Apache

```bash
sudo apt install apache2 -y
```

Verify:

```bash
sudo systemctl status apache2
```

Look for:

```text
active (running)
```

---

# Open the Firewall

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Apache Full'
sudo ufw enable
sudo ufw status
```

Always allow SSH before enabling the firewall.

---

# Test Apache

Open:

```text
http://YOUR_IP_ADDRESS
```

You should see the default Apache page.

---

# Install MySQL

```bash
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql
```

Secure the installation:

```bash
sudo mysql_secure_installation
```

Recommended answers:

* Remove anonymous users: yes
* Disable remote root login: yes
* Remove test database: yes
* Reload privilege tables: yes

---

# Install PHP

```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

---

# Test PHP

Create:

```bash
sudo nano /var/www/html/info.php
```

Add:

```php
<?php phpinfo(); ?>
```

Visit:

```text
http://YOUR_IP_ADDRESS/info.php
```

Delete the file afterward:

```bash
sudo rm /var/www/html/info.php
```

---

# Apache Configuration Awareness

Open:

```bash
sudo nano /etc/apache2/apache2.conf
```

Notice:

* DocumentRoot
* DirectoryIndex
* configuration structure

Restart Apache after changes:

```bash
sudo systemctl restart apache2
```

---

# 7. Create a Simple Test Page

Create:

```bash
sudo nano /var/www/html/index.html
```

Example:

```html
<!DOCTYPE html>
<html>
<head><title>My Lab Server</title></head>
<body>
  <h1>It works.</h1>
  <p>Server: <strong>YOUR_NAME</strong></p>
  <p>Next session: DNS, TLS, and a real domain name.</p>
</body>
</html>
```

Visit:

```text
http://YOUR_IP_ADDRESS
```

---

# 8. Azure DevOps — First Commit

## Why We’re Doing This

Every student should leave Session 1 having:

* cloned a repository
* edited files
* committed changes
* pushed code

---

# Clone the Repository

Run locally — not on the server:

```bash
git clone https://dev.azure.com/YOUR_ORG/linux-course/_git/linux-course
cd linux-course
```

---

# Create Your Student Directory

```bash
mkdir student-projects/YOUR_NAME
cd student-projects/YOUR_NAME
```

---

# Create Notes

```bash
nano session1-notes.md
```

Include:

* server IP
* commands that were difficult
* troubleshooting notes

---

# Commit and Push

```bash
git add .
git commit -m "Session 1: initial notes and server IP — YOUR_NAME"
git push origin main
```

Verify your commit appears in Azure DevOps.

---

# 9. Wrap-Up & Homework

## What You Accomplished

Today you:

* Provisioned a public cloud VM
* Used SSH authentication
* Installed a full LAMP stack
* Served a public web page
* Performed Linux administration
* Made your first repository commit

---

# Important Commands

```bash
systemctl status apache2
systemctl restart apache2
sudo ufw status
tail -f /var/log/apache2/access.log
```

---

# Before Session 2

Required:

* Register a domain name
* Create a Cloudflare account
* Verify your server is still running

Recommended TLDs:

* .com
* .net

---

# Optional Exploration

Inspect:

```bash
/var/log/apache2/access.log
```

Observe:

* browser requests
* source IPs
* timestamps
* HTTP methods

---

# Preview of Session 2

Next session we will:

* Point DNS at your server
* Configure TLS certificates
* Use Cloudflare
* Explore DNS propagation
* Discuss why TTLs matter operationally

---

# Final Notes

The goal of Session 1 is not mastery.

The goal is to remove fear of:

* Linux systems
* SSH
* cloud infrastructure
* package management
* web servers
* terminal-based operations

You now have a real Internet-facing server that you built and configured yourself.
