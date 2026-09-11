# IU Site Visit — September 9, 2026

*Conversation summary and asset index. Prepared September 11, 2026. Technical availability and connection results below reflect the September 9 discussion, not a fresh verification.*

## Main takeaway

The central idea was an AI-mediated nursing education experience in which students practice with virtual patients and receive feedback against the American Association of Colleges of Nursing (AACN) Essentials competencies. Canvas is the learning management system used by the institution under discussion.

The recommended design uses an external, versioned collection of competencies and faculty-approved rubrics to guide the tutor. A small pilot can place selected material directly into each session; retrieval-augmented generation (RAG) can select the appropriate material as the collection grows. Canvas integration appears technically feasible through existing MCP implementations and Canvas authorization mechanisms, but an institutional implementation was not selected or built.

## Nursing education concept

The conversation began with a comparison between prerecorded virtual-reality scenarios and experiences that AI could adapt during an interaction. The proposed experience would let a student encounter a patient with a specified profile, ask questions, make decisions, and respond to new challenges.

The educational purpose is to challenge students against defined competencies and give specific feedback. AI-generated conversation could make patient encounters more responsive, while faculty-approved case facts and assessment criteria would keep each exercise aligned with its learning purpose.

This remained an exploratory concept. The session did not produce a VR application, patient simulator, validated assessment system, or selected technology vendor.

## AACN competencies and learning objectives

The discussion established that AACN publicly provides its Essentials framework, competencies, subcompetencies, and progression indicators. The indicators describe observable student behavior and can help educators translate broad competencies into assessable learning objectives. The framework distinguishes entry-level and advanced-level nursing education.

An example discussed was the person-centered care subcompetency concerning a systematic, complete, and accurate patient history. A draft objective for a simulation was:

> During a simulated patient interview, gather and accurately document a structured history covering the presenting concern and relevant health background.

This was an illustrative objective written during the discussion, not an official AACN quotation. Faculty would still define the patient case, required evidence, and scoring criteria.

AACN also publishes nursing practice scenarios mapped to progression indicators, providing possible starting material for a pilot.

### AACN reference assets

| Asset | Use |
| --- | --- |
| [AACN Essentials: Domains and Concepts](https://www.aacnnursing.org/essentials/tool-kit/domains-concepts) | Browse the competency framework. |
| [Progression Indicators](https://www.aacnnursing.org/essentials/tool-kit/domains-concepts/progression-indicators) | Identify observable behaviors and progression expectations. |
| [Domain 2: Person-Centered Care](https://www.aacnnursing.org/essentials/tool-kit/domains-concepts/person-centered-care) | Review the patient-history and communication examples relevant to virtual encounters. |
| [Essentials Competency Assessment](https://www.aacnnursing.org/essentials/essentials-competency-assessment) | Review guidance on assessment, feedback, and formative use of simulation. |
| [Nursing Practice Scenarios](https://www.aacnnursing.org/essentials/tool-kit/nursing-practice-scenario) | Find existing scenarios mapped to competency indicators. |

## Recommended AI architecture

The user explicitly clarified whether RAG was the recommendation. The answer was yes: maintain authoritative curricular material outside the model and supply the relevant portions to each exercise.

Some AACN information may be represented in a model's training, but its exact wording, version, and completeness cannot be assumed. Model memory should therefore not be the authoritative record of the curriculum.

| Component | Proposed responsibility |
| --- | --- |
| General-purpose model | Conduct the patient conversation, explain concepts, and adapt questions. |
| Versioned competency library | Store the applicable AACN competencies, subcompetencies, learner levels, and indicators. |
| Faculty-approved rubrics and cases | Define what evidence counts, how performance is assessed, and the clinical facts of the scenario. |
| Selection and retrieval | Supply the correct requirements and supporting material for each exercise. |
| Student progress record | Track demonstrated behaviors and guide subsequent practice. This was proposed, not implemented. |

For a small pilot, explicit selection by competency and learner level can be sufficient; a vector database or full RAG platform is not necessary immediately. With a larger library, structured lookup can select exact competency records, while semantic retrieval can find supporting material. Fine-tuning was described as optional later work for consistent behavior, not a prerequisite or the primary way to store changing standards.

The proposed learning loop was:

**Select competency → approved patient scenario → student interaction → feedback tied to observed evidence → next challenge**

The recommended starting point was formative practice and coaching. AI scoring would need comparison with faculty judgments before consequential assessment use. Conversation can provide evidence about reasoning and communication; hands-on competencies require appropriate observation. Retrieval alone does not establish that feedback or scoring is correct.

Reference: [Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401).

## Canvas and MCP integration

The user asked whether an MCP server already exists for Canvas LMS. Several community implementations were identified, along with an official Instructure Canvas MCP product-testing listing.

| Asset | Finding from the session |
| --- | --- |
| [Canvas MCP by Vishal Sachdev](https://github.com/vishalsachdev/canvas-mcp) | Documents course content, assignments, rubrics, submissions, grades, and student-progress workflows. |
| [Canvas LMS MCP maintained under the GitHub account bruchris](https://github.com/bruchris/canvas-lms-mcp) | Documents courses, assignments, rubrics, outcomes, quizzes, and grading workflows. |
| [Instructure Product Testing Opportunities](https://community.instructure.com/en/categories/product-testing) | Includes an official Canvas MCP listing describing access with the signed-in user's permissions. |
| [Canvas MCP testing details](https://community.instructure.com/en/discussion/666665/canvas-mcp) | Full details require sign-in; general availability was not established. |

These were documentation findings. No community implementation was installed or tested against the institution's Canvas, and none was established to have Instructure approval. A repository author's full name was not verified for the `bruchris` project; the account name above is the reliable attribution.

For the proposed tutor, Canvas could supply relevant course content, assignments, and rubrics where the authorizing user has access. Write-capable tools also exist in community implementations, but automatic grading or grade submission was not authorized or selected as part of this concept.

Instructure's [Canvas API policy, section 3(i)](https://www.instructure.com/policies/canvas-api-policy) restricts unapproved MCP integrations. The institution's Canvas administrator would need to confirm the approved integration path before deployment.

## Student login: OAuth, OIDC, and tokens

The user asked whether students could use their existing login instead of manually supplying a token. The answer was yes, through an appropriately registered and enabled integration.

| Situation | Mechanism and implications |
| --- | --- |
| Personal development/testing | A user's own manually generated Canvas API token may be used where institution settings permit it. |
| Application used by students | Canvas OAuth 2.0 obtains an access token for each student after sign-in and authorization. The backend manages tokens and refreshes. |
| Tutor launched inside a Canvas course | LTI 1.3 uses OIDC for launch identity and course context. Access to Canvas APIs requires the corresponding authorization. |

The intended student experience is: **Connect Canvas → normal school sign-in → authorize access → begin using the tutor.** Students would not need to disclose passwords to the tutor or manually copy tokens in that design.

The institution must register or enable the application's developer key and permitted API scopes. Delegated API access is constrained by those scopes and the student's existing permissions. LTI launch identity does not automatically grant general Canvas REST API access; LTI services have their own authorization and deployment context.

Canvas's documentation describes manual tokens as a personal testing mechanism and requires OAuth for applications used by multiple users. The community server reviewed documented supplied Canvas API tokens; a complete student OAuth login flow still needed verification or implementation. A student's existing Canvas account alone does not enable an arbitrary third-party integration.

### Authorization reference assets

- [Canvas OAuth 2.0 Overview](https://developerdocs.instructure.com/services/canvas/oauth2/file.oauth)
- [Canvas Developer Keys and Scopes](https://developerdocs.instructure.com/services/canvas/oauth2/file.developer_keys)
- [Canvas OAuth 2.0 Endpoints](https://developerdocs.instructure.com/services/canvas/oauth2/file.oauth_endpoints)
- [LTI Launch Overview](https://developerdocs.instructure.com/services/canvas/external-tools/lti/file.lti_launch_overview)
- [LTI Registration](https://developerdocs.instructure.com/services/canvas/external-tools/lti/file.registration)

## Supporting demonstration: SciNote connectivity

Earlier in the session, the user supplied a SciNote MCP endpoint and requested configuration and testing. The session reported a successful connection, tool discovery, and authenticated account reads after an initial timeout and an authentication-setting correction.

The UI location was resolved and confirmed by the user as **Settings → Plugins → MCPs → SciNote** in the installed desktop app. Earlier directions to a separate MCP settings page did not match that installation.

### SciNote assets

| Asset | Detail |
| --- | --- |
| [SciNote MCP endpoint](https://scinote-mcp.os.mieweb.org/mcp) | Streamable HTTP endpoint using a per-user SciNote credential. |
| [Health endpoint](https://scinote-mcp.os.mieweb.org/healthz) | Reported healthy during the session. |
| [Backing SciNote development instance](https://scinote-dev.os.mieweb.org) | ELN instance accessed by the MCP service. |
| Authentication | SciNote API-key/Bearer authentication; the supplied server intentionally does not advertise OAuth. |

Fifteen tools were reported. Read and navigation capabilities included status, scope selection, teams, projects, experiments, tasks, task steps, task items, inventories, and inventory search. Write capabilities included checklist updates, step completion, result notes, item assignment, and stock consumption. No research-record writes were reported.

The search for a project associated with “Mike Delaney” identified the **Dalaly** team at Summit Biofilm Research Institute as a possible match. The person's identity was not confirmed.

Within **Polymicrobial Biofilm Treatment**, the session reported:

| Experiment or task | Recorded progress at the time |
| --- | --- |
| GingiGuard Oral Gel Cross-Kingdom Biofilm Assay | Seven tasks; none marked complete. |
| GingiGuard Assay – Test Runs / Arm A1 – Run 1 (TEST) | Seven of 21 steps complete; 29 of 94 checklist items checked. |
| Biofilm Growth Protocol / Biofilm Cultivation | Two of four steps complete. |

These are historical notebook progress indicators, not proof that an experiment or instrument was physically running at that moment. Credentials and detailed experimental procedures are omitted from this recap.

## Other conversation context

- The user discussed immersive nursing education, anatomy and physiology simulation, and potential funding. No grant shortlist or application was produced in this session.
- A brief recap also referenced an idea for a Fort Wayne life-sciences investment fund connecting development, research, healthcare, and education. This was background context, not a developed deliverable here.
- Voice and Wi-Fi troubleshooting interrupted the discussion briefly; those exchanges did not change the proposed educational design.

## Follow-up work identified

These are proposed next steps, not assignments that were started:

1. Select a small set of AACN competencies and the intended learner level for a pilot.
2. Have nursing faculty approve patient cases, observable evidence requirements, and feedback rubrics.
3. Define a formative exercise and compare AI feedback with faculty assessment.
4. Ask the Canvas administrator about the official MCP testing program and approved integration options.
5. Verify the selected MCP implementation's support for per-student OAuth and the specific course data needed.
6. Decide whether students launch the tutor from a Canvas course through LTI or connect Canvas from a separate application.

## Deliverable and asset status

The session's reusable assets were the reference links, architecture notes, login-flow explanation, and reported SciNote connection findings collected above. No separate documents, images, interactive visualizations, or source files were found in this conversation's workspace or associated visualization directory when preparing the recap.

This Markdown file consolidates those materials. No nursing tutor, Canvas integration, formal assessment rubric, or funding application was built during the conversation.
