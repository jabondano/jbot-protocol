# Discovery Interview Guide

> **For Codex / ChatGPT users:** Upload this file and say "Run the discovery interview." You can answer by voice. At the end, generate the Master Context File using the format at the bottom of this document.

---

## Interview Details

**Client:** [Company Name]
**Interviewee:** [Name, Title]
**Date:** [Date]

---

## How to Run This Interview

This is a structured conversation, not a form. Ask questions naturally. Follow interesting threads. Not every question applies to every client — use judgment. The goal is to understand how the business actually works, not how the client thinks it should work.

**Start with:**
> "Let's have a conversation about your business — how it runs, what takes the most time, what's working and what isn't. There are no wrong answers. Ready?"

---

## Section 1 — The Business

1. Tell me about [Company Name]. What does it do, in your own words?
2. Who are your main customers right now?
3. What does a typical week look like for you? Walk me through it.
4. How many people are on the team? What does each person do?
5. What's the single biggest thing that eats your time every week?
6. If you could fix one thing about how the business runs, what would it be?

---

## Section 2 — Operations & Data Flows

7. What are the highest-volume activities — things you do many times per day or week?
8. How does information move through your business? (Who tells who what, and how?)
9. Where does your most important business data live today? (Spreadsheets, software, people's heads?)
10. What data do you wish you had but don't?
11. What reporting or analysis do you produce regularly? How long does it take?
12. Is there data that only works when a specific person or laptop is available?

---

## Section 3 — Pain Points

13. Where do things break down most often?
14. What would you never want to have to think about again?
15. What do new hires struggle with most?
16. What happens when the key person is out sick or traveling?
17. What's the most manual, repetitive task your team does?

---

## Section 4 — Technology Today

18. What apps or software does the team use every day?
19. How do these systems connect to each other? (Manual, automated, not at all)
20. What's your biggest technology frustration?
21. Do any of your key systems have APIs or data exports?
22. Do you have internal technical resources? (IT, someone who handles software)
23. What tools would you never give up?

---

## Section 5 — Communication & Team Interface

24. How does the team communicate day to day? (WhatsApp, Telegram, email, calls?)
25. Does the team work primarily on desktop or mobile?
26. Are there people in the field (outside the office) who need information on the go?
27. How do remote or field staff report what they're seeing?
28. Is there information your team needs but has to ask someone for every time?

---

## Section 6 — AI & Automation Readiness

29. Is anyone on the team using AI tools today? (ChatGPT, Copilot, etc.)
30. What are they using it for? What's working, what's not?
31. Have there been previous automation attempts? What happened?
32. How comfortable is the team with learning new tools? (1-10)
33. Are there any concerns about AI usage? (Privacy, accuracy, job security)

---

## Section 7 — The Dream State

34. Imagine it's one year from now and the business is running better than ever. What's different?
35. What would it mean for you personally if you had more time back?
36. If AI could handle one thing so you never had to think about it again — what would that be?
37. What would need to be true for you to feel confident this was worth the investment?

---

## End of Interview — Generate the Master Context File

Once the interview is complete, generate a structured document using this exact format:

```
# [Company Name] — Master Context File
Generated: [date]
Interview conducted by: JBOT Protocol Discovery Agent

## Business Overview
[2-3 sentences: what they do, who their customer is, business model]

## Team
[List each person, role, and what they're responsible for]

## How the Business Runs Today
[Key workflows, information flows, who does what and how often]

## Technology Stack (Current)
[All systems, apps, data sources — including what's manual vs. automated]

## Priority Pain Points (Ranked)
1. [Most painful / most time-consuming]
2. 
3. 

## Data Inventory
[What data exists, where it lives, how often it updates, who can access it]

## Communication Patterns
[How the team communicates, what devices/apps, field vs. office split]

## AI Readiness Assessment
[What they're already using, comfort level, concerns, past attempts]

## Immediate Opportunities (Quick Wins — 30 days)
[2-3 things that could be automated or improved immediately]

## Recommended Bot Architecture (Draft)
[Based on what was shared, suggest which bots to build first and why,
who each bot serves, and what data it needs]

## Interface Recommendation
[Telegram / Discord / WhatsApp — based on team size, mobile usage, and comfort]

## Cost Estimate
[Rough monthly operating cost: VPS + API + database + tools]

## Open Questions
[Things we still need to find out before building]
```

Save this file as `[company-slug]-master-context.md` and share it with the JBOT Protocol team.

---

*JBOT Protocol — jabondano.co*
