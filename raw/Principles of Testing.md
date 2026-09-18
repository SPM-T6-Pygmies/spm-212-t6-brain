## PART 1 — TESTING & TEST CASE DESIGN

### A. Principles of testing _process_ (how testing sits in the SDLC)

1. **No silos.** Developers throwing "finished" code over the wall to QA, who throw defects back, is the anti-pattern. Long feedback loops, no shared understanding.
2. **Testing is a core activity of every iteration**, not a downstream phase. Waterfall = test as a distinct late stage → long feedback cycles. Agile = testing integrated throughout → fast feedback on small changes.
3. **Cross-functional teams.** Testers and developers (possibly the same people) work together: break silos, build shared understanding, frequent regular feedback (e.g. sprint reviews).
4. **Frequent integration into a shared repo.** Smaller, more frequent changes → earlier diagnosis and easier fault localisation. AI accelerates code generation, but _keep integrations small and reviewable_.
5. **Automation enables CI and safe refactoring.**

### B. The Agile Testing Quadrants (the "different forms" of testing)

Two axes: **business-facing ↔ technology-facing**, and **supporting the team ↔ evaluating the product**.

||Business-facing|Technology-facing|
|---|---|---|
|**Supporting the team**|**Q2** — Functional test cases, prototypes, simulations _(automated & manual)_|**Q1** — Unit tests, integration tests _(automated)_|
|**Evaluating the product**|**Q3** — Exploratory, usability, user acceptance testing _(manual)_|**Q4** — Performance, load, stress, "ility" testing, API/contract testing _(tools)_|

**Which tests matter:**

- Minimum bar: **functional test cases for every user story**.
- **Automated unit/integration tests** — safe integration, regression protection.
- Beyond that, **what the customer wants** — performance/scalability/accessibility requirements; and how costly an outage actually is.
- **Testing depth follows business risk.** (Netflix / Chaos Monkey: stress in production because an outage there costs real money.)

### C. What a test case _is_

- A **specific, detailed** description of the steps and conditions required to determine whether a function works **as intended**.
- Exists to **confirm the acceptance criteria have been met**.
- Unlike ACs, a functional test case considers **specific outputs given specific inputs**.
- Must be **clear and unambiguous** enough for anyone to follow: ✗ "log in to the app" / ✓ "log in with username X and password Y".
- Describes **inputs, conditions, expected behaviour**; may be run manually _or_ encoded as an automated test.
- Value: shared understanding of expected behaviour · facilitates PO/user story acceptance · **constrains AI agents during code generation**.

### D. Test case anatomy — specification vs execution record

**Specification (written once; changes only when the requirement changes):**  
Test Case ID · Test Scenario · Pre-conditions · Test Steps · Test Data · Expected Result · _(Created By, Date of Creation)_

**Execution record (one per run):**  
Actual Result · Pass/Fail/Not Executed/Blocked · Remarks · _(Executed By, Date of Execution)_ · build/environment, evidence or defect link

Principles:

- **One specification, many executions.** The same case is re-run across browsers, builds, environments, sprints to detect regressions.
- **"A passing case is only ever passing as of that build."**
- The template is just one representation. **The transferable skill is: specific inputs + preconditions + action + expected result.**

### E. Deriving test cases from ACs

Scenario-based ACs map almost 1:1 onto test cases:

|Acceptance Criterion|Test Case|
|---|---|
|Given…|Pre-conditions…|
|When…|Test Steps / Test Data…|
|Then…|Expected Result…|

A checklist-based AC can be refined into **multiple** scenario-based ACs → therefore **multiple** functional test cases.

### F. The 5-step approach to identifying test cases

1. **Visualise workflow** — put yourself in the end user's shoes; visualise what they see and how they might behave. If unfamiliar with the feature, converse with the team or navigate existing features to get a feel.
2. **Create the "happy path"** — the path through the feature with no errors or exceptions (the **positive test case**). Usually executed first when validating a feature.
3. **Cross-cutting quality expectations** — expectations holding across many features: auth & authorisation, security, accessibility, responsive UI, browser/device compatibility, consistent error handling, common navigation/design-system behaviour. **Don't duplicate generic checks per story — set the bar once in the DoD;** but _do_ write a dedicated test when the cross-cutting concern produces story-specific behaviour.
4. **Negative testing** — the happy path is not the only path. Erroneous user data, business exceptions, unavailable systems. Verify these "sad" events are handled correctly, e.g. via appropriate error messages.
5. **Boundary testing** — where the scenario involves ranges of inputs, find the **boundary values at which expected behaviour changes**. Test **"just below, exactly at, and just above"** (e.g. $19.99 / $20.00 / $20.01 for a $20 minimum). This is **boundary-value analysis**; its partner is **equivalence partitioning** — group inputs that should behave alike, test one from each group, then test the boundaries between them.

### G. Test cases in Scrum

**When to write them:**

1. ACs are created as the user story is written — test cases follow from them.
2. Additional cases emerge as conversations happen.
3. Sprint planning may add further ACs.
4. The team may discover additional cases before or after coding.

> TLDR: agree the **important** behavioural examples **before** implementation begins; more will emerge during development and review.

**The three-layer chain:**

- **Acceptance Criteria** → what must this story _do_?
- **Test Cases** → what concrete **evidence** shows that behaviour? _(Traceability: can you tell which requirement each test case is evidence for?)_
- **Definition of Done** → what broader quality and cross-cutting concerns must hold before the Increment is Done?

**Definition of Done** — a shared quality standard for the Increment, created by the Scrum Team if the organisation doesn't prescribe one. Typically a checklist: ACs satisfied · automated and manual tests pass · code reviewed · security/accessibility/performance/UI criteria satisfied where applicable · documentation and architecture views updated · deployable and integrated. **An item that does not meet the DoD returns to the Product Backlog.**

**Regression testing** — re-running test cases from previous sprints to verify existing code still functions. Matters because frequent changes cause unintended side effects. Integrate via the DoD checklist and/or automate in CI.

**Storage** — team wiki/spreadsheet, Jira plug-ins (Xray, Zephyr Squad — paid), or version-controlled next to the code and reviewed in PRs. **A test repository nobody maintains is worse than none, because it creates false confidence.**

### H. AI and test design

- **Generate, then curate.** AI drafts dozens of cases from a story + ACs in seconds.
- **Strong on the obvious:** happy path (step 2) and cross-cutting (step 3).
- **Weak where domain context or business rules are implicit:** domain boundaries (step 5) and business-rule negatives (step 4).
- The 5-step process therefore becomes a **review skill** — your job shifts from _writing_ to **interrogating**: do these tests actually cover the story?
- **The confidence trap:** AI-generated cases read confident and complete when neither. A passing test only matters if it checks the right behaviour. Beware **coverage theatre** — many cases, all exercising the same happy path. When AI writes both the code and its tests, both can share the same blind spot. **You are responsible for the judgement AI can't make: what behaviour is required, which risks matter, whether each expected result is actually correct.**
- **Test cases as executable spec:** flip the order — (1) agree the test cases first, (2) generate code to pass them. The cases become a precise, checkable specification the agent must satisfy. This is **spec-driven development**: well-formed ACs → concrete test cases → tighter brief, fewer AI misunderstandings.