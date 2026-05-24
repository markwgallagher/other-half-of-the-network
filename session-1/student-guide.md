# student-guide.md

# The Other Half of the Network

## Session 1 — Foundations, Flow, and Why Networks Matter

### Welcome

This course is designed to help students understand the modern network beyond just memorizing ports, protocols, and diagrams. We will focus on how real systems communicate, where problems occur, how modern applications behave, and why networking knowledge still matters in cloud, security, AI, gaming, streaming, and enterprise environments.

Session 1 establishes the mental model used throughout the course.

---

# Learning Objectives

By the end of Session 1, students should be able to:

* Explain what a network actually is in practical terms.
* Describe the difference between applications, protocols, and transport.
* Identify the major layers involved in modern communication.
* Understand packet flow at a high level.
* Explain the difference between TCP and UDP.
* Recognize how latency, bandwidth, and packet loss affect user experience.
* Understand why troubleshooting requires thinking across layers.
* Describe why encryption and modern protocols changed network visibility.
* Use Wireshark to observe basic packet exchanges.

---

# Required Software

Install before class if possible:

* Wireshark
* Modern web browser (Chrome, Firefox, or Edge)
* Terminal access

  * Windows Terminal / PowerShell
  * macOS Terminal
  * Linux shell

Optional:

* VS Code
* Packet Tracer or GNS3

---

# Vocabulary for Session 1

| Term       | Meaning                                              |
| ---------- | ---------------------------------------------------- |
| Packet     | A unit of network data sent across a network         |
| Protocol   | Rules for communication between systems              |
| Client     | A system requesting a service                        |
| Server     | A system providing a service                         |
| Latency    | Delay between request and response                   |
| Bandwidth  | Maximum transfer capacity                            |
| Throughput | Actual achieved transfer rate                        |
| TCP        | Reliable transport protocol                          |
| UDP        | Fast, connectionless transport protocol              |
| DNS        | Converts names into IP addresses                     |
| TLS        | Encryption used for secure communications            |
| Flow       | A conversation between systems                       |
| Endpoint   | Device or application participating in communication |

---

# The Big Idea

Most users think the Internet is:

* websites
* apps
* videos
* games
* cloud services

Networking professionals see:

* protocols
* state
* timing
* routing
* retransmissions
* congestion
* encryption
* application behavior
* visibility challenges

Modern networking is no longer just switches and routers.
It is understanding how distributed systems communicate.

---

# A Modern Packet Journey

When you open a webpage:

1. Your system checks DNS.
2. DNS resolves the destination.
3. Your browser opens a connection.
4. TLS negotiation occurs.
5. HTTP requests are exchanged.
6. Data is split into packets.
7. Packets traverse many networks.
8. Responses return.
9. Lost packets may be retransmitted.
10. The application renders content.

This all happens in milliseconds.

---

# Layer Thinking

You do not need to memorize the OSI model for this course.
You do need to understand layered thinking.

A problem can exist in:

| Layer Area  | Example Problem         |
| ----------- | ----------------------- |
| Physical    | Bad cable or weak Wi‑Fi |
| Network     | Routing issue           |
| Transport   | TCP retransmissions     |
| Security    | TLS handshake failure   |
| Application | Broken API              |
| DNS         | Name resolution failure |

Real troubleshooting means asking:

> “Which layer is actually failing?”

---

# TCP vs UDP

## TCP

TCP prioritizes reliability.

Features:

* Ordered delivery
* Retransmissions
* Congestion control
* Session state

Common Uses:

* HTTPS
* SSH
* Email
* APIs

Strength:
Reliable communication.

Weakness:
Extra overhead and latency.

---

## UDP

UDP prioritizes speed.

Features:

* Minimal overhead
* No guaranteed delivery
* No retransmission
* Stateless transport

Common Uses:

* Gaming
* Voice/video
* DNS
* Streaming
* QUIC foundations

Strength:
Low latency.

Weakness:
Applications must handle reliability.

---

# Why QUIC Matters

Modern applications increasingly use QUIC and HTTP/3.

QUIC:

* Runs over UDP
* Includes encryption by default
* Reduces connection setup time
* Improves mobility and performance

This changes traditional troubleshooting because:

* Less traffic is visible in plaintext
* Middleboxes lose visibility
* Old assumptions break

---

# Latency vs Bandwidth

A fast network is not always a low-latency network.

Examples:

| Situation                     | Result                            |
| ----------------------------- | --------------------------------- |
| Huge bandwidth + high latency | Large downloads okay, gaming bad  |
| Low bandwidth + low latency   | Responsive but limited throughput |
| Packet loss                   | Applications stall or retransmit  |

User experience is often more sensitive to latency than bandwidth.

---

# What Wireshark Shows You

Wireshark allows you to observe:

* DNS lookups
* TCP handshakes
* TLS negotiation
* HTTP requests
* Retransmissions
* Resets
* Timing

Wireshark does NOT magically decode everything.
Modern encryption limits visibility.

That is part of modern networking reality.

---

# Lab Exercise — First Packet Capture

## Goal

Observe a real web session.

Students may use either:

* Wireshark
* tcpdump

The important skill is observing flows and protocols.

---

## Option A — Wireshark

### Steps

1. Open Wireshark.
2. Start capture on active interface.
3. Visit a website.
4. Stop capture.
5. Search for:

   * DNS
   * TCP SYN
   * TLS
6. Identify:

   * Source IP
   * Destination IP
   * Protocols used

---

## Option B — tcpdump

### Start Capture

Linux/macOS:

```bash
sudo tcpdump -i any -w first_capture.pcap
```

Windows with Npcap:

```powershell
tcpdump -i 1 -w first_capture.pcap
```

### Generate Traffic

Visit a website or run:

```bash
ping example.com
```

### Stop Capture

Press:

```text
CTRL+C
```

### Review Capture

```bash
tcpdump -nn -r first_capture.pcap
```

Optional:

Open the `.pcap` file in Wireshark for deeper analysis.

---

## Questions

## Goal

Observe a real web session.

## Steps

1. Open Wireshark.
2. Start capture on active interface.
3. Visit a website.
4. Stop capture.
5. Search for:

   * DNS
   * TCP SYN
   * TLS
6. Identify:

   * Source IP
   * Destination IP
   * Protocols used

## Questions

* How many protocols were involved?
* Was the traffic encrypted?
* How quickly did the connection establish?
* Did you observe retransmissions?

---

# Key Concepts to Remember

* Networks are systems of systems.
* Applications drive network behavior.
* Performance is affected by timing, not just speed.
* Encryption changed observability.
* Modern troubleshooting requires cross-layer thinking.
* Understanding flows matters more than memorizing ports.

---

# Suggested Reading and Exploration

* RFC basics and protocol culture
* HTTP vs HTTPS
* Introductory Wireshark tutorials
* TCP three-way handshake
* DNS fundamentals
* QUIC and HTTP/3 overview

Recommended references include introductory networking materials and modern protocol documentation. ([en.ppt-online.org](https://en.ppt-online.org/831336?utm_source=chatgpt.com))

---

# Session 1 Exit Questions

Before the next class, be able to answer:

1. What is the difference between TCP and UDP?
2. Why does latency matter?
3. What role does DNS play?
4. Why is encrypted traffic harder to troubleshoot?
5. What does Wireshark actually show you?
6. Why is “the network is slow” usually incomplete?
