# Defect review brief

Fill in the bracketed fields and send the text below as the defects reviewer's whole brief. The reviewer starts with nothing else, so give it paths and SHAs it can use as written.

```text
Review the committed change in <absolute repo path>: `git diff <base-sha>...HEAD`,
commits `git log <base-sha>..HEAD --oneline`. The requirements are at
<issue URL or spec path>. Already decided or already raised: <list, or "nothing">.

Review only: read the whole diff and the source around it, and leave every file as it is.

Find concrete defects. Prioritize state transitions, async races, domain invariants,
cross-package behavior, schema compatibility, and release ordering. Trace the callers
and consumers you need to establish a failure. Each finding sits on one axis:
- Correctness: the inputs or reachable state that produce a wrong result, a crash, or a
  security failure.
- Spec: the requirement, the behavior that is missing or wrong, and its consequence.
- Standards: the repository rule and the concrete consequence of breaking it. Repository
  conventions outrank generic preferences.

A finding needs a behavioral, data, security, or operational consequence. Style, wording,
extra test coverage without a demonstrated gap, a description of what the diff does, and
anything on the already-decided list are outside this review.

Grade each finding by impact:
- P1: a reachable defect causing substantial harm, such as corrupted data, incorrect
  billing, unauthorized access, or a broken core workflow. Name the trigger and the impact.
- P2: another concrete defect or fragility with a named, plausible failure scenario.

Reply with a one-line verdict, then each finding as `P1|P2 · axis · file:line` followed by
the scenario, the evidence, and the consequence. With no findings, say what you traced. If
you could not read all of the diff or the requirements, say what is missing: an incomplete
review is not a clean one.
```
