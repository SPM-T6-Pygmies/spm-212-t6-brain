Source of truth for writing and scrutinising backlog items. Derived from the Week 2 deck (Three Cs, INVEST, story card anatomy, splitting patterns, DEEP), tightened where the deck leaves room for interpretation. **Where this document and a lecture example disagree, this document wins** — one disagreement is called out explicitly in §5.6.

---

### 1. Backlog taxonomy

|Item|What it is|When it exists|Estimated?|
|---|---|---|---|
|**Epic**|Large story representing a broad objective or significant piece of functionality. A _placeholder_ on the backlog.|Until the work is near enough to be worth decomposing|No (or T-shirt only)|
|**User Story**|A thin vertical slice of user-visible value, INVEST-compliant|When it is a candidate for a sprint|Yes — Fibonacci|
|**Task**|Implementation step inside a story (front-end, tests, docs). Owned by developers, not the customer.|Sprint planning|No (or hours, optional)|
|**Spike**|Timeboxed investigation to remove an unknown. Last resort.|When a story cannot be estimated at all|Timebox, not points|

Rules:

- An epic is never "done". It disappears when replaced by its stories.
- Every story belongs to exactly one epic. Orphan stories are a smell that the epic set is wrong.
- A task is not a story. "Set up the database schema" has no role, no user value, and cannot be demoed. It is a task or it is nothing.
- A spike is failure-of-conversation insurance, not a planning habit. If more than ~1 in 10 items is a spike, the problem is customer access, not complexity.

---

### 2. The Three Cs — what a story actually is

A user story is **not** the sentence on the card. It is three things (Jeffries):

1. **Card** — the short written placeholder. Cheap. An LLM writes this in seconds.
2. **Conversation** — the discussion that fills in the detail. _This is the point._ It cannot be delegated to an agent, because the agent has never met the customer.
3. **Confirmation** — the acceptance criteria that record what "done" means.

**Consequence for how we work:** the card is nearly free, the conversation is the scarce input, and the confirmation is where our effort and our marks go. A card written without a conversation behind it is a guess wearing the right costume.

---

### 3. Story card anatomy

Every story card has exactly these fields. No more.

```
┌──────────────────────────────┬────────────┬───────────┐
│ TITLE                        │ PRIORITY   │ ESTIMATE  │
│ Short, feature-named         │ 1..N order │ Fibonacci │
├──────────────────────────────┴────────────┴───────────┤
│ USER STORY                                            │
│ As a <role>, I want to <action> so that <outcome>     │
├───────────────────────────────────────────────────────┤
│ ACCEPTANCE CRITERIA                                   │
│ The conditions that must ALL hold for this to be done │
└───────────────────────────────────────────────────────┘
```

**Title** — names the feature, not the mechanism. "Accept Credit Card Payments", not "Payment module" and not "Add Stripe SDK". 2–5 words. Unique across the backlog.

**Priority** — a strict 1..N order across the backlog, not a MoSCoW bucket. Two items cannot both be "High". Ordering criterion: value ÷ cost, with high-risk items pulled forward so risks are discovered early.

**Estimate** — §7.

---

### 4. The story statement

#### 4.1 Format

```
As a <role>, I want to <action> so that <goal / outcome>
       WHO              WHAT                  WHY
```

The **structure is rigid; the phrasing is flexible.** `I want to` / `I can` / `I need to` are all acceptable. Do not let phrasing debates consume the conversation.

#### 4.2 WHO — the role

- An **abstract role**, never a named person and never a job title borrowed from the org chart. "As a subscriber", not "As Jerrick" and not "As the Head of Billing".
- The role must be one that **derives value** from the outcome. If the beneficiary and the actor differ, the card is probably two stories or the wrong role.
- "As a user" is almost always a defect. It means the team has not done the role enumeration (questionnaire / observation / interview / story-writing workshop). Exception: a genuinely universal capability where every role behaves identically.
- Multi-role features: write one story per role. Different roles have different acceptance criteria — that is the tell that they are different stories.

#### 4.3 WHAT — the action

Format: **{active verb} {object}**, with object qualifiers where they narrow scope.

- ✅ `{see list} of {credit card transactions}`
- ✅ `{get alert} for each {credit card transaction of $100 or above}`
- ❌ `manage my account` — "manage" implies CRUD; apply the Operations splitting pattern (§6.2)
- ❌ `have a better dashboard` — not an action
- ❌ `use the REST endpoint to fetch orders` — solution, not need

The action names **what the user does**, not how the system implements it. Verbs that leak implementation (cache, index, refactor, integrate, deploy) do not belong in the WHAT.

#### 4.4 WHY — the outcome

The reason the feature exists, from the role's viewpoint. It must be **falsifiable as a benefit** — if the story shipped and the outcome did not improve, someone should be able to say so.

- ✅ `so that I can pay punctually and not be charged a penalty`
- ❌ `so that I can have a better experience` — unfalsifiable
- ❌ `so that the system stores my details` — restates the WHAT

The WHY is also the ambiguity detector. "so that I can complete my purchase **conveniently**" — convenient how? Saved cards? No OTP? More card providers? Every vague WHY is a question for the customer, and the answers become acceptance criteria.

**Do not resolve a vague WHY by inventing the answer.** Flag it, ask the customer, then write it down.

---

### 5. Acceptance criteria — the core of the standard

#### 5.1 What ACs are for

ACs serve four masters simultaneously. A criterion that serves none of them is deletable.

1. **Definition of Done** — the story is complete when and only when every AC holds.
2. **Boundaries** — what is in scope and, by omission, what is out.
3. **Test basis** — each AC maps to at least one test case (Wk 4 → test class Wk 6).
4. **Spec for the agent** — when an agent writes the code, the ACs are the contract it builds against and the checklist you review its output with. No AC ⇒ no way to tell whether the agent built the right thing.

#### 5.2 The five sharpness tests

Every criterion must pass all five. Failing one is a defect to fix, not a matter of taste.

**T1 — Binary.** A reviewer can answer yes/no with no judgement call. "The screen must not take too long to appear" fails. "The screen appears within 2 seconds" passes.

**T2 — Observable at the boundary.** Phrased in terms of what the user or an external caller can see. A criterion that can only be verified by reading the source, inspecting a table, or attaching a debugger is a task, not an AC.

**T3 — Non-obvious.** It must eliminate a _real_ ambiguity — something two competent people could otherwise implement differently, or a rule only the customer knows. Criteria that restate the story sentence, or that state universal software hygiene, are noise. They pad the card, dilute review attention, and hide the criteria that matter. Delete:

- "The page loads without errors"
- "The button is clickable"
- "Data is saved to the database"
- "The user can log in" (unless this story _is_ login)
- "Follows the design system" — that belongs in the Definition of Done, once, globally

**T4 — Outcome, not mechanism.** It states the required behaviour, not the implementation. `Only Visa, Mastercard and Amex are accepted` passes. `Uses the Stripe card-network allowlist` fails — it removes the team's room to negotiate (INVEST-N) and, when written to please an agent, hands it a design decision the team should be making.

**T5 — Independently checkable.** No AC depends on another AC's interpretation. Each stands alone as a test.

#### 5.3 What ACs must cover

For each story, the set must cover:

- **The happy path** — the main flow completing successfully.
- **The rules the customer actually stated** — eligibility, limits, allowed values, ordering, permissions. These are the highest-value criteria on the card because they are the ones an agent cannot guess.
- **The error and edge paths that change behaviour** — invalid input, empty state, unauthorised access, boundary values. Only where behaviour genuinely differs; do not enumerate every possible failure.
- **Explicit exclusions where scope is genuinely contested** — one line, e.g. `Out of scope: refunds`. Use sparingly; a card full of exclusions is a card that should have been split.

Typical count: **3–7 criteria.** Fewer than 3 usually means the conversation has not happened. More than 7 usually means the story is too big — go to §6.

#### 5.4 Format: checklist vs scenario

Both are legitimate. Mix them within a card as needed.

**Checklist** — concise, faster to write, more agile. Default choice. Leaves detail open to team negotiation.

> Ensure that the customer:
> 
> - Can only pay with Visa, Mastercard or American Express
> - Is requested to provide an OTP
> - Has their payment processed and their order confirmed

**Scenario (Given-When-Then)** — a condition (_Given_) under which a trigger occurs (_When_) producing an outcome (_Then_). Use when behaviour varies by condition, when a journey has multiple branches, or when the story is complex enough that a flat list would be ambiguous.

> **Given** a customer has entered incomplete or invalid card details  
> **When** they confirm payment  
> **Then** a message advises them to check their card details, and payment is not processed

Selection rule: **if the criterion has an "if", use a scenario; if it does not, use a checklist line.** Do not inflate a simple checklist item into a three-line scenario — that is ceremony, not clarity.

**EARS** (optional, for agent-facing specs): constrained natural language using a fixed clause order and the keywords **WHILE, WHEN, IF, THEN, WHERE, SHALL**.

> **WHEN** the customer confirms payment **THE SYSTEM SHALL** report success or failure within 2 seconds

Use EARS when the card is being handed to an agent as a spec and precision matters more than readability. Do not mandate it across the backlog.

#### 5.5 Non-functional requirements in ACs

**Default: leave them out.** NFRs belong in the Definition of Done or an architecture note, where they are stated once and apply everywhere. Repeating "must use TLS" on forty cards is forty places to maintain one decision, and it drowns the criteria that are actually specific to the story.

Include an NFR on the card **only** when both hold:

1. The threshold is **specific to this story** and differs from the global standard, **and**
2. The customer stated the number, or the team agreed it in a conversation.

**Never invent a number.** "Loads within 2 seconds" that no one asked for is _false precision_ — it looks rigorous, becomes a test, becomes a constraint, and traces back to nobody. This is the single most common way an AI-drafted card corrupts a backlog.

If performance is the actual point of the story, that is a Defer-Performance split (§6.2), not an extra bullet.

#### 5.6 Deliberate deviation from the lecture example

The deck's credit-card card (slide 26) carries `Has a secure TLS connection` and `Is informed of payment success or failure within 2 seconds`. Under this standard:

- **TLS** → moves to the Definition of Done. It is a global engineering standard, not a boundary of this story, and it fails T2 (not observable to the customer at the boundary).
- **2 seconds** → keep **only** if the customer stated it. Otherwise it is invented precision under §5.5.

The deck example is a teaching illustration of the _card format_. This standard governs the _content_.

#### 5.7 AC anti-pattern catalogue

|Anti-pattern|Example|Fix|
|---|---|---|
|Restated story|"User can pay by credit card" on the credit-card story|Delete|
|Obvious hygiene|"No console errors", "Page loads"|Delete → Definition of Done|
|Unmeasurable adjective|"fast", "intuitive", "user-friendly", "seamless", "robust"|Replace with a number or an observable behaviour, or delete|
|Implementation leak|"Stores the token in Redis"|Restate as behaviour, or drop to a task|
|Invented number|"within 2 seconds" nobody asked for|Ask the customer; until then, delete|
|Hidden second story|"…and an admin can refund it"|Split (§6)|
|Test script|"Click Pay, then click Confirm, then observe…"|Compress to the outcome; steps belong in the test case|
|Over-specification|ACs so complete that no design freedom remains|Cut back to outcomes — this violates INVEST-Negotiable|
|Blanket NFR|"Must be secure", "Must be accessible"|Definition of Done, or a specific measurable criterion|

---

### 6. INVEST

#### 6.1 The six attributes

**I — Independent.** Estimable and schedulable without regard to other stories. Interdependent stories make planning and estimation harder because the order becomes forced.  
_Test:_ could this be pulled into a sprint on its own and still deliver value?  
_Failure example:_ "receive alerts for restaurant offers I have selected" cannot exist before "select which restaurants to follow". Fix by re-slicing so the first story carries a thin, self-sufficient version.  
_Tolerance:_ perfect independence is impossible. Declare the dependency on the card, order the backlog to respect it, and do not pretend it is absent.

**N — Negotiable.** The card describes the desired **outcome**, not a fixed solution. It is a placeholder for conversation, and details may be refined — even mid-sprint with the Product Owner, provided the **Sprint Goal is not endangered**.  
_Test:_ do the ACs over-specify how the solution must be implemented?  
_AI-era warning:_ the temptation to over-specify so an agent produces exactly one output destroys negotiability. Specify the _what_ precisely; leave the _how_ open.

**V — Valuable.** Value is from the viewpoint of the **role**, in a recognisable form: efficiency, better experience, cost saving, revenue, or a stated business requirement.  
_Test:_ if this shipped alone, who is better off and how?  
_Failure:_ stories written for the developers ("refactor the auth layer"). Real work, wrong artefact — that is a task.

**E — Estimable.** Clear enough that developers can reasonably size the effort. Three causes of unestimable stories, with three different fixes:

- _Missing domain knowledge_ → have the conversation; elaborate to the level needed to estimate.
- _Missing technical knowledge_ → timeboxed investigation (spike). Strictly timeboxed.
- _Too big_ → split (§6.2).

**S — Small.** Goldilocks. Must fit comfortably inside a sprint.

- _Too big_ → cannot fit; split. "See all information the hospital has of me" → bills / medical records / …
- _Too small_ → overhead exceeds value, or no standalone functional value. "See all charges to two decimal places" is a criterion, not a story. Club small ones up to modular size.
- _Rule of thumb:_ target 1/10 to 1/6 of team velocity per story. **We have no velocity baseline yet (§7)**, so until Sprint 2 use the proxy: _if the whole team cannot plausibly finish it in about half a sprint, split it._

**T — Testable.** "If the story cannot be tested, how can the developers know when they have finished coding?" (Cohn). There must be a clear set of ACs and a means to test them. A story is untestable exactly when its ACs fail §5.2.

#### 6.2 INVEST when an agent writes the code

The six attributes hold, but their weights shift:

- **Testable ↑↑** — the AC is now the contract the agent builds against _and_ the checklist you review with.
- **Valuable ↑↑** — anyone can generate a story; deciding what **not** to build is the scarce skill.
- **Independent ↑** — agents touch many files at once; independent stories keep diffs reviewable.
- **Small ↻** — re-anchored: small enough to **review**, not small enough to type. Generation speed is no longer the constraint; review capacity is.
- **Estimable ❓** — if writing code got faster but reviewing it did not, what are we estimating? See §7.
- **Negotiable ⚠** — over-specifying the AC to please the agent removes the team's room to negotiate. This is the trap.

---

### 7. Splitting

#### 7.1 Procedure

1. **Prepare the input story.** Does it satisfy INVEST apart from Small? If not, fix or reformulate first — splitting a bad story yields several bad stories.
2. **Apply a splitting pattern** (§7.2).
3. **Evaluate the split** (§7.3).

#### 7.2 Patterns (Humanizing Work)

|Pattern|Trigger question|Split by|
|---|---|---|
|**Workflow steps**|Does the story describe a workflow?|Thin slice through the whole workflow first; enhance later. Or beginning + end first, middle later.|
|**Operations**|Does it include multiple operations — "manage", "configure", CRUD?|One story per operation|
|**Business rule variations**|Does it have a variety of business rules? Terms like "flexible dates"?|Core rule set first; additional rules later|
|**Variations in data**|Does it do the same thing to different kinds of data?|One data kind first|
|**Interface variations**|Same data via multiple interfaces? Complex UI?|One interface first; simple version before rich version|
|**Major effort**|With the obvious split, is one part the hardest?|Do the hard one first, defer the decision on the rest|
|**Simple/complex**|Is there a simple core carrying most of the value?|Simple core first, enhancements later|
|**Defer performance**|Does complexity come from an NFR like performance?|Make it work first; make it fast second|
|**Break out a spike**|Still baffled?|Last resort. Name the 1–3 questions blocking you; timebox|

The word **"and"** in a story sentence is the loudest split signal there is. "Show the cinema's address **and** movie schedule" is two cards.

#### 7.3 Evaluating a split

- Are the new stories roughly equal in size?
- Does each satisfy INVEST?
- **Are there stories you can now deprioritise or delete?** If yes, the split succeeded — you just found waste. If none of the pieces can be dropped, try another pattern.
- Is there an obvious first story that delivers early value, learning, or risk mitigation?

---

### 8. Estimation

Scale: **1, 2, 3, 5, 8, 13, 21.**

- Points are **relative size** — complexity, effort, uncertainty combined — not hours and not a commitment.
- **No baseline velocity yet.** Until Sprint 2 gives us actuals: pick one small, well-understood, universally-agreed story, call it **3**, and size everything against it. Do not calibrate against calendar time. Re-anchor after the first sprint.
- Every backlog item **for the current release** carries an estimate, and estimates are **reconsidered each sprint** as the team learns.
- **13 is a warning. 21 is a rejection.** A 21 means "we do not understand this yet" — split it or spike it before it enters a sprint.
- Wide disagreement (someone says 2, someone says 13) is not a number problem. It is an information problem: someone knows something the others do not. Surface it before re-voting.
- **Estimate the reviewable increment, not the typing.** If an agent writes the code, the effort that remains is specification, review, integration and correction. Size that.

---

### 9. Definition of Ready and Definition of Done

**Definition of Ready** — a story may enter sprint planning only when:

- [ ]  Role, action and outcome are all present and specific
- [ ]  Belongs to an epic; priority assigned
- [ ]  Acceptance criteria written and passing §5.2
- [ ]  No open customer question that would change the ACs
- [ ]  INVEST-compliant, small enough for one sprint
- [ ]  Estimated at 13 or below
- [ ]  Dependencies named

**Definition of Done** — global, applies to every story, stated here once so it never appears on a card:

- [ ]  All acceptance criteria demonstrably met
- [ ]  Tests written and passing, traceable to the ACs
- [ ]  Code reviewed by a human — including, especially, agent-written code
- [ ]  Merged, integrated, no known regressions
- [ ]  Secure transport, error handling, and the team's coding standards observed
- [ ]  Demonstrable to the customer

---

### 10. Traceability

```
user story → acceptance criteria → test case (Wk 4) → test class (Wk 6) → code
```

This chain is the team's evidence of **what** was built and **why** — and it is the deliverable when a feature is picked at random in Week 13. When an agent wrote the code, the chain is the only accountability artefact that exists.

> **"The agent decided" is not an answer.** The team is accountable for what it ships.

---

### 11. Using AI on the backlog

**Good — AI as critic:**

- Audit a story against INVEST: _"which of these ACs is not testable, and why?"_
- Hunt ambiguity: _"list every term here that two people could read differently"_
- Generate edge cases and error scenarios the team missed
- Propose splits using the patterns in §7.2

**Poor — AI as author:**

- A bulk-generated backlog nobody has read. 200 plausible stories is not a backlog.
- Scope the customer never asked for
- False precision: invented thresholds, invented rules, invented roles

**Workflow:** `AI drafts → team decides → customer confirms.` The middle step is not optional, and the third is what makes the card real.

---

### 12. Backlog health — DEEP

- **Detailed appropriately** — the top of the backlog is fine-grained and fully specified; the bottom is epics and one-liners. Detailing the bottom is waste, because it will change.
- **Estimated** — current-release items have estimates; re-estimate each sprint.
- **Emergent** — items are added, removed, modified, split and re-prioritised continuously. The backlog is a living artefact reflecting **current understanding**, not a contract signed in Week 4.
- **Prioritised** — strict 1..N order; most bang for the buck at the top, high risks pulled forward.

Changes are welcome at any time in the _product_ backlog. Inside a sprint, changes must not **jeopardise the Sprint Goal**.

---

### 13. Scrutiny rubric

Apply in order. **Any FAIL in Gate A or B blocks the story from Ready.**

#### Gate A — Structure (fast reject)

|#|Check|Fail signal|
|---|---|---|
|A1|Title names a feature, ≤5 words, unique|Module names, tech names, duplicates|
|A2|Sentence has all three of role / action / outcome|A missing clause|
|A3|Role is abstract and is the beneficiary|"As a user", a person's name, a job title|
|A4|Action is {active verb}{object}, no implementation|"manage", "handle", "support", API/DB nouns|
|A5|Outcome is a falsifiable benefit, not a restatement|"better experience", repeats the action|
|A6|Sentence contains no "and" joining two capabilities|Compound story|
|A7|Assigned to an epic; has priority and estimate|Orphan, tied priority, missing estimate|

#### Gate B — Acceptance criteria

|#|Check|Fail signal|
|---|---|---|
|B1|3–7 criteria present|0–2 (no conversation) · 8+ (too big)|
|B2|Every criterion is binary (T1)|"fast", "easy", "appropriate", "as needed"|
|B3|Every criterion is observable at the boundary (T2)|Requires reading code or the database|
|B4|No obvious or restated criteria (T3)|Hygiene items, story restatements|
|B5|No implementation mechanism (T4)|Named libraries, schemas, endpoints|
|B6|No invented numbers or rules|A threshold with no customer source|
|B7|NFRs absent unless story-specific **and** customer-stated (§5.5)|Blanket TLS / performance / accessibility lines|
|B8|Happy path + stated business rules + behaviour-changing edges all covered|Only the happy path|
|B9|Format fits: scenarios where conditional, checklist otherwise|GWT ceremony on flat criteria, or flat lines hiding branches|
|B10|Each criterion maps to at least one testable case|Cannot state the test|

#### Gate C — INVEST

|#|Check|Fail signal|
|---|---|---|
|C1|**I** — schedulable alone|Requires an unbuilt sibling to have any value|
|C2|**N** — outcome specified, solution open|ACs dictate the design|
|C3|**V** — value accrues to the stated role|Value is to the dev team|
|C4|**E** — team can size it|No estimate possible → domain gap / tech gap / too big|
|C5|**S** — fits well inside one sprint|Estimated 13 (warn) or 21 (reject)|
|C6|**T** — done is unambiguous|Any Gate B failure|

#### Gate D — Context fit

|#|Check|
|---|---|
|D1|Every rule and number traces to something the customer said, or is flagged as an open question|
|D2|No scope the customer never asked for|
|D3|Terminology matches the customer's own words|
|D4|Consistent with sibling stories in the same epic — no contradiction, no overlap|
|D5|Open questions listed on the card rather than silently resolved|

#### Verdict format

```
VERDICT: READY | NEEDS WORK | REJECT
Gate A: pass/fail (+ failing checks)
Gate B: pass/fail (+ failing checks)
Gate C: pass/fail (+ failing checks)
Gate D: pass/fail (+ failing checks)

DEFECTS   — check id, quote the offending text, state why it fails, give the rewrite
QUESTIONS — what must be asked of the customer before this is Ready
SPLIT     — if S or B1 failed: the pattern to apply and the resulting story titles
```

**Rules for the reviewing agent:** quote the exact text you object to. Never resolve an ambiguity by inventing the answer — raise it as a question. Never add a criterion the customer did not ask for. Deleting a weak criterion counts as an improvement.

---

### 14. Worked example

**Before**

> **Title:** User Management  
> **Story:** As a user, I want to manage my account and see my activity so that I can have a better experience.  
> **AC:**
> 
> - The page loads successfully
> - User can update details
> - Data is saved securely to the database
> - Response time should be fast
> - UI follows best practices

Defects: A1 (module name), A3 ("as a user"), A4 ("manage" = CRUD), A5 (unfalsifiable), A6 ("and"), B2 ("fast", "successfully"), B3 (database), B4 (page loads, saved), B5 (database), B7 (blanket security + performance), B8 (no rules, no edges), C2, C4, C5, C6.  
Verdict: **REJECT** — split via **Operations** and **Workflow steps**.

**After (one of the resulting cards)**

> **Title:** Update Contact Details · **Priority:** 4 · **Estimate:** 3
> 
> **Story:** As a registered member, I want to update my email address and phone number so that appointment reminders reach me after I move or change provider.
> 
> **AC:**
> 
> - Member can change email address and phone number from their profile
> - A changed email address takes effect only after the member confirms it via a link sent to the new address
> - Reminders continue to go to the previous email until confirmation completes
> - **Given** the member enters an email address already registered to another account, **when** they save, **then** the change is rejected and the existing address is retained
> - Phone number accepts Singapore mobile numbers only _(customer confirmed 28 Aug — overseas numbers deferred)_
> 
> **Open question:** does changing the phone number require re-verification too? — for the Week 4 customer Q&A

Note what is _absent_: no "page loads", no "saved to database", no "secure", no invented latency. Every remaining line carries information an implementer could otherwise get wrong — and the one number-like constraint is attributed.

---

**Change control:** this document is versioned. Amendments require a team decision, recorded with a date and a reason. Customer clarifications that change a rule update the affected cards, not this standard.