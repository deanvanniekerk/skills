# Review brief

Fill in the bracketed fields and send the text below as the reviewer's whole brief. The reviewer starts with nothing else, so give it paths and SHAs it can use as written.

```text
Review the committed change in <absolute repo path>: `git diff <base-sha>...HEAD`,
commits `git log <base-sha>..HEAD --oneline`. The spec is <issue URL>. Already decided
or already raised: <list, or "nothing">.

Review only: read the whole diff and the source around it, and leave every file as it is.

Follow the code-review skill at <absolute path of its SKILL.md>, with <base-sha> as its
fixed point and the issue above as its spec. It runs the Standards and Spec axes. Review
a third axis yourself beside them, Defects:

Find concrete defects in the change. Prioritize state transitions, async races, domain
invariants, cross-package behavior, and schema compatibility. Trace the callers and
consumers you need to establish a failure: the inputs or reachable state that produce a
wrong result, a crash, or a security failure, and its consequence. Grade each by impact:
- P1: a reachable defect causing substantial harm, such as corrupted data, incorrect
  billing, unauthorized access, or a broken core workflow. Name the trigger and the impact.
- P2: another concrete defect or fragility with a named, plausible failure scenario.
When unsure of the grade, it is P2.

Scope is the issue, on every axis: each finding bears on a requirement the issue states
or on code the diff adds. These are follow-ups, not findings: a defect in behavior the
diff leaves as it was, a path the issue never reaches, compatibility the issue does not
ask for (such as an old client against the new server, unless the repository documents
that they deploy separately), and a design you would prefer. Style, wording, extra test
coverage without a demonstrated gap, a description of what the diff does, and anything
on the already-decided list are outside the review.

Reply with code-review's report plus a `## Defects` section after its two, each finding
as `P1|P2 · file:line` followed by the scenario, the evidence, and the consequence. With
no defects, say what you traced. Then `## Follow-ups`, one line each. If you could not
read all of the diff or the requirements, say what is missing: an incomplete review is
not a clean one.
```
