# Applying Cyber Threat Intelligence to Improve Detection, Analysis, and Decision-Making in a Healthcare Environment

> The full text of the report in this project, reproduced in Markdown. Body text is 1,945 words. Spelling in the body is reproduced exactly as written in the original document.

# Executive Summary

This report is about a medium-sized healthcare provider whose security operations centre is overwhelmed by alerts while a real attack is taking shape. In the past month, phishing emails have targeted clinical staff, some credentials have been harvested, and the SOC has seen strange VPN logins outside working hours, movement between internal servers, PowerShell running on endpoints, and many encrypted files on a shared drive. The indicators already collected are an IP address, 185.220.101.1, a domain, secure-login-health.com, and a file hash. On their own they are just facts. Read together, they look like the early stages of a ransomware attack.

The report shows how raw data becomes information and then intelligence, and why that last step keeps failing here. It sorts the indicators into technical, tactical, operational and strategic intelligence, runs the intelligence lifecycle, and assesses maturity. It ends with practical recommendations on technology, processes and people, tied to what actually happened. The short answer is that the provider does not mainly need more alerts. It needs fewer, better ones, and people with the time and training to act on them.

# Introduction

Healthcare is an attractive target becuase patient records are valuable and a clinic cannot simply switch its systems off during an attack. The European Union Agency for Cybersecurity found ransomware in 54% of health-sector incidents it analysed beetween January 2021 and March 2023, with patient data and electronic health records among the most targeted assets (ENISA, 2023). A recent review reaches a similar conclusion: ransomware in healthcare is regular, not rare, and it disrupts care as well as IT systems (Shahzadi et al., 2025).

This report is written as a cyber thraet intelligence analyst asked to make sense of what the SOC is seeing. It does not claim to identify a named attacker group; the evidence does not support that.

# Task 1: From Data to Intelligence

Data is the starting point, and the least useful on its own. An IP address, a domain or a file hash is a fact with no story attached. Here, 185.220.101.1, secure-login-health.com and the SHA-256 hash are data. The purpose of this stage is simply to record what was observed, accurately and in a searchable form. A firewall log showing a connection to 185.220.101.1 at 2 a.m. is worth keeping, but it does not yet tell anyone what to do.

Information is data that has been checked, sorted and given context. The IP is looked up and found to be foreign, the domain is checked and found to be newly registered and impersonating a health login page, and the hash matches known malware. Now the facts have labels: malicious, suspicious, known-bad. A useful example is a short note saying the domain was used in phishing emails to nurses and reception staff, and three of them entered credentials. That connects a technical fact to a person and a place, and it is already more valuable than a list of indicators.

Intelligence is information analysed against a question and turned into a judgement a decision-maker can act on. The question here is: are we in the early stages of a ransomware attack, and what should we stop first? Read together, the pattern looks like one: phishing to steal credentials, logins outside hours, movement between servers, PowerShell on endpoints, then files being encrypted. That sequence, rather than any single indicator, is the inteligence. It supports a specific decision: isolate the affected servers, reset passwords for the staff who were phished, and block the domain and IP.

Organsiations often fail to make that last jump for a few connected reasons. In this provider, the clearest is volume: thousands of SIEM alerts a day, many of them false positives, teach analysts to ignore alerts rather than investigate them. A second reason is that data is collected without a question behind it. A third is fragmentation: phishing, VPN logs, endpoint alerts and file encryption are held in diffrent tools and looked at by different people, so nobody sees the whole sequence. Finally, intelligence is treated as a product to buy, rather than a process that needs analysts with time to think and managers who will act.

# Task 2: Cyber Threat Intelligence Classification

The same indicators mean different things to different people, which is why CTI is usually split into four levels, summarised in Appendix Table A1. Technical intelligence is the most concrete: it lets tools and analysts block, detect or hunt for something specific, right now, and its audience is the SOC and detection engineers. Tactical intelligence is about how the attacker works, thier tactics, techniques and procedures, rather than the addresses they used this time. It helps defenders recognise the same behaviour even when the indicators change, and supports decisions about which detections to build next, for example around PowerShell and unusual VPN use. Operational intelligence is about a specific campaign, who is behind it and how far they may go, and its audience runs secuirty operations and incident response. Strategic intelligence is the least technical: it informs senior leaders about risk over months, and supports decisions on budgets and risk appetite.

In this case the operational picture is incomplete, and it is importent to say so. The pattern fits a financially motivated ransomware operation, and ENISA (2023) found most health-sector attacks were financially motivated, but there is no evidence yet to name a group or to say whether data was stolen as well as encrypted. That gap can only be filled from outside the provider, such as whether other healthcare organisations have seen the same domain or hash (Johnson et al., 2016).

# Task 3: Apply the Cyber Threat Intelligence Lifecycle

The intelligence lifecycle is the process that turns the work in Tasks 1 and 2 from a one-off report into something the provider does routinely (Lee, 2019). The case study uses seven phases, summarised in Appendix Table A2. Planning is where this provider needs to start, because everything else depends on it. Without a clear statement of what the SOC most needs to know, collection is driven by what feeds are availible rather than by what decisions need making. A short list of priority intelligence requirements, written with the CISO and clinical operations, would change that, with phishing and ransomware at the top.

Collection then becomes a question of what can answer those requirements: internal telemetry from the SIEM, EDR, email gateway and VPN logs, plus external feeds and sharing with other healthcare providers. Processing is the step that makes analyis possible, cleaning, normalising and enriching raw data and putting it in one searchable place. Analysis is where data becomes intelligence, by testing a hypothesis and being honest about confidence. Production means writing the result for its audience, a short blocking list for the SOC and a plain-language briefing for senior leaders. Dissemination is getting those products to the right people quickly. Feedback, the phase most likely to be skipped, is asking whether the product helped and using the answer to sharpen the next cycle.

# Task 4: Threat Intelligence Maturity Assessment

Three models are useful here because they measure different things, and together they give a clearer picture than any one alone, as summarised in Appendix Table A3. The Threat Intelligence Maturity Model (TIMM) looks at the CTI programme as a whole, on five levels from Initial to Optimising (ThreatConnect, 2019). This provider is at Level 1, Initial, and only just there. It has a SIEM and indicators, but no central repository, no documented process for turning data into intelligence, and no evidence that intelligence is routinely produced or consumed.

The Hunt Maturity Model (HMM) asks how good the organisation is at proactively looking for threats, from HM0, where it relies entirely on automated alerting, to HM4, where successful hunts become automated detections (Bianco, 2016). This provider is at HM0: there is no sign of hunting, because the team is too busy triaging alerts. The Detection Maturity Level (DML) looks at the detections themselves (Stillions, n.d.). At the lowest level they are based on atomic indicators, a single IP or hash, wich an attacker can change quickly. Higher levels are based on tools and then on tactics, techniques and procedures. Here the provider is mostly at the atomic level, with no sign of behaviour-based detections. The improvement is to build a small number of tested, behaviour-based detections tied to the ATT&CK techniques seen here (MITRE Corporation, n.d.).

Read together, the three models tell the same story. TIMM says the programme is not yet a programme. HMM says there is no proactive hunting. DML says the detections that do exist are fragile. That is also hopeful, because the next steps are clear and do not require a large purchase first: a named owner, a central repository, a few use cases, a weekly hunt, and a handful of behaviour-based detections would move the provider up a level on all three at once.

# Task 5: Intelligence-Driven Recommendations

On technology, the first step is a threat intelligence platform, not as a place to buy more feeds, but as the single repository the provider does not have, where indicators, context and analyst notes can live together. A SOAR tool would help next, by automating repetitive triage such as enriching an IP or hash and opening a ticket. The case for EDR or XDR is strongest here: the PowerShell executions and lateral movement are endpoint behaviours, and without endpoint visibility the team is guessing. User behaviour analytics would flag the specific anomaly in this case, a VPN login outside hours from a foreign IP. File integrity monitoring would have caught the mass encryption on the shared drive earlier, and attack surface management would help the provider keep track of which clinics and systems are exposed.

On processes, the most important change is to integrate CTI into detection engineering, so every new piece of intelligence ends up as a detection, a rule or a hunt, not just a report that is read once. Incident responce also needs a short, practised playbook for ransomware (CISA, 2023): who decides to isolate a server, who tells clinical managers, and when to call in outside help. Purple team exercises would test whether the new detections actually work, and threat modelling, focused on the EHR platform and the VPN, would help the provider decide what to protect first.

On people, SOC analysts need training specific to this case, not generic awareness training, so they can recognise phishing, investigate a VPN anomaly and write a short intelligence note. A CTI analyst role, even if it starts as part of someone's existing job, would give the programme an owner, which is the single most important gap in Task 4. Executive awareness matters too, because senior leaders need to understand ransomware as a business risk, not just an IT problem. Finally, threat hunting capability needs to be built deliberately, with time protected and results shared.

# Conclusion

This case is not really about a single IP address, domain or hash. Those matter, and they should be blocked and searched for, but they are the easiest part of the attack to change. The harder problem is that the provider had the data and did not turn it into intelligence in time. The phishing, credential theft, out-of-hours logins, lateral movement and encrypted files were all visible, in different tools, to different people, and nobody put them together until the attack was well under way.

What would change that is not a single purchase, but a small set of connected changes: a central place to store intelligence, a named owner, a lifecycle that is actually followed, a few behaviour-based detections, a weekly hunt, and training that matches the threats the provider actually faces. Those steps would make the next attack easier to spot, easier to explain to senior leaders, and easier to stop before it reaches the shared drive.

# References

- Bianco, D. J. (2016). A framework for cyber threat hunting (White paper). Sqrrl Data, Inc.
- CISA. (2023). #StopRansomware guide. Cybersecurity and Infrastructure Security Agency.
- CISA, Federal Bureau of Investigation, & U.S. Department of Health and Human Services. (2024). #StopRansomware: Black Basta. Cybersecurity and Infrastructure Security Agency.
- European Union Agency for Cybersecurity. (2023). ENISA threat landscape: Health sector. ENISA.
- European Union Agency for Cybersecurity. (2024). ENISA threat landscape 2024. ENISA.
- Forum of Incident Response and Security Teams, Cyber Threat Intelligence Special Interest Group. (2024). What is cyber threat intelligence? FIRST.
- Johnson, C., Badger, L., Waltermire, D., Snyder, J., & Skorupka, C. (2016). Guide to cyber threat information sharing (NIST Special Publication 800-150). National Institute of Standards and Technology. https://doi.org/10.6028/NIST.SP.800-150
- Lee, R. M. (2019). The cycle of cyber threat intelligence. SANS Institute.
- MITRE Corporation. (n.d.). MITRE ATT&CK. https://attack.mitre.org/
- National Institute of Standards and Technology. (2024). The NIST Cybersecurity Framework (CSF) 2.0 (NIST CSWP 29). U.S. Department of Commerce.
- Shahzadi, A., Ishaq, K., Dogar, A. B., Khan, J. A., Mylonas, A., Nawaz, N. A., Yasin, A., & Khan, F. A. (2025). Safeguarding the healthcare sector from ransomware attacks: Insights from a literature review. PeerJ Computer Science, 11, e3073. https://doi.org/10.7717/peerjcs.3073
- Stillions, R. (n.d.). Detection maturity level [Framework].
- ThreatConnect. (2019). Threat intelligence maturity model (TIMM) [White paper]. ThreatConnect, Inc.
# Appendix A: Supporting Tables

Table A1. Classification of the indicators in this case (Task 2).

| Level | Purpose | Audience | In this case | What is still needed |
| --- | --- | --- | --- | --- |
| Technical | Let tools and analysts block, detect or hunt for a specific indicator now | SOC analysts and detection engineers | Block 185.220.101.1, block secure-login-health.com, search endpoints for the hash | First-seen dates, whether the indicator is still active, and whether it has appeared elsewhere |
| Tactical | Help defenders recognise the same behaviour when indicators change | Detection engineers, incident responders, threat hunters | Map phishing, credential theft, valid-account VPN use, lateral movement, PowerShell and encryption to ATT&CK, and build detections for those behaviours | Which PowerShell commands ran, which servers were reached, and which tools were used |
| Operational | Explain a specific campaign: who is behind it, what they want, how far they may go | Security operations and incident response managers | Decide on resourcing, whether to take systems offline, and how to brief clinical managers | Whether other healthcare providers saw the same domain or hash, and the attacker's usual demands |
| Strategic | Inform senior leaders about risk over months, not hours | Executives and the board | Decide budgets, risk appetite, and what to prioritise, such as protecting the EHR platform | Trends in healthcare attacks, the cost of clinical downtime, and the likelihood of a repeat attack |

Table A2. The CTI lifecycle, phase by phase (Task 3).

| Phase | Objectives | Stakeholders | Tools | Expected output | Implementation challenge |
| --- | --- | --- | --- | --- | --- |
| 1. Planning & Direction | Agree what the SOC most needs to know, and what decisions intelligence must support | CISO, SOC lead, clinical operations, senior management | Priority intelligence requirements, a short list of use cases (phishing, ransomware) | A written set of requirements and priorities | Requirements are vague, or set by the tool rather than by the decisions that need making |
| 2. Collection | Gather raw data that can answer those requirements, from inside and outside | SOC analysts, IT, external feeds, ISACs, vendors | SIEM, EDR, email gateway, VPN logs, commercial and open-source feeds | Raw data, stored and searchable | Too much data, too little of it relevant; feeds are bought before requirements are set |
| 3. Processing | Turn raw data into a usable form: clean, normalise, deduplicate and enrich it | SOC analysts, TIP administrators | Threat intelligence platform, enrichment services, STIX/TAXII | Clean, enriched information with context | Manual work, inconsistent formats, and no single repository |
| 4. Analysis | Test hypotheses, correlate events, and turn information into judgements | CTI analyst, threat hunters | ATT&CK, Diamond Model, kill chain, analyst tradecraft | Analytical judgements with confidence levels | Analysts have no time to think; analysis is skipped in favour of triage |
| 5. Production | Write the product for its audience, in the right format and length | CTI analyst | Report templates, briefings, dashboards | A finished product: a blocking list, a report, or a briefing | Reports are too long, too technical, or written for the wrong audience |
| 6. Dissemination | Get the product to the right people, at the right time, with handling rules | CTI analyst, SOC lead, communications | TIP, SOAR, email, briefings, Traffic Light Protocol markings | Intelligence received and understood by its audience | Intelligence is shared too widely, too late, or not at all |
| 7. Feedback & Continuous Improvement | Ask whether the product helped, and use the answer to sharpen the next cycle | All consumers, SOC lead, CISO | Feedback forms, after-action reviews, metrics (time to detect, time to act) | Refined requirements and better products next time | Feedback is never collected, so the same mistakes repeat |

Table A3. Maturity assessment summary (Task 4).

| Model | Current level | Justification from this case | Recommended next step |
| --- | --- | --- | --- |
| TIMM | 1 — Initial | No central repository, no documented process, intelligence is not routinely produced or consumed; alerts are ignored and response is reactive | Name a CTI owner, build a central repository, define a small set of use cases and measure against them |
| HMM | HM0 — Initial | No proactive hunting; the team relies entirely on automated alerting and is too busy triaging to hunt | Protect a small weekly hunt slot, start with PowerShell and out-of-hours VPN use, and record what was found |
| DML | Atomic-indicator level | Detections are based on single IPs, domains and hashes, which an attacker can change quickly; no behaviour-based detections are evident | Build a small number of tested, behaviour-based detections tied to the ATT&CK techniques seen here |

# Appendix B: Indicators in this case

Indicators of compromise provided in the scenario: IP address 185.220.101.1; domain secure-login-health.com; SHA-256 hash a1b2c3d4e5f67890abcdef1234567890abcdef1234567890abcdef1234567890. Observations: phishing emails targeting clinical staff, credential harvesting, abnormal VPN logins outside business hours, lateral movement between internal servers, PowerShell executions on endpoints, large volumes of encrypted files on a shared drive, and SIEM alerts ignored because of false positives. These indicators are reproduced here for reference; they are not claimed to be attributed to a named threat actor in this report.
