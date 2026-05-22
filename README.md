# The Other Half of the Network

> A practical cloud & Linux series for network engineers — built around real infrastructure, real tools, and a finished artifact you'll actually use.

---

## What This Is

Network engineers are increasingly expected to have opinions about application performance, cloud connectivity, zero-trust architecture, and the stack that runs behind the firewall. This short course series is designed to close that gap — not with slides and theory, but with a running server on your own domain and a tool you build yourself.

Three lunch-break sessions. About $10 out of pocket. Something real at the end.

---

## What You'll Build

By the end of Session 3 you'll have a **personal network diagnostic tool** running at your own domain — a web application that performs:

- **DNS lookup with TTL display** — A/AAAA records, authoritative nameservers, resolver used
- **HTTP reachability check** — response code, time to first byte, redirect chain, TLS certificate details and expiry
- **BGP route lookup** — ASN, prefix, and upstream peers via public looking glass API. Your own personal looking glass.
- **Traceroute visualization** — per-hop latency rendered as a clean HTML table, not raw terminal output

This is not a WordPress blog that gets torn down after the class. It's infrastructure you own, on a domain you own, doing something you'll actually open again.

---

## Who This Is For

- Network engineers who want to speak credibly on both sides of the firewall
- Anyone who has ever been in a room where an application problem got blamed on the network and wants better tools for that conversation
- People who want a low-stakes, personal context for building real AI-assisted development skills — without a hackathon audience

The course assumes comfort with the command line in a general sense (if you've used IOS or NX-OS you're fine) and no prior Linux or web server experience.

---

## Course Structure

### Session 1 — Standing Up the Stack
*~90 minutes*

| Topic | What's covered |
|---|---|
| DigitalOcean setup | Account creation, billing limits, SSH key authentication |
| Linux CLI orientation | Navigation, file system layout, pipes — with IOS/NX-OS analogies |
| User & permission management | Non-root user, sudo, file permissions |
| LAMP stack installation | Apache, MySQL, PHP — installed, verified, and serving a page |
| Azure DevOps onboarding | Clone the course repo, make your first commit |

**Session outcome:** A running web server reachable at a public IP address.

---

### Session 2 — DNS, TLS & Making It Reachable
*~90 minutes*

| Topic | What's covered |
|---|---|
| Domain registration | Cloudflare Registrar — at-cost pricing, no markup |
| DNS configuration | A, CNAME, MX records, TTL — and why TTL is everyone's enemy during a change window |
| TLS certificate | Let's Encrypt + Certbot — free, automated, three commands |
| Cloudflare proxy | DDoS protection, orange-cloud vs grey-cloud, what Cloudflare is actually doing |
| Firewall | Basic ufw configuration |

**Session outcome:** Your domain resolving to your server with a valid HTTPS certificate.

---

### Session 3 — Build Something Useful with GenAI
*~90 minutes*

| Topic | What's covered |
|---|---|
| GenAI-assisted development | Using Claude to generate the network diagnostic application |
| Application deployment | Flask/PHP app deployed on the LAMP stack |
| Feature build | DNS lookup, HTTP check, BGP route lookup, traceroute visualization |
| Troubleshooting | Reading log files, basic debugging workflow |
| Repo contribution | Push finished project code to the team repo |

**Session outcome:** A live, publicly accessible network diagnostic tool at your own domain.

---

## Tools & Cost

Everything below is what a student needs for the full course. Total out-of-pocket cost is approximately **$10**, self-funded.

| Tool | Purpose | Cost |
|---|---|---|
| [DigitalOcean](https://digitalocean.com) | Cloud VM — predictable flat-rate billing, clean console | $6/month |
| [Cloudflare Registrar](https://cloudflare.com/products/registrar) | Domain registration at cost, no markup | ~$9–10/year (~$1 prorated) |
| [Cloudflare DNS](https://cloudflare.com) | DNS hosting, DDoS protection, proxy — free tier | Free |
| [Let's Encrypt](https://letsencrypt.org) | TLS certificate via Certbot, automated renewal | Free |
| [Claude](https://claude.ai) | Code generation for the network diagnostic app | Free tier |
| PuTTY / Terminal | SSH client — CLI only, no GUI tools | Free |

> **Why DigitalOcean and not AWS?** AWS free tier billing complexity is a real distraction in a course context — surprising charges are common enough that protecting students from them consumes meaningful class time. DigitalOcean's $6/month Droplet is predictable, the console is clean, and all the concepts transfer directly to AWS, Azure, or GCP. For a shop already on Azure, DigitalOcean's mental model also maps more cleanly to "a VM in the cloud" without AWS-specific abstraction layers.

---

## Repo Structure

```
other-half-of-the-network/
│
├── README.md                        ← You are here
│
├── session-1/
│   ├── outline.md                   ← Session structure and timing
│   ├── student-guide.md             ← Step-by-step follow-along
│   └── instructor-notes.md          ← Gotchas, timing, what to say
│
├── session-2/
│   ├── outline.md
│   └── student-guide.md
│
├── session-3/
│   ├── outline.md
│   ├── student-guide.md
│   └── starter-code/
│       ├── app.py
│       ├── templates/
│       │   └── index.html
│       └── requirements.txt
│
├── student-projects/
│   └── README.md                    ← One subdirectory per student
│
└── resources/
    ├── ssh-key-setup.md             ← Mac, Windows, Linux instructions
    ├── digitalocean-setup.md        ← Account and Droplet walkthrough
    └── cloudflare-setup.md          ← DNS and proxy configuration reference
```

Student project code lives in `student-projects/YOUR_NAME/`. The `starter-code/` directory in Session 3 is the scaffold the GenAI-generated application builds on top of.

---

## Why This Course Exists

Two arguments, one for engineers and one for leadership.

**For engineers:** You can't troubleshoot what you've never touched. When an application is slow and the room is looking for somewhere to point, the engineer who has personally deployed the stack — who knows what a misconfigured connection pool looks like, what a certificate expiry does to a user session, what a 86400-second TTL means for a change window — is a different kind of contributor than the one who can only verify the path is clear. This course puts you on the other side of the firewall, even if only at small scale. That experience transfers.

**For leadership:** The visible AI capability-building moments in most organizations — hackathons, all-hands workshops, prompt engineering lunch-and-learns — create social pressure that produces performance, not capability. The people who actually develop a working relationship with these tools do it in low-stakes, personally motivated contexts where they can experiment without an audience. This course builds AI-assisted development skills the right way: a personal problem, personal infrastructure, no grade, no performance. Every student who uses Claude to generate their network tool has done the thing that matters — they've taken a problem they understood, used an AI to produce a first draft, evaluated it critically, and made it work.

---

## Getting Started

If you want to run this course for your own team:

1. Clone this repo and add it to your team's version control
2. Send students the [pre-session checklist](session-1/student-guide.md#pre-session-checklist) 48 hours before Session 1
3. Each student needs a personal credit/debit card and an SSH client — that's it
4. Work through the sessions as written or adapt freely — the outlines are starting points, not scripts

If you're a student working through this independently, start with the [Session 1 student guide](session-1/student-guide.md) and work at your own pace. Each session builds on the last but the gap between sessions can be days or weeks — the server keeps running.

---

## Contributing

This course was built for a network engineering team and is actively maintained. If you find an instruction that's broken, a tool that's changed, or a better way to explain something to a network-first audience — pull requests are welcome.

If you've run this with your own team and have notes on what landed and what didn't, open an issue. That feedback makes the material better for everyone.

---

## License

MIT — use it, adapt it, run it for your team, share it freely. Attribution appreciated but not required.

---

*Built with the conviction that the best way to understand a customer's problem is to have solved it yourself, at small scale, with your own money on the line.*
