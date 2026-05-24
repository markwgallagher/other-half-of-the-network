# instructor-notes.md

# Session 1 — Instructor Notes

## Standing Up the Stack

**Audience:** Primarily network engineers and adjacent technical staff
**Duration:** ~90 minutes
**Format:** Follow-along, hands-on

This document is intended to support the live presentation and classroom pacing for Session 1.

The student guide is procedural.

These notes focus on:

* pacing
* demonstrations
* classroom management
* operational framing
* common student failures
* useful commentary
* transition points

---

# Core Session Philosophy

The purpose of Session 1 is not Linux mastery.

The purpose is to:

* remove fear
* build operational confidence
* create momentum
* demystify cloud infrastructure
* establish comfort with CLI workflows

Students should leave with the feeling:

> “I actually built and deployed something real.”

That emotional shift matters.

---

# What Students Are Actually Learning

Technically, they are learning:

* Linux basics
* SSH
* package management
* Apache
* PHP
* MySQL
* cloud provisioning
* git workflows

Operationally, they are learning:

* systems thinking
* deployment sequencing
* troubleshooting under uncertainty
* service verification
* infrastructure ownership
* comfort with terminal workflows

That second list is more important long-term.

---

# Recommended Pacing

| Section                        | Target Time |
| ------------------------------ | ----------- |
| Welcome & framing              | 10 min      |
| DigitalOcean + SSH keys        | 15 min      |
| Droplet provisioning           | 10 min      |
| Linux CLI orientation          | 15 min      |
| Non-root user setup            | 10 min      |
| Updates and package management | 5 min       |
| LAMP install                   | 20 min      |
| Azure DevOps commit            | 10 min      |
| Wrap-up                        | 5 min       |

Protect the LAMP install section.

That is the psychological payoff moment.

---

# Presentation Strategy

Avoid spending too long on slides before students touch the keyboard.

The rhythm should be:

1. Explain briefly
2. Demonstrate live
3. Students perform the task
4. Pause for troubleshooting
5. Continue

Students learn faster when momentum stays high.

---

# Important Instructor Mindset

Do not present Linux as sacred wizard knowledge.

Present it as:

* understandable
* operational
* practical
* learnable through repetition

Avoid excessive jargon unless you immediately contextualize it.

---

# 0. Welcome & Framing

## Important Messaging

Say explicitly:

> “The goal is not memorizing commands. The goal is understanding systems.”

Students often assume:

* servers are magical
* cloud infrastructure is extremely fragile
* Linux admins memorize everything
* terminal workflows are inherently difficult

Session 1 should dismantle those assumptions.

---

# Emphasize This Is Real Infrastructure

Students should understand:

* this is public Internet infrastructure
* their server is genuinely reachable
* these are real operational workflows
* mistakes are normal and recoverable

This creates buy-in.

---

# 1. DigitalOcean Account Setup

## Why DigitalOcean Instead of AWS

This question will come up.

Recommended framing:

* AWS is excellent but operationally noisy for beginners
* pricing complexity creates anxiety
* DigitalOcean exposes the concepts directly
* students spend less time navigating UI complexity

Make clear:

> “The concepts transfer directly to AWS later.”

---

# Common Problems

## Billing Verification Delays

Some students may experience:

* fraud checks
* SMS verification delays
* rejected cards
* account review holds

Have alternate discussion material ready while waiting.

---

## Students Skipping Spending Limits

Do not allow students to skip this step.

Explain why operational discipline matters early.

Good opportunity to discuss:

* accidental cloud spend
* orphaned resources
* snapshots
* load balancers
* oversized instances

---

# SSH Key Section

## Critical Teaching Moment

This is often the first time students truly understand:

* asymmetric authentication
* public/private key pairs
* passwordless trust

Slow down slightly here.

---

# Useful Analogy

Public key:

> “A padlock you can hand out publicly.”

Private key:

> “The only key capable of opening it.”

---

# Common Windows Problems

Most likely issue:

* PuTTY users
* `.ppk` confusion
* OpenSSH not configured

Strong recommendation:

* Windows Terminal
* built-in OpenSSH client

Avoid spending excessive time supporting PuTTY unless necessary.

---

# Common SSH Mistakes

## Students copy the private key instead of the public key

Watch carefully for this.

Public key should end with:

```text
.pub
```

---

## Students lose the private key location

Reinforce:

```text
~/.ssh/
```

---

# 2. Provisioning the Droplet

## Important Operational Concepts

This is a good place to explain:

* what a VM actually is
* virtualization basics
* public IP assignment
* regions and latency
* why infrastructure locality matters

Keep explanations brief and practical.

---

# LTS Explanation

Do not overcomplicate this.

Simple framing:

> “LTS releases prioritize stability and long-term maintenance.”

Mention:

* production preference
* patch stability
* reduced operational surprises

---

# First SSH Login

Pause after students successfully connect.

This is a major milestone.

Some students will visibly realize:

> “I’m operating a real server.”

Acknowledge that moment.

---

# Root Prompt Discussion

Explicitly point out:

```text
#
```

means root.

```text
$
```

means regular user.

Say this directly:

> “You will eventually break something as root. Everyone does.”

This reduces fear when mistakes happen later.

---

# 3. Linux CLI Orientation

## Important Framing

Network engineers often expect:

* mode-driven CLI behavior
* restricted commands
* vendor abstraction

Linux feels different because:

* everything is exposed
* everything is composable
* the filesystem matters

---

# Focus on Navigation Confidence

Students do not need mastery.

They need comfort with:

* moving around
* reading files
* recovering orientation
* using help systems

---

# Most Important Commands

Students should become comfortable with:

```bash
pwd
ls
cd
cat
less
tail
grep
```

These are foundational operational tools.

---

# Pipes Are a Great Bridge

Network engineers already understand filtering concepts from:

```text
| include
```

Use this bridge aggressively.

It reduces intimidation.

---

# Important Teaching Moment

Explain:

> “Linux commands are small tools designed to be combined.”

That idea is foundational to operational automation later.

---

# 4. Non-Root User Creation

## Important Operational Framing

This section is about:

* operational safety
* accountability
* least privilege
* real-world practices

Good analogy:

> “Using root for daily operations is like using enable mode permanently.”

---

# Common Mistakes

## Students forget the password immediately

Encourage password managers if appropriate.

---

## Students typo the username

Common during:

```bash
usermod -aG sudo yourname
```

Watch carefully.

---

# 5. System Updates

## Good Discussion Opportunity

While updates run, discuss:

* package repositories
* trusted software sources
* security updates
* patching responsibility
* operational maintenance

---

# apt Discussion

Students often ask:

> “Is apt like an app store?”

Reasonable answer:

> “Conceptually yes — but repository-driven and automation-friendly.”

---

# 6. LAMP Stack Installation

# This Is the Core Session

Protect this time.

Do not rush.

This is where students see:

* services
* ports
* web traffic
* application infrastructure
* public accessibility

Everything becomes real here.

---

# Apache Install

## Important Talking Points

Explain:

* Apache listens on port 80
* browsers connect via HTTP
* the web root maps to filesystem content
* services run independently of login sessions

---

# systemctl Is Important

Students should understand:

```bash
systemctl status
systemctl restart
systemctl enable
```

These are operationally critical.

---

# Firewall Section

## CRITICAL WARNING

Always ensure students allow SSH before enabling UFW.

Explicitly verify:

```bash
sudo ufw allow OpenSSH
```

before:

```bash
sudo ufw enable
```

Otherwise students may lock themselves out.

---

# Apache Verification Moment

When students open:

```text
http://YOUR_IP_ADDRESS
```

pause and reinforce:

> “You are now hosting a public web server.”

This moment matters psychologically.

---

# MySQL Installation

Keep database explanation high-level.

Students do not yet need:

* schema design
* SQL syntax
* query optimization

Focus on:

* persistent application storage
* services
* authentication
* operational roles

---

# mysql_secure_installation

Students sometimes panic at the prompts.

Reassure them:

* defaults are usually fine
* yes/no answers are acceptable
* this is basic hardening

---

# PHP Section

## Important Concept

Explain:

* Apache serves static files directly
* PHP generates dynamic content
* PHP integrates into the request flow

This is often the first time students understand dynamic web generation.

---

# PHP Info Page

Strongly emphasize deletion afterward.

Useful line:

> “Operationally useful during setup, dangerous to leave exposed.”

---

# Apache Config Tour

Do not deep-dive.

The goal is awareness, not mastery.

Students should simply recognize:

* configs are files
* services read configs
* changes require reload/restart

---

# 7. Creating the Test Page

## This Is Another Important Psychological Moment

Students replaced the default page with their own content.

Pause here.

Reinforce:

> “You are now serving content from infrastructure you control.”

---

# 8. Azure DevOps Section

## Why This Matters

Many students postpone learning git indefinitely.

This section forces:

* cloning
* editing
* committing
* pushing

Keep the workflow lightweight and encouraging.

---

# Most Likely Git Problem

Authentication failures.

Depending on org configuration:

* PATs may be required
* browser auth may fail
* credential caching may behave inconsistently

Have a prepared PAT workflow ready.

---

# Important Messaging

Say explicitly:

> “Infrastructure without version control becomes operational archaeology.”

That line tends to stick.

---

# 9. Wrap-Up

## Reinforce Accomplishment

Students often underestimate what they just did.

Recap explicitly:

* provisioned cloud infrastructure
* authenticated with SSH keys
* administered Linux
* installed a web server
* configured services
* deployed content
* committed to version control

That is a substantial amount of operational knowledge for a first session.

---

# Preview Session 2 Carefully

Build anticipation around:

* real DNS
* domain ownership
* TLS certificates
* Cloudflare
* traffic visibility
* Internet propagation behavior

Good teaser line:

> “Next session is where your server starts feeling like a real Internet property instead of just an IP address.”

---

# Common Student Emotional States

| Situation                            | Instructor Response                        |
| ------------------------------------ | ------------------------------------------ |
| “I’m behind.”                        | Normalize it immediately                   |
| “I broke something.”                 | Troubleshoot publicly and calmly           |
| “Everyone else gets this except me.” | Explicitly reject this assumption          |
| “Linux is intimidating.”             | Remind them they already deployed a server |

---

# If Time Runs Short

Priority order:

1. LAMP stack completion
2. Apache verification
3. Student web page
4. Git commit

Lower priority:

* extended CLI discussion
* detailed Apache config discussion
* advanced permissions discussion

---

# Final Instructor Reminder

The students do not need perfect retention after Session 1.

They need:

* confidence
* curiosity
* operational momentum
* reduced fear of Linux and infrastructure

If students leave thinking:

> “I can actually do this.”

then the session succeeded.
