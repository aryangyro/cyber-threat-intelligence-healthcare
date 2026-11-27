# Glossary — Cyber Threat Intelligence (Healthcare)

The README already has a short glossary of the terms used most in this
project. This file goes further: the extra terms you will meet when you
start reading real threat reports, feeds and sharing groups. Plain
English, one line or two per term, in alphabetical order.

## The terms

- **Campaign** — a series of attacks over weeks or months that share the
  same goal, the same tools, or the same victims. One phishing email is
  an incident; a year of phishing against hospitals is a campaign.
- **Command and control (C2)** — the channel an attacker uses to send
  instructions to malware already inside a network, and to receive stolen
  data back. Blocking C2 traffic can stop an attack even after the
  malware is in.
- **Confidence level** — how sure the analyst is about a judgement: low,
  medium or high. Good intelligence always says how confident it is,
  instead of pretending to be certain. A judgement based on one log line
  is low confidence; the same judgement backed by three independent
  sources is high.
- **Cyber Kill Chain** — a model of an attack in seven stages: recon,
  weaponisation, delivery, exploitation, installation, command and
  control, and actions on objectives. It helps defenders think about
  where to stop an attack early, not just at the end.
- **Data exfiltration** — data being copied out of an organisation by an
  attacker. Ransomware groups often steal patient records before
  encrypting them, so they can threaten to publish them as well.
- **Diamond Model** — a way of describing an attack with four points:
  adversary, capability, infrastructure and victim. It is useful because
  it shows the relationships: the same adversary using a new capability
  against a new victim is still the same problem.
- **Dwell time** — how long an attacker is inside a network before being
  found. Shorter is better. In healthcare it is often weeks or months,
  because the signs are buried in alert noise.
- **Enrichment** — adding context to a bare indicator. An IP address on
  its own is just numbers; enriched, it becomes "registered two days
  ago, hosted in X, seen in three other hospital incidents this month".
  Enrichment is what turns data into information.
- **False negative** — a real attack that the defences missed. The
  opposite of a false positive, and usually the more dangerous of the
  two, because nobody knows it happened.
- **False positive** — an alert that looks like an attack but is not.
  Too many of these is what causes alert fatigue: analysts stop trusting
  the alerts at all.
- **Indicator decay** — the fact that indicators go stale. An IP address
  that was malicious last month may belong to someone innocent today.
  Old indicators should be retired, not kept in blocklists forever.
- **ISAC — Information Sharing and Analysis Center** — a group where
  organisations in one sector, such as healthcare, share threat
  information with each other. The provider in this case study could
  have learned about the phishing domain from other hospitals first.
- **Observable** — a single fact seen in the data, like "this IP was
  contacted at 2 a.m.". An observable becomes an indicator when it is
  judged to be evidence of an attack.
- **OSINT — Open-Source Intelligence** — intelligence gathered from
  public sources: news, social media, public records, breach
  disclosures. It is free, legal, and often the first place a new threat
  shows up.
- **Pivoting** — starting from one known indicator and finding connected
  ones: the same domain leads to another IP, which leads to another
  domain. Analysts pivot to map the full shape of an attack.
- **Priority Intelligence Requirement (PIR)** — a question the team
  agrees it needs answered, written down before collection starts.
  "Are we being targeted by ransomware groups?" is a PIR. Without PIRs,
  teams collect whatever feeds are available, whether or not it helps a
  decision.
- **STIX — Structured Threat Information Expression** — a standard format
  for writing threat intelligence so tools can share it automatically,
  instead of as prose in emails and PDFs.
- **TAXII — Trusted Automated Exchange of Indicator Information** — the
  standard way STIX data is sent between organisations and tools. STIX
  is the language, TAXII is the delivery service.
- **Threat actor** — a person or group behind attacks. Analysts group
  them by motive: financially motivated criminals, hacktivists, insiders,
  and state-backed groups behave differently, so the response differs.
- **TLP — Traffic Light Protocol** — a simple sharing rule attached to
  intelligence: RED means do not share it outside the room, AMBER means
  share it only inside your organisation, GREEN means share it inside
  the community, CLEAR means share it freely. It stops sensitive details
  leaking further than intended.

## Terms that are easy to mix up

- **Data vs information vs intelligence** — data is a fact, information
  is a checked and labelled fact, intelligence is information analysed
  against a question so someone can decide. Most teams are drowning in
  the first two and short of the third.
- **Indicator vs observable** — an observable is something you saw; an
  indicator is an observable you have judged to be evidence of an
  attack. Not every strange log line is an indicator.
- **Threat hunting vs alert triage** — triage is reacting to alerts that
  already fired. Hunting is going looking for attacks the alerts missed.
  A team that only triages will never find the quiet intruder.
