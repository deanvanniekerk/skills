---
name: spec-review
description: "Review a spec's merged tickets once agents built them independently: find the judo moves that collapse duplicated code, the code that does not fit its neighbours, and the bugs where tickets meet, and write them up as findings for `spec-consolidate`."
disable-model-invocation: true
license: MIT
---

# Spec Review

A spec's tickets were built by agents working independently, each reviewed against its own ticket. No agent saw the others' work, so the merged result usually solves the same idea two or three ways, each in its own style. Now the whole range is in view. Find what **consolidating** it takes: the judo moves that make the duplicated code disappear, until the spec reads as if one author wrote it, the same author as the rest of the codebase.

This is a review: it produces a findings file, and `spec-consolidate` applies it. Several reviewers can run over the same range at once, each on its own model, and `spec-consolidate` takes all their files. Leave the repository as you found it.

**Input**: the spec (a parent issue and its sub-issues, or one big issue alone) and the merged range `<base>..<head>`, where `<base>` is the default branch just before the spec's first PR. Resolve whichever the user gives you to both, and pin `<head>`.

**Decisions** are the product choices the spec records: decision sections in the issues, Deviations in the PR bodies, the user's replies. They are binding. Consolidation keeps behaviour exactly as it is; anything that would change it goes under **Open questions** with a concrete scenario.

**Output**: one Markdown file at the path your brief gives, or outside the repository when none is given.

## Steps

1. **Read.** The spec, every ticket, every PR body, then the whole diff of the range. Read the repository's own conventions (`AGENTS.md`, `CLAUDE.md`, contributing and standards docs).
2. **Inventory.** List everything the range adds: functions, types, schemas, components, hooks, queries, constants, error codes, test helpers and fixtures. For each, find its siblings, first in the other tickets' code, then in the codebase before `<base>`. Done when every item on the list names its siblings, or says it has none.
3. **Find the judo moves** in that inventory, using [Judo moves](#judo-moves). Rank them by how much code and how many concepts each one deletes.
4. **Harmonise.** Read each ticket's code against its neighbours, using [Fit](#fit).
5. **Check the seams** between tickets, using [Seams and drift](#seams-and-drift).
6. **Verify** every finding against the code at `<head>`. Reproduce a bug with a failing test where you can, copy the test into the finding, and then remove it. Discard anything you cannot confirm.
7. **Write the findings file**, largest move first. Each finding gets:
   - an ID (`F1`, `F2`, …) and its kind: consolidation, harmonisation, bug, seam, coverage or drift;
   - every site it touches, as `file:line`;
   - what is wrong, with the scenario or the duplicated code side by side;
   - the move: what stays, what goes, and where the survivor lives;
   - its size: the lines and concepts it deletes;
   - how you verified it: the test that fails, or the path you traced.

   Then **Open questions**, each with its scenario, and **Discarded**, one line each. Write each finding so it can be applied by someone who has not read the range: name the code, never "the helper above".

   Done when every confirmed finding is in the file and the repository's working tree is as you found it. Finish with `Findings <path>, <n> findings, <m> open questions, at <head sha>`.

## Judo moves

A judo move uses the codebase's existing structure so that a whole category of code disappears, rather than being tidied. The test is what a reader has to hold in their head afterwards: fewer concepts and fewer lines, not the same ones rearranged.

Across tickets, look for:

- **Duplicates.** Two tickets each wrote a helper, type, query, component or fixture for the same thing. Keep one, in the layer that owns the concept.
- **Near-duplicates.** The same shape under different names, or differing by one parameter. Make it one, or use the canonical version the codebase already has.
- **Parallel mechanisms.** A ticket built its own way of doing what the codebase already does, such as state, data fetching, formatting, validation, error mapping or permissions. Move it onto the existing mechanism.
- **Special cases.** A conditional one ticket added that another ticket's model now answers. Let the model answer it.
- **Scattered policy.** One rule restated in several places. Give it a single source of truth and point every reader at it.
- **Layers.** A wrapper or pass-through that only forwards. Delete it.
- **Growth.** A file the range pushed past 1,000 lines, or a module several tickets each extended in their own direction. Split it along the seams the spec reveals.

Prefer the move that deletes code over one that moves it. Prefer the codebase's canonical helper over a new shared one.

## Fit

The spec should look like the rest of the codebase grown, not bolted on:

- names that match the domain vocabulary and their neighbours;
- each piece in the package, layer and folder that owns its concept;
- the codebase's own patterns for errors, permissions, tenancy, data access and UI composition;
- tests that follow the codebase's testing style and assert something a regression would break.

## Seams and drift

These are what no per-ticket review could see:

- **Seams.** Every state or rule a ticket added, crossed with every other ticket's. Where two meet with no stated outcome, it is a bug if the spec implies the answer, and an open question if it doesn't.
- **Coverage.** Everything the tickets delivered together, against the parent spec: a requirement dropped or half-delivered between tickets, or two decisions that contradict each other.
- **Drift.** Docs, glossary entries, comments and tests that a later ticket left wrong, and tests that pass whatever the code does.
