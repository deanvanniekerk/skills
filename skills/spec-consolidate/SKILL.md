---
name: spec-consolidate
description: "Apply one or more spec-review findings files to a spec's merged range: merge the duplicate findings, verify each against the code, apply the confirmed ones, and ship one behaviour-preserving PR."
disable-model-invocation: true
license: MIT
---

# Spec Consolidate

`spec-review` reads a spec's merged range and writes its findings to a file; several reviewers, each on its own model, may have read the same range. You turn their files into one PR that **consolidates** the spec, so it reads as if one author wrote it, the same author as the rest of the codebase.

**Input**: the spec, its merged range `<base>..<head>` with `<head>` pinned, and one or more findings files, each labelled by the reviewer that wrote it (its model, say).

**Decisions** are the product choices the spec records: decision sections in the issues, Deviations in the PR bodies, the user's replies. They are binding. Consolidation keeps behaviour exactly as it is; anything that would change it goes under **Open questions** with a concrete scenario.

A finding is a lead, not proof: you judge every one against the code yourself, by the same rules whichever reviewer raised it. Judge a proposed move by `spec-review`'s [Judo moves](../spec-review/SKILL.md#judo-moves) and [Fit](../spec-review/SKILL.md#fit): prefer the move that deletes code over one that moves it, and the codebase's canonical helper over a new shared one.

## Steps

1. **Read.** The spec, every ticket, every PR body, and the repository's own conventions (`AGENTS.md`, `CLAUDE.md`, contributing and standards docs). Then every findings file, whole.
2. **Merge.** Two findings are one when they touch the same code and propose the same change, whatever their wording; record every file and ID that raised it. Findings that touch the same code with different moves stay separate, for step 3 to choose between. Done when every finding in every file sits under exactly one merged finding.
3. **Verify** each merged finding against the code at `<head>`: read its sites and trace its scenario, and reproduce a bug with a failing test where you can. Each one ends **confirmed**, **discarded** with the reason, or an **open question** when the change would alter behaviour or a Decision. Done when every merged finding has one of the three.
4. **Apply** the confirmed findings, largest first. Run the repository's checks after each, so a consolidation that breaks behaviour is caught where it happened. Give each bug fixed a test. A move too large for one PR becomes a **follow-up**, with its plan.
5. **Publish** one PR (with `blast-it` when it is installed). In the body:
   - the consolidations: what merged into what, and the code it deleted;
   - harmonisations and bugs fixed (file:line, the scenario, the fix and its test);
   - **Open questions**, each with its scenario;
   - follow-ups, each with its plan;
   - discarded findings, one line each.

   With nothing confirmed, publish no PR.

## Report

Finish with `PR <url>, head <sha>, checks <state>`, or `No PR: nothing confirmed` with the open questions. Then a **disposition** table with a row per finding in every file: its file and ID, what became of it (applied, discarded with the reason, open question, or follow-up), and the other files that raised the same finding. Done when the table accounts for every finding in every file and, with a PR, the repository's checks pass on its head.
