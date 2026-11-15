# Self-check — Cyber Threat Intelligence in Healthcare

Five quick questions to test whether the ideas in this repo actually stuck.
Try answering each one in your head (or on paper) before reading the answer.
No marks, no timer — this is just for you.

---

## 1. Data, information, or intelligence?

Your hospital's firewall logs a connection to an IP address at 3 AM.
On its own, is that data, information, or intelligence — and what would
have to change for it to become intelligence?

**Answer:** On its own it is just data — a fact with no story attached.
It becomes information once you add context (for example, the IP belongs
to a server that was registered yesterday and pretends to be a hospital
login page). It only becomes intelligence when it helps someone make a
decision, like "block this IP on every hospital firewall tonight" or
"check whether any other machine talked to it." If it does not change
what anyone does, it is not intelligence yet, however interesting it is.

## 2. Which IoC would you chase first?

You have three indicators from last night and time to check only one
properly before your shift ends:

- a) A file hash that matches a known ransomware sample exactly.
- b) A domain name that looks almost like your hospital's patient portal.
- c) An IP address in a country your hospital never does business with.

Which one do you pick, and why?

**Answer:** Start with (a), the file hash. An exact hash match is the
strongest evidence of the three — it is either the same file or it is
not, there is not much grey area. The lookalike domain (b) comes second:
it is suspicious, but lots of similar domains are harmless, so you need
to check what it actually does. The foreign IP (c) is the weakest on its
own, because plenty of normal traffic comes from unexpected places.
Score indicators 1–3 like the checklist in `INDICATOR_CHECKLIST.md`
suggests, and always chase the highest score first.

## 3. Spot the wrong step in the lifecycle

A junior analyst does the CTI lifecycle in this order: collect alerts,
write a report for management, plan what to collect, then share the
findings with other hospitals. What is wrong with that order?

**Answer:** The planning step comes first, not third. The lifecycle
starts with direction — deciding what you actually need to know — and
only then do you collect. Collecting first means you drown in alerts
that nobody asked for. Reporting before you have analysed anything is
also backwards: the report should come after analysis, and sharing with
other hospitals (dissemination) is the last step, once you are sure
about what you found.

## 4. Strategic, tactical, or operational?

Match each one to the right level:

- a) "Ransomware groups are targeting hospitals this quarter."
- b) "Block these 12 domains at the firewall this week."
- c) "The phishing email arrived at 09:14 and the link points here."

**Answer:** (a) is strategic — it is for managers deciding where to
spend money and attention over months. (b) is tactical — it is for the
security team planning what to block and detect over days or weeks.
(c) is operational — it is about one specific attack, right now, for
the people responding to it. Mixing them up is a common mistake: telling
a manager about a single email wastes their time, and telling an
analyst "ransomware is bad" gives them nothing to do.

## 5. What does "no threat actor named" actually protect?

The report in this repo deliberately does not name a threat actor.
What goes wrong if a team names one too early, based on weak evidence?

**Answer:** Two things. First, the team starts defending against the
wrong attacker — they look for that group's tools and habits and miss
what is really happening. Second, naming an actor is a claim that other
people will repeat, and once it is in a report or a news story it is
very hard to take back, even if it turns out to be wrong. Attribution
needs strong evidence and it needs to be checked. Until then, describe
what you can prove (the indicators, the behaviour) and leave the "who"
blank.

---

## How did you do?

- **5 out of 5:** you have got the core ideas. Try the exercises in
  `EXERCISES.md` next — they are harder.
- **3–4:** re-read the section you missed in the `README.md` or the
  full `REPORT.md`. The answer is always in there.
- **0–2:** start from the top of the `README.md` and read it through
  once, slowly. CTI makes a lot more sense the second time round.
