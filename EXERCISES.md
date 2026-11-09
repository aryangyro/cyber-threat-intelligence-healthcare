# Practice exercises — Cyber Threat Intelligence in healthcare

Short exercises to try after reading the README and the report. You do
not need any tools for the first three — a pen and paper (or a text
file) is enough. The point is to practise the thinking, not to run
software.

## Exercise 1: Write your own intelligence note

Imagine you are the analyst on shift and the SOC lead asks you, in
plain words, what is going on. Using only the indicators and
observations from this case, write a note of no more than five
sentences that covers:

1. What you think is happening.
2. What evidence supports it.
3. What you are not sure about yet.
4. The one action you would take first.

Then compare your note with the Executive Summary in `REPORT.md`.
Good intelligence notes are short, say what is uncertain, and end
with a decision — if your note is just a list of indicators, rewrite
it until it tells the reader what to do.

## Exercise 2: Triage drill

Six alerts land in the queue at the same time. Sort them into
**investigate now**, **watch**, and **probably harmless**, and write
one line of reasoning for each:

1. A nurse logs into the VPN at 2 a.m. from a foreign IP address.
2. A receptionist logs into the VPN at 8 a.m. from the clinic's own
   network.
3. PowerShell runs on a doctor's computer during a scheduled IT
   maintenance window, started by an IT admin account.
4. PowerShell runs on a reception computer at midnight, started by a
   receptionist's account, and downloads a file from the internet.
5. A file on a shared drive is encrypted, and the same thing is
   happening to other files in the same folder.
6. A firewall blocks a connection to a well-known software update
   server.

The lesson this is meant to teach: a single alert rarely tells you
much on its own. It is the combination — and the context, like the
time of day and whose account it is — that turns an alert into a
lead. Alerts 1, 4, and 5 are the ones that matter here, and together
they are much stronger than any one of them alone.

## Exercise 3: Map one step to MITRE ATT&CK

Pick one step from the attack (phishing, stolen credentials, the
odd-hours VPN login, lateral movement, PowerShell, or the file
encryption) and look it up in the MITRE ATT&CK matrix. Write down:

- The tactic it belongs to (for example, Initial Access, or
  Credential Access).
- The technique name.
- One detection idea for that technique that does **not** depend on
  a single IP address or file hash.

If you do this for all six steps, you have the start of a
behaviour-based detection list — the thing Section 4 of the report
says this provider is missing.

## Exercise 4: Which indicator is the weakest?

Look at the three IoCs in the README (the IP address, the domain,
and the file hash) and rank them from easiest to hardest for the
attacker to change. Then answer:

- If the attacker changes the easiest one tonight, which of your
  detections still work?
- What would you detect instead, so that changing an IP or a domain
  does not blind you?

This is the core argument of the report in one exercise: indicators
are useful, but detections built on behaviour last longer.
