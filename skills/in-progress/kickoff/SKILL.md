---
name: kickoff
description: Turn a project description into an approved design and a complete backlog of epics and tickets, from first commit to production deploy, published to the issue tracker.
disable-model-invocation: true
---

# Kickoff

Take a project description and turn it into everything needed to build and ship the system: a confirmed design (for projects with a UI) and a **complete backlog**, a roadmap issue whose epics and tickets cover the whole road from empty repo to production.

The issue tracker and triage label vocabulary should have been provided to you. If not, tell the user to run `/setup-matt-pocock-skills`. With no tracker configured, use the local-files layout in step 9.

Two gates guard the expensive work: nothing is published until the user approves the epic list (gate 1) and the ticket breakdown (gate 2). Stop at each gate and wait.

## Process

### 1. Read the input

The user passes a project description: a file path, a URL, pasted text, an issue reference, or several. Read every one in full, including issue comments.

If the repo already holds code, explore it now: stack, structure, what is built, what is deployed. Already-built work is dropped from the backlog later and noted as "already exists" in the roadmap.

### 2. Check for an existing roadmap

Search the tracker for an open issue labelled `roadmap` (locally: `.scratch/<project>/roadmap.md`). If one exists, stop: point the user at it and explain that extending an existing roadmap is not supported yet.

### 3. Fill the gaps

List every decision the description leaves open, using the areas in [CHECKLIST.md](CHECKLIST.md) as the prompt: users and roles, core features, stack, data, auth, hosting, environments, scale, compliance, integrations, budget. Then call the Skill tool with "grilling" and grill the user on those open decisions only; a decision the description already settles is not asked again.

Done when every checklist area is either settled by the user or settled by the description, and the user has confirmed the summary.

### 4. Design gate (UI projects only)

Infer from the settled decisions whether the system has a user interface, and confirm with the user. With no UI, skip to step 5 and record "Design System: N/A (no UI)" for the roadmap.

With a UI:

1. Pick the 2 or 3 screens that define the product (usually the landing or home screen, the core workflow screen, and one dense data screen).
2. Build **2 or 3 mockup directions** as standalone HTML files under `.scratch/<project>/mockups/`, one file per direction, each rendering those screens with realistic content. Directions must be **structurally different** (layout, hierarchy, density, tone), not palette swaps of one layout.
3. Show the user the files and stop. Wait for them to pick a direction, or to name the bits to combine.
4. From the approved direction, write `DESIGN.md` at the repo root: design tokens (colour, typography, spacing, radii, shadows, motion), light and dark themes if wanted, a component inventory (each component with its variants and states), and layout and accessibility rules.
5. Write the tokens file in the format the settled stack uses (a Tailwind theme, JSON tokens, or CSS custom properties when nothing else is implied).

The mockups stay in `.scratch/` as reference and are not committed.

### 5. Draft the epic list

Walk every item in [CHECKLIST.md](CHECKLIST.md). For each, either propose one or more epics or mark it N/A with a one-line reason. Feature work becomes one epic per domain area, derived from the user stories.

Order the epics with blocking edges:

- **Foundation** (repo, CI, environments, staging deploy pipeline) blocks everything.
- **Design System**, **Data model**, and **Auth** come next; Design System blocks every epic with UI tickets.
- Feature epics follow, blocked by what they truly depend on.
- **Launch** (production hardening and go-live) comes last.

### 6. Gate 1: approve the epics

Present the epic list as a numbered tree: each epic with its title, a one-line goal, and what blocks it. Below it, list every N/A checklist item with its reason, and every "already exists" item.

Ask whether any epic is missing, should be split or merged, or is wrongly N/A. Iterate until the user approves.

### 7. Draft the tickets

For each approved epic, dispatch a subagent, started with a fresh context window. Run them in parallel. Give each one:

- the project summary and settled decisions from step 3,
- `DESIGN.md` if it exists,
- the full approved epic list (so it knows what other epics own),
- its own epic, and the ticket rules below.

Each subagent returns its epic's short spec (problem, user stories, key decisions, out of scope) and its tickets.

<ticket-rules>

- Each ticket is a **tracer bullet**: a narrow but complete vertical slice through every layer it touches (schema, API, UI, tests), demoable or verifiable on its own.
- Each ticket fits a single fresh agent session.
- Each ticket lists the tickets that **block** it, inside this epic or in another epic by title. A ticket with no blockers can start immediately.
- Prefactoring and setup come first in the epic.
- Each ticket carries checkable acceptance criteria.
- No file paths or code snippets; they go stale. Design tokens and component names from `DESIGN.md` are fine.

</ticket-rules>

Check the returned drafts together: remove duplicates across epics, and fix any blocker that names a ticket that does not exist.

### 8. Gate 2: approve the tickets

Present each epic's tickets as a numbered list: title, blocked by, what it delivers. Ask whether the granularity is right, the blocking edges are right, and anything should be merged, split, or added. Iterate until the user approves.

### 9. Publish

Create any missing labels (`roadmap`, `epic`, and the AFK-ready triage label); never rename existing ones. Then publish in dependency order, blockers first, so every reference points at a real identifier:

1. The **roadmap** issue, labelled `roadmap`, using the template below.
2. Each **epic** as a sub-issue of the roadmap, labelled `epic`, body is its short spec. Set native blocking edges between epics.
3. Each **ticket** as a sub-issue of its epic, labelled with the AFK-ready triage label, using the ticket template below. Set native blocking edges.
4. Update the roadmap body so every epic line links to its issue.

When the tracker has no native sub-issues or blocking, put `Part of <parent>` at the top of the child body and a `## Blocked by` section listing blockers.

**Local files** layout, when no real tracker is configured:

```
.scratch/<project>/roadmap.md
.scratch/<project>/<NN>-<epic-slug>/epic.md
.scratch/<project>/<NN>-<epic-slug>/<NN>-<ticket-slug>.md
```

Numbered in dependency order, blockers first, one ticket per file, blocking edges written as text.

Finish by giving the user the roadmap link and the count of epics and tickets.

<roadmap-template>

## Project

<two or three lines: what the system is, who it serves>

## Settled decisions

<one line per decision from step 3: stack, hosting, auth, data, environments>

## Design

<link to `DESIGN.md`, or "N/A (no UI)">

## Epics

- [<epic title>](link): <one-line goal>. Blocked by: <epic titles, or none>

## Not applicable

- <checklist item>: <reason>

## Already exists

- <checklist item>: <what is already built>

</roadmap-template>

<ticket-template>

## Parent

<the epic>

## What to build

<the end-to-end behaviour this ticket makes work, from the user's perspective>

## Acceptance criteria

- [ ] <criterion>

## Blocked by

<each blocking ticket, or "None (can start immediately)". Omit when set as native edges.>

</ticket-template>
