A change in Scrum is any new, revised, or corrected requirement that affects the Product Backlog, or

planned work. Changes need to be managed so that the Scrum Team can assess their impact, maintain

transparency and quality, and reprioritize work with the Product Owner, adapting to the evolving

stakeholder and user needs.

**1. Change Request / Change Description**

• What is the requested change?

• Why is the change needed?

• Is it a new requirement, a clarification, or a correction of an existing requirement?

**2. Product Backlog**

• Add a new User Story/PBI if it is a new requirement

• Reprioritize incomplete User Stories

• Update existing backlog items if the requirement has been clarified

**3. Acceptance Criteria**

• Define or update the Acceptance Criteria for the changed/new requirement

• Check whether existing Acceptance Criteria are still valid

• Verify that all original Acceptance Criteria remain satisfied

**4. Analyze Changes - Story Points**

• Estimate the new User Stories

• Re-estimate an existing item only if new information materially changes the team's

understanding of its size, complexity, effort, or uncertainty

• For an incomplete item, the team can estimate the **remaining work**

**5. Software Design Documentation**

• C4 Architecture Models

• Class Diagram

• Sequence Diagram

• Database/Data Model

• API/interface specifications, if applicable

**6. Test Cases**

**![Attachment.tiff](file:///Attachment.tiff)**SMU Classification: Restricted

School of Computing and Information Systems IS212 Software Project Management

Page 2 of 3

SMU Classification: Restricted

• Unit Test Cases

• Functional Test Cases

• Integration Test Cases

• End-to-End (E2E) Test Cases

• Regression Test Cases

**Changes to User Stories**

These are some ways you can handle your user stories.

**Scenario 1** — Original Acceptance Criterion was missed

US-01: As a user, I want to make a payment online so that I can have the convenience of having orders

delivered. [Story Points: 5]

Acceptance Criteria:

• AC1: Pay by Visa

• AC2: Pay by Mastercard

• AC3: Display payment confirmation

US-01 was marked Done in Sprint 1 after the team believed that AC1, AC2, and AC3 were satisfied.

In Sprint 2, the team discovered that AC3 was actually not implemented.

**Solution**:

Reopen US-01 and return it to an incomplete state in Sprint 3. If only AC3 remains incomplete, the

remaining work for AC3 can be estimated at 2 Story Points, for example. The team completes AC3 and

verifies that all Acceptance Criteria and the Definition of Done are satisfied before marking US-01 Done

again.

Note: AC3 was part of the original User Story. The team simply failed to implement it. Therefore, this is

not a new requirement.

**Scenario 2** — New requirement

US-01: As a user, I want to make a payment online so that I can have the convenience of having orders

delivered. [Story Points: 5]

Acceptance Criteria:

• AC1: Pay by Visa

• AC2: Pay by Mastercard

• AC3: Display payment confirmation

US-01 was marked Done in Sprint 1 after the team believed that AC1, AC2, and AC3 were satisfied.

In Sprint 2, the team discovered that PayNow is now required as an additional form of online payment.

PayNow was not part of the original requirements or Acceptance Criteria of US-01.

**Solution**:SMU Classification: Restricted

School of Computing and Information Systems IS212 Software Project Management

Page 3 of 3

SMU Classification: Restricted

Keep US-01 as Done and create a new User Story:

US-25: As a user, I want to make a payment online via PayNow so that I can have the convenience of

having orders delivered.

Acceptance Criteria:

• AC1: Pay by PayNow

• AC2: Display payment confirmation for PayNow

Story Points: Estimate story points for US-25

**Scenario 3** — New requirement + original Acceptance Criterion is incorrect

US-01: As a user, I want to make a payment online so that I can have the convenience of having orders

delivered. [Story Points: 5]

Acceptance Criteria:

• AC1: Pay by Visa

• AC2: Pay by Mastercard

• AC3: Display payment confirmation

US-01 was marked Done in Sprint 1 after the team believed that AC1, AC2, and AC3 were satisfied.

In Sprint 2, the team discovered two issues:

1. PayNow is now required as an additional form of online payment. PayNow was not part of the

original requirements of US-01.

2. The implementation of AC3 does not actually satisfy the original Acceptance Criterion because

the payment confirmation is incorrect due to a bug

**Solution**:

1. Create a new User Story for PayNow

US-25: As a user, I want to make a payment online via PayNow so that I can have the convenience of

having orders delivered.

Acceptance Criteria:

• AC1: Pay by PayNow

• AC2: Display payment confirmation for PayNow

Story Points: Estimate story points for US-25.

2. Address the problem with US-01

Reopen US-01 and return it to an incomplete state in Sprint 3. The team corrects the implementation

of AC3 and verifies that all Acceptance Criteria and the Definition of Done are satisfied before marking

US-01 Done again.

If only AC3 requires correction, the remaining work can be estimated at 2 Story Points, for example.