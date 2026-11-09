# Cyber Threat Intelligence — Healthcare Case Study

A practical project exploring how cyber threat intelligence (CTI) works in a real organisation, not just in theory. It follows one healthcare provider through a suspected ransomware attack, and shows, step by step, how a pile of confusing security alerts can be turned into something a team can actually act on.

If you are new to cybersecurity, start here. This README explains everything in plain English first, and the full report is in [`REPORT.md`](REPORT.md). No background knowledge is assumed — every term is explained the first time it appears.

---

## Contents

- [What is cyber threat intelligence?](#what-is-cyber-threat-intelligence)
- [The scenario, step by step](#the-scenario-step-by-step)
- [The indicators (IoCs)](#the-indicators-iocs)
- [Glossary](#glossary)
- [What the report covers — the 5 sections](#what-the-report-covers--the-5-sections)
  - [1. From Data to Intelligence](#1-from-data-to-intelligence)
  - [2. CTI Classification](#2-cti-classification)
  - [3. The CTI Lifecycle](#3-the-cti-lifecycle)
  - [4. Maturity Assessment](#4-maturity-assessment)
  - [5. Intelligence-Driven Recommendations](#5-intelligence-driven-recommendations)
- [Why no threat actor is named](#why-no-threat-actor-is-named)
- [How to read the report](#how-to-read-the-report)
- [Files in this repo](#files-in-this-repo)
- [References](#references)

---

## What is cyber threat intelligence?

Imagine a hospital's security team getting thousands of alarms a day. Most of them are harmless. A few of them are the first signs of a real attack. The team cannot investigate all of them, so the real attack hides in the noise.

**Cyber threat intelligence is the work of turning raw security data into useful, timely knowledge that helps people make a decision.**

That sounds abstract, so here is the simple version:

- **Data** is a fact with no story attached. An IP address. A website name. A file fingerprint. On its own, it tells you nothing about what to do.
- **Information** is data that has been checked, sorted, and given context. That IP address turns out to be in a foreign country. That website was registered yesterday and pretends to be a hospital login page. That file fingerprint matches known malware. Now the facts have labels.
- **Intelligence** is information analysed against a real question, and turned into a judgement someone can act on. The question here is: *are we in the early stages of a ransomware attack, and what should we stop first?* When the facts are read together — phishing, stolen passwords, strange logins, movement between servers, files being encrypted — the answer becomes clear: isolate the affected servers, reset the passwords of the staff who were phished, and block the malicious address and domain.

The whole point of CTI is that last step. Collecting indicators is easy. Knowing what they mean, and what to do about them before it is too late, is the hard part — and it is the part this project is about.

---

## The scenario, step by step

The case study is a **medium-sized healthcare provider**: several clinics, one central electronic health record (EHR) system, and a security operations centre (SOC) — a team whose job is to watch for attacks.

Healthcare is a popular target for attackers, for two reasons the report explains. First, patient records are valuable. Second, a clinic cannot simply switch its systems off during an attack — people need care. Research cited in the report found ransomware in 54% of health-sector incidents analysed by ENISA between January 2021 and March 2023, and a recent literature review reaches a similar conclusion: ransomware in healthcare is regular, not rare, and it disrupts care as well as IT systems.

Here is what happens, in the order it unfolds:

### 1. Phishing emails target clinical staff

Attackers send fake emails to nurses, reception staff, and other clinical workers. The emails look like they come from a trusted source and try to trick people into clicking a link or entering their login details on a fake page. This is the way in — not a clever hack of a firewall, but a convincing email.

### 2. Credentials are harvested

Some staff enter their usernames and passwords on the fake login page (the domain `secure-login-health.com`). The attackers now have real logins for real systems. No password needs to be guessed or cracked — it was handed over.

### 3. Abnormal VPN logins outside business hours

A VPN is the secure tunnel staff use to log in to work systems from outside the building. The SOC starts seeing logins at odd hours — 2 a.m., for example — from a foreign IP address. A nurse logging in at 2 a.m. from another country is not normal, and it is a strong sign that a stolen password is being used by someone who is not that nurse.

### 4. Lateral movement between internal servers

Once inside, the attacker does not stay on one computer. They move from server to server inside the hospital's network, looking for valuable systems — especially the central EHR system and shared drives. This is called **lateral movement**, and it is how a single stolen password turns into access to the whole network.

### 5. PowerShell executions on endpoints

PowerShell is a legitimate administration tool built into Windows, used by IT staff to automate tasks. Attackers love it because it is already installed on every Windows computer, it is powerful, and its use does not always look suspicious on its own. In this case, PowerShell is seen running on staff computers (endpoints) in ways that do not match normal IT work — a sign the attacker is using it to run commands, download tools, or move further.

### 6. Large volumes of encrypted files on a shared drive

Finally, many files on a shared drive are found encrypted — scrambled so they cannot be opened without a key. This is the classic early sign of **ransomware**: software that locks up an organisation's files and demands payment to unlock them. By this point the attack is well under way.

### Meanwhile: alert fatigue

All of this is happening while the SOC is drowning in SIEM alerts — thousands a day, many of them false alarms. The analysts are so busy triaging alerts that they never step back and see the sequence above as one attack. Each piece was visible, in a different tool, to a different person. Nobody put them together in time. That failure — not a lack of data, but a failure to turn data into intelligence — is the central problem of the whole project.

---

## The indicators (IoCs)

An **indicator of compromise (IoC)** is a piece of evidence that suggests an attack has happened or is happening — an address, a domain, or a file fingerprint you can search for and block. The case provides three:

| Indicator | Value | What it means |
|---|---|---|
| Malicious IP address | `185.220.101.1` | The foreign address the abnormal VPN logins came from. Block it, and search logs for any other connections to it. |
| Suspicious domain | `secure-login-health.com` | The fake login page used in the phishing emails. It impersonates a health service login page. Block it, and check who visited it. |
| Malware file hash (SHA-256) | `a1b2c3d4e5f67890abcdef1234567890abcdef1234567890abcdef1234567890` | A unique fingerprint of the malicious file. Search endpoints for a file with this exact hash. |

A SHA-256 hash is like a fingerprint for a file: change even one byte of the file and the hash changes completely, so matching this exact string means you have found that exact file.

**Important:** these indicators are useful, but they are also fragile. An attacker can change an IP address or register a new domain in minutes. That is why the report argues for detections based on *behaviour* (phishing, unusual logins, PowerShell use, mass encryption) rather than on single indicators alone — behaviours are much harder for an attacker to change. See Section 4 on maturity for more on this.

---

## Glossary

Terms a newcomer will meet in this repo, in plain English:

- **IoC — Indicator of Compromise.** Evidence of an attack: an IP, domain, or file hash you can block or search for. See the table above.
- **SIEM — Security Information and Event Management.** The system that collects logs and alerts from across the organisation (firewalls, servers, email, VPN) into one place. Powerful, but it can produce thousands of alerts a day — which is how alert fatigue happens.
- **SOC — Security Operations Centre.** The team that watches the SIEM, investigates alerts, and responds to incidents. In this case, they are overwhelmed.
- **EHR — Electronic Health Record.** The central digital record of patient information. It is the provider's most important system, and the thing an attacker most wants to reach.
- **TIP — Threat Intelligence Platform.** A tool that acts as a single home for threat intelligence: indicators, context, and analyst notes all in one searchable place, instead of scattered across emails and spreadsheets. The report recommends this as a first technology step, because the provider does not currently have one.
- **SOAR — Security Orchestration, Automation and Response.** A tool that automates repetitive security chores — for example, automatically looking up an IP address or file hash, enriching an alert with context, and opening a ticket — so analysts spend their time investigating instead of copy-pasting.
- **EDR / XDR — Endpoint Detection and Response / Extended Detection and Response.** Software on computers (endpoints) that watches what actually happens on them — programs running, commands executed, files changed — and can spot suspicious behaviour like the PowerShell activity in this case. XDR extends the same idea across email, network, and cloud as well.
- **UBA — User Behaviour Analytics.** A tool that learns what "normal" looks like for each user — when they log in, from where, what they access — and flags behaviour that breaks the pattern, such as a VPN login at 2 a.m. from a foreign IP.
- **FIM — File Integrity Monitoring.** A tool that watches important files and folders and raises an alarm when they change unexpectedly. It would have caught the mass encryption on the shared drive much earlier.
- **ASM — Attack Surface Management.** The practice (and tooling) of keeping track of everything an organisation exposes to the internet — which clinics, systems, and services are reachable from outside — so nothing is forgotten and left unprotected.
- **ATT&CK — MITRE ATT&CK.** A public catalogue of the tactics, techniques, and procedures attackers actually use, organised into a matrix. The report maps the attack in this case to ATT&CK techniques (phishing, valid accounts, lateral movement, PowerShell, encryption) so the team can build detections for the *behaviour*, not just the indicators.
- **Tactics, Techniques and Procedures (TTPs).** *Tactics* are what an attacker is trying to do (e.g. get in, stay in, steal data). *Techniques* are how they do it (e.g. phishing, PowerShell). *Procedures* are the specific steps in a particular attack. Knowing the TTPs lets defenders recognise the same attack even when the IP address changes.
- **Threat hunting.** Proactively searching through logs and systems for signs of an attack that the automated alerts missed — instead of waiting for an alarm to go off.
- **Alert fatigue.** What happens when a team gets so many alerts, most of them false alarms, that they start ignoring them — including the real ones.
- **Ransomware.** Malicious software that encrypts an organisation's files and demands payment for the key to unlock them.

---

## What the report covers — the 5 sections

The full report is in [`REPORT.md`](REPORT.md) (1,945 words of body text). It has an Executive Summary, an Introduction, five sections, a Conclusion, References, and two appendices. Here is what each section does, and why it matters.

### 1. From Data to Intelligence

This section explains the three levels — **data, information, intelligence** — using the indicators from this case, and shows why organisations so often fail to make the last jump.

- *Data:* `185.220.101.1`, `secure-login-health.com`, and the SHA-256 hash are facts with no story attached. A firewall log showing a connection to that IP at 2 a.m. is worth keeping, but it does not yet tell anyone what to do.
- *Information:* the facts are checked and labelled. The IP is foreign, the domain is newly registered and impersonates a health login page, the hash matches known malware. A short note saying "this domain was used in phishing emails to nurses and reception staff, and three of them entered credentials" connects a technical fact to a person and a place.
- *Intelligence:* the information is analysed against a question — *are we in the early stages of a ransomware attack, and what should we stop first?* Read together, the sequence (phishing → stolen credentials → out-of-hours logins → movement between servers → PowerShell → encrypted files) is the intelligence, and it supports a specific decision: isolate the affected servers, reset the phished staff's passwords, and block the domain and IP.

It then explains four connected reasons the provider fails to make that jump: **volume** (thousands of SIEM alerts a day teach analysts to ignore alerts), **collection without a question** (data gathered because a feed exists, not because a decision needs it), **fragmentation** (phishing, VPN logs, endpoint alerts, and file encryption live in different tools, seen by different people), and **treating intelligence as a product to buy** rather than a process that needs analysts with time to think and managers who will act.

### 2. CTI Classification

The same indicators mean different things to different people, so CTI is usually split into four levels. The report explains each one, who it is for, and what it looks like in this case (summarised in Table A1 in the appendix):

| Level | Purpose | Audience | In this case |
|---|---|---|---|
| **Technical** | Let tools and analysts block, detect, or hunt for something specific, right now | SOC analysts and detection engineers | Block the IP and domain; search endpoints for the hash |
| **Tactical** | Help defenders recognise the same *behaviour* even when the indicators change | Detection engineers, incident responders, threat hunters | Map phishing, credential theft, VPN use, lateral movement, PowerShell, and encryption to ATT&CK, and build detections for those behaviours |
| **Operational** | Explain a specific campaign: who is behind it, what they want, how far they may go | Security operations and incident response managers | Decide on resourcing, whether to take systems offline, and how to brief clinical managers |
| **Strategic** | Inform senior leaders about risk over months, not hours | Executives and the board | Decide budgets, risk appetite, and what to prioritise — such as protecting the EHR platform |

The honest gap is flagged here too: the **operational** picture is incomplete. The pattern fits a financially motivated ransomware operation, and ENISA (2023) found most health-sector attacks were financially motivated — but there is no evidence yet to name a group or to say whether data was stolen as well as encrypted. That gap can only be filled from outside the provider, for example by checking whether other healthcare organisations have seen the same domain or hash.

### 3. The CTI Lifecycle

The intelligence lifecycle is the process that turns the work in Sections 1 and 2 from a one-off report into something the provider does routinely. It has **seven phases** (summarised in Table A2 in the appendix):

1. **Planning & Direction** — Agree what the SOC most needs to know, and what decisions intelligence must support. This is where the provider needs to start: without a short list of priority intelligence requirements, written with the CISO and clinical operations, collection is driven by what feeds are available rather than by what decisions need making. Phishing and ransomware go at the top.
2. **Collection** — Gather raw data that can answer those requirements, from inside (SIEM, EDR, email gateway, VPN logs) and outside (external feeds, sharing with other healthcare providers).
3. **Processing** — Turn raw data into a usable form: clean it, normalise it, deduplicate it, enrich it, and put it in one searchable place. Without this step, analysis is not possible.
4. **Analysis** — Test hypotheses, correlate events, and turn information into judgements — being honest about confidence levels. This is where data becomes intelligence.
5. **Production** — Write the result for its audience, in the right format and length: a short blocking list for the SOC, a plain-language briefing for senior leaders.
6. **Dissemination** — Get the product to the right people, at the right time, with handling rules for who can share it further.
7. **Feedback & Continuous Improvement** — Ask whether the product helped, and use the answer to sharpen the next cycle. This is the phase most likely to be skipped — and if it is skipped, the same mistakes repeat.

Each phase in the report also names its stakeholders, tools, expected output, and the implementation challenge that typically trips it up.

### 4. Maturity Assessment

How good is this provider at threat intelligence, really? The report answers with **three different models**, because they measure different things — and together they give a clearer picture than any one alone (summarised in Table A3 in the appendix):

- **TIMM — Threat Intelligence Maturity Model.** Looks at the CTI programme as a whole, on five levels from Initial to Optimising. The provider is at **Level 1, Initial** — and only just there. It has a SIEM and indicators, but no central repository, no documented process for turning data into intelligence, and no evidence that intelligence is routinely produced or consumed.
- **HMM — Hunt Maturity Model.** Asks how good the organisation is at proactively hunting for threats, from HM0 (relies entirely on automated alerting) to HM4 (successful hunts become automated detections). The provider is at **HM0**: there is no sign of hunting, because the team is too busy triaging alerts.
- **DML — Detection Maturity Level.** Looks at the detections themselves. At the lowest level they are based on *atomic indicators* — a single IP or hash, which an attacker can change quickly. Higher levels are based on tools, and then on tactics, techniques, and procedures. Here the provider is mostly at the atomic level, with no sign of behaviour-based detections.

Read together, the three models tell the same story — and it is also hopeful: the next steps are clear and do not require a large purchase first. A named owner, a central repository, a few use cases, a weekly hunt, and a handful of behaviour-based detections would move the provider up a level on all three at once.

### 5. Intelligence-Driven Recommendations

The final section turns everything before it into practical recommendations, grouped into **Technology, Processes, and People**:

- **Technology:** a threat intelligence platform (TIP) as the single repository the provider does not have; SOAR to automate repetitive triage; EDR/XDR, because the PowerShell executions and lateral movement are endpoint behaviours and without endpoint visibility the team is guessing; user behaviour analytics (UBA) to flag the out-of-hours VPN login; file integrity monitoring (FIM) to catch mass encryption earlier; attack surface management (ASM) to keep track of which clinics and systems are exposed.
- **Processes:** integrate CTI into detection engineering, so every new piece of intelligence ends up as a detection, a rule, or a hunt — not just a report that is read once. A short, practised incident response playbook for ransomware: who decides to isolate a server, who tells clinical managers, and when to call in outside help. Purple team exercises to test whether the new detections actually work, and threat modelling focused on the EHR platform and the VPN to decide what to protect first.
- **People:** SOC analysts need training specific to this case — recognising phishing, investigating a VPN anomaly, writing a short intelligence note — not generic awareness training. A CTI analyst role, even if it starts as part of someone's existing job, would give the programme an owner, which is the single most important gap from Section 4. Executive awareness matters too, because senior leaders need to understand ransomware as a business risk, not just an IT problem. Finally, threat hunting capability needs to be built deliberately, with time protected and results shared.

---

## Why no threat actor is named

A newcomer might expect a threat intelligence report to name the attacker group. This one deliberately does not — and says so explicitly.

The evidence in the case supports a judgement about *what is happening* (the early stages of a ransomware attack) but not about *who is doing it*. The pattern fits a financially motivated ransomware operation, and most health-sector attacks are financially motivated, but there is no evidence in the scenario that points to a specific group, and no evidence yet on whether data was stolen as well as encrypted. Naming a group on that basis would be guessing, and a wrong attribution is worse than no attribution: it sends defenders looking in the wrong direction and gives leaders false confidence.

Good intelligence is honest about what it does not know. The report states the gap, and says what would fill it — checking whether other healthcare organisations have seen the same domain or hash.

---

## How to read the report

- **Short on time?** Read the **Executive Summary** and the **Conclusion** in [`REPORT.md`](REPORT.md). Together they give you the whole story in a few minutes: what happened, what it means, and what should change.
- **Want the method?** Read Sections 1–3. They explain, in order, how data becomes intelligence, how intelligence is classified for different audiences, and how the lifecycle makes it repeatable.
- **Want the judgement?** Read Sections 4–5. They assess how mature the provider is, and turn that assessment into concrete recommendations.
- **Want the detail?** Appendix A has the three supporting tables (classification, lifecycle, maturity), and Appendix B lists the indicators and observations from the case.
- **Want the sources?** The References section lists 13 sources (see below). Every factual claim in the report is tied to one of them.

---

## Files in this repo

| File | What it is |
|---|---|
| [`README.md`](README.md) | This file — the beginner-friendly guide to the project. |
| [`REPORT.md`](REPORT.md) | The full report text, in Markdown: Executive Summary, Introduction, Sections 1–5, Conclusion, References, Appendix A (supporting tables) and Appendix B (indicators). |
| [`WORKLOG.md`](WORKLOG.md) | A log of what was done each day, and the challenges encountered along the way. |

---

## References

13 references, APA 7th. The full list is in [`REPORT.md`](REPORT.md#references). The key sources a newcomer should know:

- **NIST SP 800-150** — Johnson et al. (2016), *Guide to Cyber Threat Information Sharing*. The standard reference for how threat information should be shared.
- **CISA** — Cybersecurity and Infrastructure Security Agency (2023), *#StopRansomware Guide*, plus the 2024 joint advisory on Black Basta. Practical, official guidance on ransomware.
- **MITRE ATT&CK** — the public catalogue of attacker tactics, techniques, and procedures, used in this project to map the attack and to design behaviour-based detections.
- **ENISA** — European Union Agency for Cybersecurity (2023), *Threat Landscape: Health Sector*, and (2024), *Threat Landscape 2024*. The source for the statistic that ransomware appeared in 54% of health-sector incidents analysed between January 2021 and March 2023.
- **SANS Institute** — Lee (2019), *The Cycle of Cyber Threat Intelligence*. The source for the lifecycle approach used in Section 3.
- **Shahzadi et al. (2025)** — *Safeguarding the healthcare sector from ransomware attacks*, PeerJ Computer Science, 11, e3073. Peer-reviewed literature review; the source for the finding that ransomware in healthcare is regular, not rare, and disrupts care as well as IT systems.
- Also cited: Bianco (2016) on the Hunt Maturity Model, ThreatConnect (2019) on the Threat Intelligence Maturity Model (TIMM), Stillions (n.d.) on the Detection Maturity Level, NIST CSF 2.0 (2024), and FIRST (2024) on what CTI is.
