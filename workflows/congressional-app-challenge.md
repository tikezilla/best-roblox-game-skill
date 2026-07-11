# Workflow: Congressional App Challenge Roblox Project

Use this workflow for Congressional App Challenge or school competition projects built in Roblox. The goal is a playable, original, explainable app with strong UX and documented student contribution.

## Pre-Build Brief

Define these before construction:

1. One-sentence purpose
2. Target user
3. Concrete problem
4. Reason Roblox is part of the solution
5. Original interactive mechanic
6. Playable vertical slice
7. Three technically explainable systems
8. User experience plan
9. Testing plan
10. Student contribution log
11. Three-minute demonstration plan

Do not start by choosing a generic Roblox genre. Use a genre template only when a system clearly supports the app concept.

## AI Usage and Student Contribution

AI support is permitted only when disclosed in submission materials and when the project still shows significant student contribution and technical understanding. Maintain a contribution log that separates:

- Student ideas, design choices, testing, and modifications
- AI-assisted code or documentation
- Third-party libraries, models, audio, images, or packages
- Bugs the student diagnosed or fixed
- Technical concepts the student can explain to judges

Never tell the student to hide AI usage or claim unsupported authorship. Help the student understand and explain every major system.

## Project Documentation Files

Create and maintain:

```text
docs/
|-- concept-brief.md
|-- architecture.md
|-- decision-log.md
|-- testing-log.md
|-- technical-challenges.md
`-- demo-video-outline.md
```

These files support submission answers about purpose, audience, tools, functionality, technical difficulty, lessons learned, and a future version.

## Build Sequence

1. Write `docs/concept-brief.md` with purpose, audience, problem, and Roblox rationale.
2. Define the playable vertical slice in one paragraph.
3. Choose exactly three technical systems for v1, such as simulation logic, UI state, data visualization, NPC guidance, quiz/decision engine, or progress tracking.
4. Create `docs/architecture.md` with service ownership, script manifest, RemoteEvents, and data needs.
5. Get one architecture approval before implementation when changing Studio.
6. Build the vertical slice first.
7. Playtest, read console output, and record results in `docs/testing-log.md`.
8. Add UX polish using `references/visual-direction-ux.md`.
9. Run `workflows/competition-polish-audit.md`.
10. Write the demo video outline last, based on the working app.

## Judging Audit

Before submission, verify:

- Originality: the app has a clear original mechanic or perspective.
- Implementation: the core feature works in a playable Roblox place.
- UX and design: the target user can understand what to do without a long explanation.
- Programming skill: at least three systems are technically explainable.
- Source access: the app and code can be shown if requested.
- Disclosure: AI and third-party resources are documented.