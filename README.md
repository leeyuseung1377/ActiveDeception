# Active Deception System

An Active Deception Security System that automatically detects malicious traffic using **Suricata IDS**, evaluates attacker risk levels, and dynamically redirects suspicious clients to a **Honeypot** through an **Nginx Reverse Proxy**.

Unlike traditional IDS solutions that only detect attacks, this project actively deceives attackers while protecting the real production server.

---

# Project Architecture

```text
                +----------------------+
                |     Attacker (Kali)  |
                +----------+-----------+
                           |
                      HTTP Request
                           |
                           v
+-------------------------------------------------------+
| VM1 : Deception Gateway                               |
|-------------------------------------------------------|
| • Nginx Reverse Proxy                                 |
| • Suricata IDS                                        |
| • Python Controller                                   |
| • SQLite Database                                     |
|-------------------------------------------------------|
| Detect → Risk Analysis → Redirect Decision            |
+-------------+----------------------------+------------+
              |                            |
              |                            |
              v                            v
+-------------------------+      +-----------------------+
| VM2 : Real Web Server   |      | VM3 : Honeypot        |
| 192.168.85.150          |      | 192.168.85.160        |
+-------------------------+      +-----------------------+
```

---

# Features

## Suricata IDS

- Suricata 8.0.3
- Custom OWASP Top 10 Detection Rules
- Real-time Attack Detection
- JSON Logging via `eve.json`

### Supported Detection

- SQL Injection
- Cross Site Scripting (XSS)
- Command Injection
- Directory Traversal
- SSRF
- Authentication Attacks
- Request Smuggling
- Deserialization
- Broken Access Control
- Nmap Scanning

---

## Python Controller

The controller continuously analyzes Suricata alerts.

Current functions

- Parse `eve.json`
- Remove duplicated events
- Calculate attacker risk scores
- Store attack history
- Update attacker status
- Generate `risky_ips.json`

---

## SQLite Database

### alert_events

Stores every detected attack.

Information stored

- Source IP
- Destination IP
- Signature
- Severity
- Timestamp
- Raw Suricata Event

---

### risk_scores

Maintains cumulative attacker scores.

Example

| Source IP | Score | Status |
|-----------|------:|--------|
|192.168.85.144|18|RISKY|
|192.168.85.120|4|NORMAL|

---

### controller_runs

Stores controller execution history.

Includes

- Start Time
- End Time
- Processed Events
- Duplicate Events
- Invalid Events

---

# Automatic Risk Evaluation

Each detected attack increases the attacker's risk score.

Example

| Attack | Score |
|---------|------:|
|SQL Injection|+8|
|Command Injection|+10|
|Directory Traversal|+4|
|Nmap Scan|+2|

Once the accumulated score exceeds the threshold, the attacker is classified as:

```text
RISKY
```

---

# Automatic Nginx Synchronization

```text
Suricata Alert

↓

Python Controller

↓

SQLite

↓

risky_ips.json

↓

sync_nginx_risky_ips.py

↓

/etc/nginx/deception/risky_ips.map

↓

Nginx Reload
```

No manual intervention is required.

---

# Dynamic Traffic Redirection

### Normal User

```text
Client

↓

Nginx

↓

Real Web Server
```

### Suspicious User

```text
Client

↓

Nginx

↓

Honeypot
```

---

# Current Workflow

```text
Suricata IDS

↓

eve.json

↓

Python Controller

↓

SQLite Database

↓

Risk Score Calculation

↓

risky_ips.json

↓

Nginx Synchronization

↓

Automatic Reload

↓

Traffic Redirection

↓

Real Server / Honeypot
```

---

# Project Status

## Completed

- Nginx Reverse Proxy
- Suricata Deployment
- Custom Detection Rules
- Python Controller
- SQLite Integration
- Risk Score System
- Automatic JSON Generation
- Automatic Nginx Synchronization
- Dynamic Honeypot Redirection

---

## In Progress

- Risk Score Optimization
- Time-based Score Decay
- Honeypot Session Logging
- Web Administration Dashboard
- Kibana Visualization

---

## Planned

- Adaptive Risk Threshold
- Machine Learning-based Risk Prediction
- Threat Intelligence Integration
- Multiple Honeypot Support
- REST API
- Docker Deployment

---

# Technology Stack

| Component | Technology |
|------------|------------|
| Operating System | Ubuntu Server |
| IDS | Suricata 8.0.3 |
| Reverse Proxy | Nginx |
| Programming Language | Python 3 |
| Database | SQLite |
| Logging | Suricata eve.json |
| Virtualization | VMware |

---

# Project Goal

This project aims to build an **Active Deception Platform** capable of

- Detecting malicious network traffic
- Evaluating attacker behavior
- Automatically redirecting suspicious users
- Collecting attack intelligence
- Protecting production services without affecting legitimate users

---

# Repository Structure

```text
Active-Deception/
│
├── controller/
│   ├── controller.py
│   ├── sync_nginx_risky_ips.py
│   └── risky_ips.json
│
├── database/
│   └── deception.db
│
├── nginx/
│   └── deception_proxy.conf
│
├── suricata/
│   └── local.rules
│
├── docs/
│
└── README.md
```

---

# License

MIT License
