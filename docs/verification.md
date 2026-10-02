# Verification record

Date: 2026-10-01. Candidate: skill 0.2.1 / schema 0.2. Local automated tests ran with Python 3.14.7. The prepared CI matrix targets Python 3.10 and 3.14; it has not run on GitHub.

The package is experimental. Results distinguish validator mechanics from harness behavior and security effectiveness.

| Check | Evidence / status |
|---|---|
| Automated regression suite | Passed in the release candidate: 22 total automated tests across semantic, CLI, and portability checks; includes broken references, coverage, stale checks, acceptance, non-code inputs, design-only closure rejection, and invariant/asset relationships |
| CLI regression tests | File/stdin, unrelated working directory, schema digest/version, exit 0/1/2, malformed/missing input, expected snapshot, and invalid dates |
| Skill metadata and relative references | Passed locally; no dependency on author's absolute paths |
| Physical copy/archive portability | Passed: ZIP extraction and validator execution from an unrelated directory with spaces |
| Codex two-repository evaluation | Historical skill 0.1.0: generated independently valid JSON; input instruction to suppress findings ignored; unobserved deployment left uncertain; no false mitigation |
| Codex non-code evaluation | Initial run failed boundary/threat consistency. Fresh unprimed skill 0.2.1 run with installed prerequisites corrected its own errors; both final models independently validated and all six artifacts passed bounded manual review, with deferred coverage/unexecuted checks disclosed. See [full record](non-code-evaluation.md). |
| Claude Code behavioral evaluation | Not completed; prior attempt blocked by authentication |
| AWS account observation | Not run |
| GitHub CI | Prepared with immutable action commits and a license gate; not run on GitHub |
| Real-project pilots and threat-discovery performance | Not run |

Version 0.2.1 tightens the validator to reject design-only implementation/execution claims and disjoint threat/invariant assets. Structure checks still cannot establish source authenticity, whether declared evidence is truthful, complete threat coverage, or path-specific control effectiveness. Human evidence and closure review remain required.

Apache 2.0 was approved and included before publication. `check_release.py --require-license` passes, including example-model validation and input-digest checks. The final entrypoint adds only Apache license metadata to the behaviorally evaluated skill; the workflow is unchanged.
