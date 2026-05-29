# System Design

## Architecture

```text
Browser UI
   ↓
Local HTTP Server
   ↓
Study Planning Agent
   ↓
Perception Module
   ↓
Decision Module
   ↓
Action Module
   ↓
JavaScript Data Store with Task Status: web/data/plans.js
   ↓
Interactive Browser UI
```

## Module responsibilities

- `web_server.py`: serves the browser UI, static assets, plans.js, API routes, and fallback record detail pages.
- `StudyPlanningAgent`: coordinates the planning workflow behind the API.
- `PerceptionModule`: interprets the raw study goal into a topic, time window, difficulty, and task type.
- `DecisionModule`: turns the perceived state into a structured plan, planning strategy, priorities, and warnings.
- `ActionModule`: packages the current plan record and applies feedback revisions in place.
- `MemoryModule`: persists plan records, feedback revisions, deletions, and task-status updates through `web/data/plans.js`.
- `HTMLViewModule`: renders the main browser page shell and record detail pages.

## Design note

The web interface is the interaction layer of the prototype. It collects the user's goal, feedback, task-status changes, and delete actions, calls the local HTTP API, and renders the results in the browser without leaving the main page. The actual intelligent behavior remains inside the agent modules, especially perception, decision-making, action, and memory.

The system no longer depends on Markdown files for display. `plans.js` is used as a lightweight local data store because it allows the browser to render records directly, keeps the prototype easy to inspect in GitHub, and avoids a more complex persistence layer for this assignment. Feedback updates revise the current plan record, task progress is stored alongside the plan, and deleting a plan removes it from the same local data store, so the interface behaves like an interactive task planner rather than a document viewer.
