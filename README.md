# Augment7 SignAR Documentation Repository

This repository contains Augment7's project and technical documentation for SignAR. The source code, model runtime, local training toolkit, configuration, and deployment workflow belong in the separate **System Repository**. This repository keeps the project records needed to understand the proposal, current system, evaluation, operation, and decisions.

## Project overview

SignAR is a Filipino Sign Language (FSL) recognition and learning project. The original [group proposal](<./02 - Proposal/Group Proposal.docx>) describes a mobile application with real-time sign recognition, text output, AR visualization, a digital sign chart, and interactive learning.

The current working system is an installable browser application. It recognizes signs supported by its active model and shows the accepted label and configured meaning in a two-dimensional card over the live camera preview. It supports transcript editing and optional speech. It is not a native mobile app, does not spatially anchor 3D content, and does not translate arbitrary FSL conversation. A separate sign chart and structured lesson mode are not in the current release. The group should record whether these proposal items are deferred or remain final project commitments.

## Document map

| Document | Purpose and status |
|---|---|
| [Repository compliance checklist](./repository-compliance-checklist.md) | Tracks the required two-repository split, contents, contributor history, instructor access, privacy, and final review. |
| [Section 2: Original group proposal](<./02 - Proposal/Group Proposal.docx>) | Preserves the source proposal and initial project direction. |
| [Section 3: Proposed project direction](<./03 - Proposed Final Project Direction/Member 1/project-direction.md>) | Describes the working browser scope and its relationship to the original proposal. Draft for group review. |
| [Section 4: System architecture](<./04 - Preliminary System Architecture/Member 2/system-architecture.md>) | Documents the public decoder, private training toolkit, model contract, and deployment boundary. Draft for group review. |
| [Section 5: Use cases and user stories](<./05 - Preliminary Use Cases and User Stories/Member 3/use-cases-user-stories.md>) | Describes learner, collector, and model-review workflows. Coordinate with [Member 5's co-authored use case](<./05 - Preliminary Use Cases and User Stories/Member 5/coordinated-sign-use-case.md>). |
| [Section 6: Technology stack](<./06 - Preliminary Technology Stack/Member 4/technology-stack.md>) | Lists browser, tracking, inference, training, and evaluation technologies. Confirm exact versions before final submission. |
| [Section 8: Three-month timeline](<./08 - Preliminary Gantt Chart and Timeline/Member 6/gantt-timeline.md>) | Plans project work around the proposal's three-month duration. Dates are illustrative pending the group's calendar; coordinate with [validation gates](<./08 - Preliminary Gantt Chart and Timeline/Member 7/validation-gates-and-contingencies.md>). |
| [Operations and project records](<./09 - Operations and Project Records/README.md>) | User and administrator manuals, testing documentation, deployment record, feedback log, issue/improvement log, and audit log. Evidence templates are pending actual project records. |
| Supporting drafts | [Implementation plan](<./Supporting Drafts/Member 5/implementation-plan.md>) and [integration/validation plan](<./Supporting Drafts/Member 7/integration-validation.md>) support the report but are not additional numbered sections. |

## Contributions and recordkeeping

- Contributors maintain their assigned sections and review shared changes with affected owners.
- Coordinate the co-authored material in Sections 5 and 8, then agree on one final version for the report.
- Keep commit authorship and pull-request history consistent with the work members actually completed. Do not fabricate or backdate contributions, testing, deployment, feedback, or decisions.
- Give all group members and the instructor access to this repository. Update the approved documents and records before final submission.
- Keep raw recordings, identifying details, private datasets, local environments, and generated training artifacts out of both public repositories. Store only consented, privacy-reviewed, redacted feedback summaries here.
- Mark drafts and pending evidence clearly. A blank template is not proof that an activity occurred.

The [Documentation Repository `.gitignore`](./.gitignore) provides privacy defaults. Add the confirmed System Repository link to this README after the group identifies the official repository.
