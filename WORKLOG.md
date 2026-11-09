# Work log — Nov 8, 2026

## What I did today
- Worked through a healthcare CTI case study: a provider drowning in SIEM alerts while phishing, credential theft and early ransomware signs were happening underneath.
- Wrote the report: Executive Summary, Introduction, the five sections
  above, Conclusion, References (13, APA 7th), plus supporting tables
  and an indicators appendix.
- Kept the writing plain and human, with plain black headings.

## Challenges
- The case gave a lot of raw indicators but no attribution evidence, so
  naming a threat actor would have been guessing — I left that out
  deliberately and said why.
- The scoring in the brief added up differently in two places (110 vs 100),
  so I did not invent a rubric to resolve it.

## Nov 8, 2026 — README expansion (later the same day)

### What I did
- Rewrote the README into a long, beginner-friendly guide so that someone
  new to cybersecurity can understand the whole project without reading
  the report first.
- Added, in plain English: what CTI is (data vs information vs
  intelligence), the healthcare scenario step by step (phishing, credential
  harvesting, out-of-hours VPN logins, lateral movement, PowerShell,
  encrypted files, and alert fatigue), and a glossary of 10+ terms
  (IoC, SIEM, SOC, EHR, TIP, SOAR, EDR/XDR, UBA, FIM, ASM, plus ATT&CK,
  TTPs, threat hunting, alert fatigue, and ransomware).
- Explained each of the five sections of the report in the README:
  Data to Intelligence, Classification (Technical / Tactical / Operational /
  Strategic), the 7-phase Lifecycle, Maturity (TIMM / HMM / DML), and the
  Recommendations (Technology / Processes / People).
- Added a full `REPORT.md` with the complete report text, extracted
  directly from the original document with python-docx — headings,
  the three supporting tables, and all 13 references preserved, and no
  content invented. Title-page identification lines were left out.
- Also rewrote the README and work log earlier today to remove any
  college-specific wording, so the project is presented plainly, as a
  personal project.

### Challenges
- Keeping the README accurate while simplifying it: every plain-English
  explanation had to match what the report actually says — for example,
  the maturity levels (TIMM Level 1, HMM HM0, DML atomic-indicator level)
  and the IoCs had to be quoted exactly, not paraphrased into something
  different.
- The report itself contains a few small spelling slips in the body text
  (they were part of the original writing). I left them exactly as they
  were in `REPORT.md` rather than silently fixing them, so the report
  text stays a faithful copy of the original.
