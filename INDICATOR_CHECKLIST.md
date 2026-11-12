# Indicator Triage Checklist

A short checklist for working through indicators of compromise (IoCs)
from the healthcare ransomware scenario in this repo. Use it when a new
alert lands and you need to decide, quickly, whether it is worth
escalating.

## Before you trust an indicator

- [ ] **Source:** where did this indicator come from — a firewall log,
  an endpoint alert, a threat feed, a colleague? Note it down.
- [ ] **Freshness:** when was it first seen? An IP from a feed that is
  six months old is far less useful than one seen yesterday.
- [ ] **Context:** does it fit this organisation? A hash for a Windows
  malware family means little if the alert came from a Linux server.
- [ ] **Corroboration:** does any *other* source mention the same
  indicator? One source is a lead, two independent sources are a case.

## Scoring an indicator

Give each indicator a quick score before you act on it:

| Question | Low (1) | Medium (2) | High (3) |
| --- | --- | --- | --- |
| How specific is it? | A common port or domain | A known-bad IP range | An exact file hash |
| How recent is it? | Months old | Weeks old | Seen in the last day |
| How bad is the impact if true? | A scan | A phishing attempt | Ransomware staging |

A total of 7–9 means escalate now. 4–6 means watch it and gather more
evidence. 1–3 means log it and move on.

## After triage

- [ ] **Record the decision** — what you decided, why, and the score.
  Intelligence nobody wrote down is intelligence nobody can reuse.
- [ ] **Share what matters** — a high-scoring indicator should reach
  the rest of the team, not sit in one person's notes.
- [ ] **Expire old indicators** — set a review date. An indicator that
  is never re-checked slowly turns into noise.

## Worked example (from the scenario)

An alert flags an outbound connection to an IP that appears on a threat
feed as a ransomware command-and-control server.

- Source: firewall log + public feed (two sources — good).
- Freshness: the feed entry is 3 days old, and the connection happened
  20 minutes ago.
- Context: the connection came from a nurse's workstation, not a server.
- Score: specificity 3 (exact IP), recency 3, impact 3 — total 9.
- Decision: escalate immediately, isolate the workstation, and check
  what else that machine talked to in the last 24 hours.

## Common triage mistakes

1. **Chasing every alert equally.** Not every indicator deserves the
   same effort. Score first, then spend time where the score is high.
2. **Trusting a feed blindly.** Feeds contain false positives. Check
   the indicator against your own logs before acting on it.
3. **Forgetting to write it down.** If the triage is not recorded, the
   next person starts from zero — and may make the opposite call.
