# Cloud Engineering Series – Style Guide v1.0

**Working Title:** Cloud Engineering Series

**Audience:** Beginners → Intermediate Linux, AWS, DevOps, SRE Engineers

**Primary Goal:**  
Teach readers **how engineers think**, not just which commands they type.

---

# 1. Writing Philosophy

Every chapter must answer four questions:

> **What?**  
> Introduce the concept.

> **Why?**  
> Explain why it matters.

> **How?**  
> Demonstrate using Linux commands.

> **When?**  
> Show real production scenarios.

Readers should always understand **why a command is used before memorizing its syntax.**

---

# 2. Storytelling Philosophy

Each chapter is a mission.

The reader is **not studying Linux.**

The reader is

- a Cloud Engineer
- a DevOps Engineer
- an SRE
- on their first production assignment.

Every command solves a real problem.

---

# 3. Mission Structure

Every mission begins with:

```
Mission Title

Estimated Time

Difficulty

Prerequisites

Mission Brief

Learning Objectives
```

Example

```
Mission 2 – Explore the Linux File System

Estimated Time
45–60 minutes

Difficulty
Beginner

Prerequisites

• SSH access
• Basic Linux login
• Mission 1 completed
```

---

# 4. Scene Structure

Every scene follows exactly this order.

```
Situation

Question

Tool

Purpose

Syntax

AWS Example

Why This Matters

Tool Evolution

AWS Tip (optional)

AWS Scenario

Interview Corner

Common Errors → Root Cause

Knowledge Check

Engineering Habit

Production Tip

Scene Summary
```

Mission Progress should appear **after Scene 2, Scene 4, and Scene 6 only** (25%, 50%, and 100%), not after every scene.

---

# 5. Heading Hierarchy

```
# Mission

## Scene

### Section

Normal Body
```

Never use four heading levels unless absolutely necessary.

---

# 6. Paragraph Rules

Maximum paragraph:

**4 lines**

Ideal paragraph:

**2–3 lines**

If a paragraph becomes longer,

split it.

---

# 7. Sentence Style

Prefer

> Professional engineers verify before acting.

Avoid

> It should be noted that professional engineers generally verify the environment before performing administrative tasks.

Short.

Direct.

Conversational.

---

# 8. Voice

Write like

> a senior engineer mentoring a junior engineer.

Never sound like

> an academic textbook.

---

# 9. Tone

Use

"You"

"You've"

"Imagine..."

"Suppose..."

Avoid

"The user"

"One should"

"It is recommended"

---

# 10. Story Continuity

Every scene should naturally lead into the next.

Example

Scene 1 ends

> Now that you've identified the operating system...

Scene 2 begins

> ...the next question is equally important.

The chapter should feel like

one investigation,

not six separate articles.

---

# 11. Linux Commands

Every command uses this format

```
pwd
```

Never inline commands inside long paragraphs if they deserve emphasis.

---

# 12. Code Blocks

Always

```
hostname

whoami

pwd
```

Never screenshot commands.

Never use colored terminals.

---

# 13. Tables

Use consistent tables.

Example

|Command|Purpose|
|---|---|

Example

| Problem | First Thing to Check |

Readers become familiar with recurring patterns.

---

# 14. Interview Corner

Always include

Question

Expected Discussion

Interview Insight

Avoid making every answer begin with

> I'd first...

Rotate naturally.

---

# 15. Common Errors

Always actionable.

Bad

Permission denied

Good

Permission denied → Verify user identity using `whoami` and `id`.

---

# 16. Knowledge Check

Maximum

3 questions.

Multiple choice.

One correct answer.

Readers should complete each scene in under two minutes.

---

# 17. Engineering Habit

Exactly one sentence.

Examples

> Verify before modifying.

> Prefer absolute paths.

> Check permissions before escalating privileges.

No explanations unless essential.

---

# 18. Production Tip

This is where we share experience.

Example

> Many engineers begin every SSH session with:

```
hostname
whoami
pwd
df -h
```

These tips should feel practical and memorable.

---

# 19. AWS Content

AWS should enhance Linux, not dominate it.

Each scene may include:

- AWS Example
- AWS Tip
- AWS Scenario

Avoid turning the book into AWS documentation.

---

# 20. Interview Focus

Every scene should answer:

> Could this appear in an AWS/DevOps interview?

If not,

remove it.

---

# 21. Mission Ending

Every mission ends with:

- Mission Debrief
- Hands-on Lab
- Mini Challenge
- Interview Challenge
- Key Takeaways
- Mission Scorecard
- Next Mission
- Mission Complete

---

# 22. Recurring Characters

Maintain continuity.

- 👨‍💼 Manager
- 👨‍💻 Reader (Cloud Engineer)

No unnecessary characters.

Keep the focus on solving problems.

---

# 23. Emoji Guide

Use emojis intentionally, not excessively.

|Emoji|Purpose|
|---|---|
|🎯|Mission / Goal|
|📍|Scene|
|💭|Situation|
|❓|Question|
|🔧|Tool|
|💡|Explanation|
|☁️|AWS|
|🎤|Interview|
|❌|Common Errors|
|🧠|Knowledge Check|
|⭐|Engineering Habit|
|🛠|Production Tip|
|📊|Progress|
|🏁|Mission Complete|

No decorative emojis.

---

# 24. Editorial Rules

Every chapter must pass these checks before publication:

- No repeated wording.
- No paragraph over four lines.
- Consistent heading hierarchy.
- Consistent table formatting.
- One idea per paragraph.
- No unexplained jargon.
- Every command tested.
- Every scene advances the story.

---

# 25. Series Identity

This is the heart of the series.

> **We don't teach Linux commands.**
> 
> **We teach engineers how to investigate, think, and solve problems using Linux.**

Every chapter should reinforce that promise.

---

# 🌟 One Addition I'd Make

I'd add a final section called **"Non-Negotiables."** These are the rules we never break:

1. **Story before syntax.** Introduce the problem before the command.
2. **Think before type.** Explain the reasoning before the solution.
3. **One mission, one narrative.** Every scene advances the same story.
4. **Production first.** Use examples that reflect real AWS/DevOps work.
5. **Interview ready.** Every chapter should help readers explain not just _what_ to do, but _why_.

---

I would save this as `STYLE_GUIDE.md` in the root of the repository, alongside `README.md` and the `manuscript/` folder. From now on, before we write any new mission, we'll check it against this guide. It will keep every book in the series consistent and make the series feel like it came from a single professional publisher rather than being written chapter by chapter.

### 26. Cloud Neutrality

- Teach **engineering principles first**.
- Use **AWS as the primary example** because it aligns with the target audience.
- When appropriate, mention the equivalent concept rather than diving into provider-specific details.
- Avoid locking the explanation to AWS terminology unless the topic is inherently AWS-specific.

For example:

|Concept|AWS|Azure|GCP|
|---|---|---|---|
|Virtual Machine|EC2|Azure VM|Compute Engine|
|Monitoring|CloudWatch|Azure Monitor|Cloud Monitoring|
|Firewall|Security Group|Network Security Group|VPC Firewall Rules|

This table doesn't need to appear everywhere—only where it genuinely helps.

---

### My vision for the series

I don't want readers to finish our books thinking:

> "I learned AWS."

I want them to think:

> **"I learned how a cloud engineer thinks. I can apply this whether I'm using AWS, Azure, GCP, or a Linux server in a data center."**

If we achieve that, our books will have a much longer shelf life than books tied to a single cloud provider.