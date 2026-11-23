# Worked Example: The 2 a.m. VPN Login

The README tells the whole attack as a story. This file slows one moment down and works it through properly: a single alert, from raw data to a decision. It is the clearest way to see what "turning data into intelligence" actually means in practice.

## The alert

Tuesday, 2:14 a.m. The SIEM raises an alert:

```
VPN login succeeded — user: nurse.jones — source IP: 185.220.101.1 — time: 02:14
```

On its own, this is a fact with no story attached. Thousands of alerts like it arrive every day. Most of them are harmless. The question is what to do with this one, and the answer depends on how far you take it through the three levels.

## Level 1: Data

What you have:

- A username: `nurse.jones`
- An IP address: `185.220.101.1`
- A time: 2:14 a.m.
- A result: login succeeded

That is all. It does not tell you whether anything is wrong. A nurse logging in at night could be on a night shift, or checking something from home. At this level, the only honest thing to say is "a login happened." A SOC that stops here either investigates everything — which is impossible — or investigates nothing.

## Level 2: Information

Now the facts get checked, sorted, and given context:

- **The IP is foreign.** `185.220.101.1` geolocates to another country. Nurse Jones works at a clinic in this country and has never logged in from abroad.
- **The time breaks the pattern.** Her normal logins are 7 a.m. to 7 p.m. on shift days. She is not on the night rota this week.
- **The domain from the phishing emails is connected.** Two days ago, phishing emails impersonating a health service login page were sent to clinical staff. The fake page was `secure-login-health.com`. Three staff entered credentials there — and the SOC never confirmed who.
- **The device is unfamiliar.** The login came from a device the VPN has never seen for this user.

Now the alert has a label: *a stolen credential is probably being used by someone who is not nurse.jones.* That is information — data with context. It is already much more useful than the raw alert, but it still does not say what to do.

## Level 3: Intelligence

Intelligence is information analysed against a real question. The question here is the same one as the whole case:

> *Are we in the early stages of a ransomware attack, and what should we stop first?*

Read this alert together with the other pieces:

1. Phishing emails went to clinical staff two days ago.
2. Credentials were harvested on a fake login page.
3. Now a login at 2 a.m. from a foreign IP, on an unfamiliar device.
4. If this is the attacker, the next steps are predictable from the pattern: movement between servers, PowerShell on endpoints, then encryption on a shared drive.

The judgement: **this is very likely the attacker using a stolen credential, and the window to act is now — before lateral movement starts.**

And the judgement supports specific decisions, in order:

1. **Disable the VPN session and reset nurse.jones's password immediately.** This cuts off the access the attacker is using right now.
2. **Block `185.220.101.1` at the firewall.** Cheap, fast, and stops that address being reused.
3. **Check whether the other two staff who entered credentials on the fake page have had unusual logins too.** One compromised account is a warning; three is a breach in progress.
4. **Start hunting, not just alerting.** Search endpoint logs for PowerShell executions and look for movement between servers — the next steps in the pattern — instead of waiting for the SIEM to flag them.

Notice what changed between the levels. At Level 1, the alert was one of thousands. At Level 2, it was suspicious. At Level 3, it became a decision with a deadline: act now, in this order, because the next stage of the attack is already predictable.

## What would have gone wrong without this

If the SOC had treated the alert as just another login:

- The attacker would have had hours — maybe a full day — of quiet access before the next visible sign (mass encryption) appeared.
- By then the response would be damage control: isolating servers, restoring from backups, telling clinical managers the EHR is down.
- The cost of acting late is not just technical. A clinic cannot switch its systems off during an attack — people need care.

That is the central point of the whole project: the failure was not a lack of data. Every piece was visible. The failure was never stepping back to read the pieces together as one attack.

## Try it yourself

Pick any one alert from your own experience — a failed login, a strange email, a slow server — and run it through the three levels:

1. **Data:** what are the bare facts, with no interpretation?
2. **Information:** what context changes the meaning — who, when, where, what is normal for this user or system?
3. **Intelligence:** what question does this answer, and what decision does it support? What should happen first, and what happens if nobody acts?

If you cannot get past Level 1, you do not have an intelligence problem — you have a context problem. The fix is usually boring: know what normal looks like for your users, and keep the context (shift patterns, known devices, recent phishing) somewhere the person reading the alert can actually see it.
