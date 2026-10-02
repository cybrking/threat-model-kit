# Security controls for engineering

Input: <ticket/feature/idea/design/repositories and snapshot>
Maturity: <idea/design/implementation/deployment>
Priorities: <policy reference or provisional>

## Work to prioritize

| Priority | Control / threats | What to build | Where enforced | Owner | Status |
|---|---|---|---|---|---|
| <priority> | <control ID / threat IDs> | <concrete requirement> | <logical component or evidenced file/service> | <owner or owner needed> | <proposed/implemented; checks pending/verified> |

## Copyable implementation tasks

### <Control ID>: <short action title>

- **Requirement:** <specific behavior engineers must implement>
- **Why:** <attack scenario and harm; threat IDs>
- **Enforcement point:** <server/tool/worker/storage boundary; conditional if undecided>
- **Owner and timing:** <role/person or owner needed; before implementation/before release/optional hardening>
- **Acceptance criteria:** <observable success and abuse/negative cases, not “follow best practices”>
- **Validation:** <check IDs, planned procedure, and actual results if available; otherwise not run>
- **Dependencies / decisions:** <assumptions, design choices, and conditions that change the requirement>
- **Residual risk:** <remaining harm and required owner decision>

Repeat only for distinct controls; share one requirement across related threats. Keep IDs/statuses consistent with model.json. Early design requirements remain proposed until implementation and validation evidence are available.
