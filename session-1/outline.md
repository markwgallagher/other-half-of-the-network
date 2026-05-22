# Session 1: Standing Up the Stack
## "The Other Half of the Network" — Course Series
**Duration:** ~90 minutes | **Format:** Follow-along, hands-on

---

## Pre-Session Checklist (send to students 48hrs before)
- [ ] Personal credit/debit card available (~$6 charge)
- [ ] SSH client installed — Terminal (Mac/Linux) or Windows Terminal / PuTTY (Windows)
- [ ] Azure DevOps account access confirmed
- [ ] Git installed locally (`git --version` to verify)
- [ ] Browser open to: digitalocean.com, cloudflare.com

---

## 0. Welcome & Framing (10 min)

### Why we're here
- The network is half the picture — this series covers the other half
- Not a Linux certification prep course — we're building something real
- By the end of today: a running web server on a public IP
- By the end of Session 3: a personal network diagnostic tool at your own domain

### Ground rules
- Follow along on your own machine — not a spectator sport
- It's okay to be behind — shout out, we'll catch up together
- CLI only — no GUI tools, no cPanel, no file managers
- There are many ways to do everything shown here; this is one of them
- Questions anytime — if you're confused, someone else is too

### Cost transparency
- Today's session will trigger a $6 charge when you create the Droplet
- Total course cost is ~$10 — this is yours, not expensed
- You can keep the server running after the course for $6/month
- We'll cover how to destroy it cleanly if you don't want to

---

## 1. DigitalOcean Account Setup (15 min)

### Create your account
- Navigate to digitalocean.com → Sign Up
- Use your personal email (not work) — this is your infrastructure
- Verify email address

### Billing & limits — do this before anything else
- Add payment method (credit/debit card)
- Navigate to: Settings → Billing → Spending Limits
- Set a monthly spending alert at $15 — protects against accidents
- Brief discussion: why DigitalOcean over AWS for this course
  - Predictable flat-rate billing vs. AWS consumption model
  - Cleaner console — less time navigating, more time learning
  - Same concepts transfer: VMs, SSH keys, firewalls, DNS

### SSH key setup — the right way to authenticate
- What is an SSH key pair and why it matters (2 min)
  - Private key stays on your machine, never shared
  - Public key goes on the server — it's what gets installed
  - No password = no brute-force attack surface
- Generate a key pair if you don't have one:

```

ssh-keygen -t ed25519 -C "your_email@example.com"

```

- Accept default location (`~/.ssh/id_ed25519`)
- Set a passphrase — this protects your private key
- Copy your public key:
  - Mac/Linux: `cat ~/.ssh/id_ed25519.pub`
  - Windows: `type %USERPROFILE%\.ssh\id_ed25519.pub`
- Add to DigitalOcean: Settings → Security → SSH Keys → Add SSH Key
- Paste public key, give it a meaningful name

---

## 2. Provision Your Droplet (10 min)

### Create the VM
- Dashboard → Create → Droplets
- **Region:** Choose closest to you (note: for real workloads, choose closest to users)
- **Image:** Ubuntu 24.04 LTS
  - Brief: what LTS means and why it matters in production
- **Size:** Basic → Regular → $6/month (1GB RAM, 1 vCPU, 25GB SSD)
  - This is intentionally minimal — teaches resource awareness
- **Authentication:** SSH Key → select the key you just added
- **Hostname:** give it something meaningful — `yourname-lab-01`
- Click Create — takes about 30 seconds

### First connection
- Copy the Droplet's public IP from the dashboard
- Connect:

```

ssh root@YOUR_IP_ADDRESS


```

- Accept the host fingerprint (yes) — brief explanation of what this prompt means
- You're in. Note the prompt: `root@yourname-lab-01:~#`

### A word about root
- You're logged in as root — this is deliberate for today's simplicity
- In production: root login is disabled, you'd use a non-root sudo user
- We'll create a non-root user shortly — this mirrors real practice
- The `#` prompt means root; `$` means regular user — pay attention to this

---

## 3. Linux CLI Orientation (15 min)
*For network engineers — drawing on what you already know*

### The mental model shift
- IOS/NX-OS: vendor-controlled, mode-based, limited filesystem access
- Linux CLI: everything is a file, one mode, unlimited flexibility
- The shell is your config terminal — except it also runs the OS

### Navigation — muscle memory first
```bash
pwd           # where am I? (like 'show ip interface brief' — orientation)
ls            # what's here?
ls -la        # long format with hidden files
cd /etc       # change directory
cd ~          # go home
cd -          # go back (like 'end' in IOS)
```

### Key directories — the ones that matter today
| Path | What lives here | IOS analogy |
|---|---|---|
| `/etc` | Config files | running-config |
| `/var/log` | Log files | show log |
| `/var/www` | Web content | — |
| `/home` | User home dirs | — |
| `/tmp` | Temporary files | — |

### Reading files
```bash
cat /etc/hostname         # dump entire file
less /etc/os-release      # paginated view (q to quit)
tail -f /var/log/syslog   # follow live (like 'debug' — Ctrl+C to stop)
```

### Getting help
```bash
man ls          # manual page for any command
ls --help       # quick flag reference
```

### Pipes — composing commands (network engineers love this)
```bash
cat /etc/os-release | grep VERSION    # filter output
ps aux | grep apache                  # find a running process
```
- If you've ever used `| include` in IOS, this is the same idea — more powerful

---

## 4. Create a Non-Root User (10 min)

### Why this matters
- Running everything as root is like using the enable password for your daily SSH session
- Best practice: non-root user with sudo privileges
- This is what you'd do in any real environment

### Create the user
```bash
adduser yourname
```
- Set a strong password
- Fill in details or press Enter to skip

### Grant sudo privileges
```bash
usermod -aG sudo yourname
```

### Test it
- Open a second terminal window
- SSH in as your new user:


```

ssh yourname@YOUR_IP_ADDRESS

```

- Test privilege escalation:


```

sudo apt update

```

- If it prompts for your password and runs — you're set

### Understanding file permissions (5 min)
```bash
ls -la /etc/passwd
```
- Read the output: `rwxr-xr-x` — owner / group / others
- `chmod` and `chown` — brief explanation, we'll use these when deploying the app
- The web server will need to read files owned by your user — this matters in Session 3

---

## 5. Update the System (5 min)

### Apply all updates before installing anything
```bash
sudo apt update          # refresh package list (like 'show version' — inventory)
sudo apt upgrade -y      # apply updates
```

### Brief: what apt is
- Ubuntu's package manager — think of it as the App Store for server software
- Packages come from repositories (repos) — curated, verified sources
- `apt install` is how we'll install everything today: Apache, PHP, MySQL
- Alternative: `yum` / `dnf` on Red Hat based systems (CentOS, RHEL) — same concept

---

## 6. Install the LAMP Stack (20 min)

### What LAMP is and why it matters
- **L**inux — the OS (already done)
- **A**pache — the web server (handles HTTP requests)
- **M**ySQL — the database (stores application data)
- **P**HP — the scripting language (generates dynamic content)
- WordPress, Joomla, Drupal, and a huge proportion of the web run on this stack
- Your customers are running this — or something very close to it

### Install Apache
```bash
sudo apt install apache2 -y
```

**Verify it's running:**
```bash
sudo systemctl status apache2
```
- Look for `Active: active (running)`
- `systemctl` is how you manage services on modern Linux — replaces the old `service` command

**Open the firewall:**
```bash
sudo ufw allow 'Apache Full'
sudo ufw enable
sudo ufw status
```

**Test it:**
- Open a browser and navigate to `http://YOUR_IP_ADDRESS`
- You should see the Ubuntu Apache default page
- This is what your customers see when Apache is running but no site is configured yet

### Install MySQL
```bash
sudo apt install mysql-server -y
sudo systemctl start mysql
sudo systemctl enable mysql        # start automatically on boot
```

**Secure the installation:**
```bash
sudo mysql_secure_installation
```
- Set root password
- Remove anonymous users: yes
- Disallow root login remotely: yes
- Remove test database: yes
- Reload privilege tables: yes

**Verify:**
```bash
sudo systemctl status mysql
```

### Install PHP
```bash
sudo apt install php libapache2-mod-php php-mysql -y
```

**Verify PHP is working with Apache:**
```bash
sudo nano /var/www/html/info.php
```
Add the following:
```php
<?php phpinfo(); ?>
```
Save and navigate to `http://YOUR_IP_ADDRESS/info.php`
- You should see the PHP info page — a wall of configuration detail
- Note the Server API line: `Apache 2.0 Handler` — PHP is talking to Apache correctly
- **Delete this file when done** — it exposes system information:
```bash
  sudo rm /var/www/html/info.php
```

### Quick Apache configuration
```bash
sudo nano /etc/apache2/apache2.conf
```
- Brief tour of the config file structure
- Note `DocumentRoot` — this is where web files live (`/var/www/html`)
- Note `DirectoryIndex` — order in which Apache looks for index files

**Restart to apply any changes:**
```bash
sudo systemctl restart apache2
```

---

## 7. Create a Simple Test Page (5 min)

### Put something real at the web root
```bash
sudo nano /var/www/html/index.html
```

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

Navigate to `http://YOUR_IP_ADDRESS` — your page replaces the Apache default.

---

## 8. Azure DevOps — First Commit (10 min)

### Why we're doing this now
- The course materials live in the team repo
- Every student leaves Session 1 having made at least one commit
- This is the forcing function — we're not asking people to adopt git separately

### Clone the course repo
```bash
# Back on your local machine (not the server)
git clone https://dev.azure.com/YOUR_ORG/linux-course/_git/linux-course
cd linux-course
```

### Create your student directory
```bash
mkdir student-projects/YOUR_NAME
cd student-projects/YOUR_NAME
```

### Add a simple notes file
```bash
nano session1-notes.md
```
Add your server IP, any commands that tripped you up, anything you want to remember.

### Commit and push
```bash
git add .
git commit -m "Session 1: initial notes and server IP — YOUR_NAME"
git push origin main
```

**Verify:** Open Azure DevOps in a browser and confirm your commit appears.

---

## 9. Wrap-Up & Homework (5 min)

### What you accomplished today
- Created and secured a cloud VM — $6/month, yours to keep
- Installed and verified a full LAMP stack
- Served a web page from your own server
- Made your first commit to the team repo

### Key commands to remember
```bash
systemctl status apache2     # is the web server running?
systemctl restart apache2    # apply config changes
sudo ufw status              # what's the firewall allowing?
tail -f /var/log/apache2/access.log   # watch live web traffic
```

### Before Session 2
- [ ] Register a domain name — Cloudflare Registrar (cloudflare.com/products/registrar)
  - Pick anything — it's ~$10/year and it's yours
  - `.com` or `.net` recommended for MX record compatibility in Session 3
- [ ] Create a free Cloudflare account if you don't already have one
- [ ] Make sure your server is still running (check the DigitalOcean dashboard)
- [ ] Optional: explore `/var/log/apache2/access.log` — what hits are you already getting?

### Preview of Session 2
- Point your domain at this server
- Get a real TLS certificate (free, takes about 3 minutes)
- Understand what Cloudflare is actually doing between your users and your server
- Start to see why DNS TTL is everyone's enemy during a change window

---

## Instructor Notes

### Common issues to anticipate
- **SSH key permissions on Windows:** PuTTY uses `.ppk` format — if students are on PuTTY, they'll need PuTTYgen to convert the key. Recommend Windows Terminal + OpenSSH instead.
- **`ufw enable` warning:** UFW will warn that enabling the firewall may disrupt existing SSH connections. It won't if Apache Full and SSH are both allowed — but have students confirm `ufw allow OpenSSH` first.
- **Slow `apt upgrade`:** Can take 3-5 minutes on a fresh instance. Good time to talk about package managers and repos while waiting.
- **PHP info page:** Some students will forget to delete it. Make a point of checking this before moving on.
- **Git authentication in Azure DevOps:** Personal Access Tokens may be required depending on org settings. Pre-generate a PAT and include the URL format in the student guide: `https://YOUR_PAT@dev.azure.com/...`

### Timing notes
- The CLI orientation section (section 3) can be shortened if the group is comfortable — skip to the parts they haven't seen
- The LAMP install section is the core of the session — protect this time
- The Azure DevOps commit is last because it's the one thing that can be done async if you run long

### Things to say explicitly
- "The `#` at the end of your prompt means you're root. The `$` means you're a regular user. You will make mistakes as root that you wouldn't make as a regular user. This is how you learn."
- "Every command you just ran to install and configure a web server is something your customers' platform teams run in automation. Now you've done it by hand. You know what it does."
- "When someone tells you in an engagement that 'the web server is down,' you now know exactly what that means and what the first three questions to ask are."
