# Prompt Cards — AI-Assisted Development (ChatYa)

The sections below document the actual prompts used to direct the AI coding
assistant (Claude) through each stage of building and refining this project.
Each card shows the objective, the prompt engineering techniques it applied,
a representative excerpt of the prompt itself, and what it produced. Full
prompt text and the assistant's full responses are preserved in the
project's chat export, available on request.

---

## Card 1 — Initial Concept Prompt

**Stage:** Project kickoff
**Objective:** Establish the base concept — a Reddit-style community platform.
**Techniques used:** Direct task framing, feature enumeration.

> "there's a project I did in my previous semester of creating some sort of
> reddit app replica, can you do it here? the perfect clone of reddit, which
> is supposed to run in html, develop it, the backend and frontend, don't
> exclude anything, it's supposed to allow to create communities like reddit,
> post things, and all, and comment..."

**Outcome:** A single-file HTML/JS prototype (communities, posts, comments,
voting) as a fast proof of concept.

---

## Card 2 — Full Technical Specification Prompt

**Stage:** Pivot to a real backend project
**Objective:** Replace the prototype with a production-style Python/Flask
academic project with a defined architecture, phased build order, and an
NLP + LLM comparison feature.
**Techniques used:** *Role prompting* ("You are a senior full-stack Python
developer, NLP/LLM engineer..."), *task decomposition* (18 explicit
phases), *structured output constraints* (exact JSON schemas for every AI
response), *few-shot examples* (sample sentiment classifications for
sarcasm/slang), *explicit constraints* ("Do not invent information," "Do
not fabricate numerical accuracy"), *file-by-file output format*
requirements.

> "Build a web application that allows a user to: 1. Enter a subreddit
> name... [18-phase build order] ... For every implementation step: Give the
> filename. Give the COMPLETE code for that file. ... Do not leave
> placeholders like 'add your code here.'"

**Outcome:** Flask app factory, SQLAlchemy models, Reddit-data ingestion via
PRAW with Demo Mode fallback, VADER sentiment baseline, and a working
analytics dashboard — verified end-to-end with an automated smoke test
before being handed back.

---

## Card 3 — Branding Correction Prompt

**Stage:** Product identity refinement
**Objective:** Replace all "Reddit" branding with an original product
identity ("ChatYa") without breaking functionality.
**Techniques used:** *Explicit negative constraints* (a list of forbidden
names/terms), *terminology substitution rules* (`subreddit` → `community`,
`r/` → `c/`), *scope enumeration* (every surface branding must/must not
appear on), *visual identity direction* (palette, tone).

> "The name 'Reddit' MUST NOT appear as the application/product name
> anywhere... Use: c/technology... Do NOT copy Reddit's logo, Snoo mascot,
> exact branding, or proprietary visual assets."

**Outcome:** Full rebrand across templates, flash messages, and error
strings — traced and verified with a grep-based audit for leftover "Reddit"
references before re-testing.

---

## Card 4 — Full Platform Requirements Prompt ("STOP" Prompt)

**Stage:** Major architecture pivot
**Objective:** Correct course from an analytics dashboard to an actual
functional social platform with real accounts and native content.
**Techniques used:** *Strong corrective framing* ("STOP. The current
application is NOT what I need"), *exhaustive feature enumeration*
(29 numbered user capabilities), *explicit data model specification*
(field-by-field schema for 8 tables), *explicit route table* (every URL
path required), *security constraint list* (hashing, CSRF, authorization
checks), *acceptance-test specification* (a 17-step manual test script the
assistant must pass before declaring completion), *anti-placeholder
constraint* ("Do not create fake buttons that do nothing").

> "I need you to fundamentally refactor the application into a FUNCTIONAL
> COMMUNITY SOCIAL MEDIA PLATFORM... Before declaring the project complete,
> test the complete user journey: 1. Register account... 17. Verify
> application still works without Gemini credentials. Fix all errors
> encountered."

**Outcome:** Complete rewrite — Flask-Login auth, 8 SQLAlchemy models,
CRUD for posts/comments/communities, vote-toggle logic, notifications, and
a working ChatYa AI layer with graceful degradation. This prompt's
acceptance-test section directly shaped the verification approach in Card 5.

---

## Card 5 — Verification Prompt

**Stage:** Quality assurance
**Objective:** Confirm the rebuilt platform actually works rather than
accepting a plausible-looking but untested result.
**Techniques used:** *Minimal instruction relying on established context*
("Continue") — deliberately low-specification, testing whether the
assistant would self-direct correctly using Card 4's acceptance criteria
already in context.

> "Continue"

**Outcome:** A scripted end-to-end test (register → login → create
community → join → post → vote → comment → reply → save → search →
profile → logout/login → AI endpoints) plus a second pass specifically
testing authorization boundaries (403 on editing another user's content)
and duplicate-prevention constraints (vote toggling, unique memberships).
Two real bugs were found and fixed this way: a Jinja macro losing template
context, and a stale endpoint reference from an earlier rename.

---

## Card 6 — Documentation & Compliance Prompt

**Stage:** Academic submission requirement
**Objective:** Produce this Prompt Cards documentation to satisfy the
assignment's requirement to show prompt engineering was used to refine the
project.
**Techniques used:** *Constraint-based instruction* (repo → README,
no-repo → Google Doc), *output location specification*, *explicit success
criterion* ("clearly show how Prompt Engineering was used").

> "now give me the prompt cards i might have used to improve the previous
> reddit project... add all the prompts/prompt cards used for project
> enhancement in the readme file... Make sure the prompts clearly show how
> Prompt Engineering was used to improve or enhance your existing project."

**Outcome:** This document.
