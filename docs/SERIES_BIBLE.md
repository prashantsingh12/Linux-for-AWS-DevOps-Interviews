# SERIES_BIBLE.md

# Series Bible

## Cloud Engineering Interview Series

**Series status:** Active  
**Current book:** Linux for AWS & DevOps Interviews  
**Series philosophy:** Teach engineers how to think, investigate, verify, and solve problems.

---

## 1. Series Promise

This series is not a collection of command references or certification notes.

It is a practical journey through the skills an engineer needs to work confidently in modern cloud and DevOps environments.

The reader should finish each book thinking:

> **I understand how an engineer approaches the problem, not just which command or tool to use.**

The skills should remain useful across AWS, Azure, GCP, and traditional data-center environments.

---

## 2. Target Reader

The series is written for:

- Beginners entering Linux, cloud, or DevOps roles.
- IT professionals moving toward AWS, DevOps, or SRE roles.
- Experienced infrastructure engineers preparing for interviews.
- Engineers who know commands and tools but want stronger troubleshooting and interview reasoning.

The writing should assume curiosity, not prior mastery.

---

## 3. Core Engineering Philosophy

The recurring philosophy of the series is:

> **Observe. Think. Verify. Act.**

Every book should reinforce these habits:

1. Understand the situation before changing anything.
2. Ask the right diagnostic question.
3. Use the appropriate tool or command.
4. Verify the result.
5. Explain the reasoning clearly.
6. Make the smallest safe change when action is required.
7. Confirm that the change solved the problem.

---

## 4. Story-Driven Learning Model

The reader is an engineer, not a student sitting in a classroom.

Concepts are introduced through realistic work situations: production incidents, deployments, infrastructure changes, troubleshooting sessions, interviews, and operational decisions.

Each chapter should feel like one investigation rather than a collection of unrelated tutorials.

The recurring flow is:

**Mission Brief → Question → Command/Tool → AWS/DevOps Scenario → Interview Corner → Reflection → Mission Progress**

The detailed scene structure is governed by `STYLE_GUIDE.md`.

---

## 5. Recurring Characters

Keep the cast intentionally small.

- **Manager** — provides the assignment, constraints, or operational context.
- **Reader / Cloud Engineer** — performs the investigation and makes decisions.

Characters exist to create realistic engineering situations, not to become a separate storyline.

---

## 6. Teaching Principles

### Story before syntax

Introduce the problem before introducing the command.

### Reasoning before memorization

Explain what evidence the engineer needs and why the command provides it.

### Production before theory

Prefer situations that resemble real infrastructure work.

### Interview relevance

The reader should be able to explain not only what to do, but why.

### Cloud-enhanced, not cloud-dependent

AWS is the primary practical environment because it matches the target audience. Engineering principles remain cloud-neutral.

### Progressive difficulty

Each mission should build on previous habits and concepts.

---

## 7. Book Architecture

The series uses a common architecture so readers know how to learn from each book.

**Book → Chapters → Missions → Scenes → Practice → Interview Challenge**

A mission is a meaningful unit of work. A scene solves one part of that mission.

The reader should always know:

- Where they are.
- What problem they are solving.
- What they have learned.
- Why it matters in production.
- How it may appear in an interview.
- What comes next.

---

## 8. Series Sequence

The current planned sequence is:

1. **Linux for AWS & DevOps Interviews**
2. **Git for AWS & DevOps Interviews**
3. **Docker for AWS & DevOps Interviews**
4. **Terraform for AWS & DevOps Interviews**
5. **Kubernetes for AWS & DevOps Interviews**

Later books may extend the series, but new titles should not be added to the committed roadmap without an explicit project decision.

---

## 9. Cross-Book Continuity

Each book should prepare the reader for the next layer of engineering responsibility.

- Linux establishes system and troubleshooting fundamentals.
- Git establishes version control and collaborative engineering workflows.
- Docker establishes application packaging and container fundamentals.
- Terraform establishes infrastructure as code.
- Kubernetes establishes container orchestration and platform operations.

The books should connect naturally without requiring the reader to memorize content from a previous book.

---

## 10. Editorial Hierarchy

The project uses this hierarchy:

**SERIES_BIBLE → STYLE_GUIDE → BOOK_ROADMAP → PROJECT_STATUS → Manuscript**

- `SERIES_BIBLE.md` defines stable series identity and creative architecture.
- `STYLE_GUIDE.md` defines detailed writing and formatting rules.
- `BOOK_ROADMAP.md` defines the planned progression of the current book.
- `PROJECT_STATUS.md` records live execution status.
- `manuscript/` contains the actual book.

When documents conflict, higher-level guidance wins unless explicitly revised.

---

## 11. Definition of a Successful Book

A finished book should be:

- Practical enough to follow hands-on.
- Structured enough to study systematically.
- Realistic enough to reflect production work.
- Focused enough to remain useful for interviews.
- Clear enough for a beginner to follow.
- Deep enough for an experienced interviewer to respect the reasoning.

The goal is not maximum command coverage.

The goal is **maximum engineering understanding per page**.

---

## 12. Non-Negotiables

1. **Story before syntax.**
2. **Think before type.**
3. **One mission, one narrative.**
4. **Production first.**
5. **Interview ready.**
6. **Commands must be accurate and tested before publication.**
7. **Avoid unnecessary jargon and unexplained assumptions.**
8. **Keep AWS as an example, not a limitation.**
9. **Do not sacrifice clarity for technical complexity.**
10. **Protect continuity across the entire series.**

---

## 13. Quality Bar

Before a book is considered publishable, it should pass three tests.

### Engineer Test

Would a working engineer recognize the situation as realistic?

### Learner Test

Could a motivated beginner follow the reasoning and reproduce the work?

### Interview Test

Could the reader explain the decision-making process in an interview?

If a section fails all three tests, it does not belong in the book.