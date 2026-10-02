# Threat Model Kit

A portable threat modeling skill designed for Claude Code and Codex that turns ideas, tickets, designs, and repositories into evidence-grounded threat models and actionable engineering security controls.

**Experimental alpha · skill version 0.2.1 · schema 0.2.** The installed command remains `repo-threat-model`. This is a synthesis of established methods, not a validated industry standard. Harness and assessment evidence are documented in [verification](docs/verification.md).

## What engineering receives

The skill produces three linked artifacts:

| Artifact | Purpose |
|---|---|
| `security-controls.md` | Prioritized requirements, enforcement points, owners, and testable acceptance criteria |
| `assessment.md` | Scope, assumptions, evidence review, visibility gaps, check results, and owner decisions |
| `model.json` | Versioned, machine-readable assets, trust boundaries, attack scenarios, controls, and checks |

For example, an early tenant-export feature could produce this **proposed**, unverified requirement:

| Control | Requirement | Enforcement | Acceptance criteria |
|---|---|---|---|
| Tenant-scoped exports | Authorize every export against the authenticated user's current tenant membership | Request handler, worker data access, status endpoint, download handler | Tenant A cannot request, poll, or download tenant B's export; access revoked before execution or download denies access |

See the fictional [tenant-export controls](examples/tenant-export/security-controls.md) and [AI-support controls](examples/support-assistant/security-controls.md), produced in the [recorded evaluation](docs/non-code-evaluation.md). They are partial, proposed design requirements; adapt them to actual scope and evidence.

An idea can be assessed before code exists. The skill records described capabilities and design assumptions, then refines them when code and deployment evidence become available.

## Inputs

Supply any combination of a pasted idea, feature request, Jira ticket text or accessible ticket reference, design document, or repositories. Optional AWS read access can supply in-scope configuration observations. No Jira connector, AWS SDK, or cloud access is required for basic modeling. A ticket key alone requires a readable connector or supplied content; the skill cannot retrieve inaccessible context.

## Install

Clone the public repository, then run the installation commands from its root:

```sh
git clone https://github.com/cybrking/threat-model-kit.git
cd threat-model-kit
```

Python 3.10+ and `jsonschema` are needed for the validator; the modeling instructions run through your authenticated agent harness.

For Claude Code:

```sh
mkdir -p ~/.claude/skills
test ! -e ~/.claude/skills/repo-threat-model && test ! -L ~/.claude/skills/repo-threat-model && cp -R skills/repo-threat-model ~/.claude/skills/
```

For Codex:

```sh
mkdir -p ~/.agents/skills
test ! -e ~/.agents/skills/repo-threat-model && test ! -L ~/.agents/skills/repo-threat-model && cp -R skills/repo-threat-model ~/.agents/skills/
```

These commands leave an existing installation untouched. To upgrade, review local changes and replace or merge the skill deliberately. Restart the harness if the skill is not discovered. Installation paths follow [Claude Code skills](https://code.claude.com/docs/en/skills) and [Codex skills](https://learn.chatgpt.com/docs/build-skills). This is a skill folder, not a plugin package.

## Use

In Claude Code:

```text
/repo-threat-model Assess this feature: tenant admins can request an asynchronous customer export and download it later. No code exists yet. Write the model, assessment, and engineering security controls to ./threat-model.
```

In Codex:

```text
Use $repo-threat-model to assess the Jira ticket text below. Record unresolved architecture decisions and provide proposed controls with abuse-case acceptance criteria. Write outputs to ./threat-model.
```

For repositories, in either harness:

```text
Use the repo-threat-model skill to assess ./api and ./infrastructure. No AWS access or active testing is authorized. Preserve deployment uncertainty and produce the three artifacts.
```

For AWS observations, supply an existing authorized profile/identity plus accounts, regions, and relevant service scope. Do not paste credentials. The skill performs authorized read-only configuration observation; repository execution, active exploitation, customer-record retrieval, and infrastructure changes require separate authorization.

## Validate a model

Use an existing Python environment with `jsonschema`, or run with `uv`:

```sh
uv run --with 'jsonschema>=4.23,<5' python skills/repo-threat-model/scripts/validate_model.py threat-model/model.json --expected-snapshot YOUR_BUNDLE_ID --as-of 2026-10-01
```

The date is an example; supply the assessment date. Bundle IDs identify the inputs and timestamped observations assessed, not an invented deployment revision.

Exit 0 means recorded structure and consistency pass. It does **not** verify evidence authenticity, threat completeness, control effectiveness, or release approval. Read `coverage_gaps` and `review_gaps`, inspect source evidence, and obtain authorized owner decisions. Proposed controls and unexecuted checks can be honestly represented in a valid model. Design-only implementation or execution claims are rejected, but forged or mislabeled evidence still requires substantive review.

## Develop and test

```sh
python3 -m venv .venv
.venv/bin/python -m pip install -r requirements-dev.txt
.venv/bin/python -B -m unittest discover -s skills/repo-threat-model/tests -p 'test_*.py' -v
.venv/bin/python scripts/check_release.py
```

The CI workflow runs local checks; it does not call paid model APIs, read Jira, or connect to AWS. Behavioral evaluations require an authenticated harness and the [evaluation rubric](docs/evaluation.md). See [release readiness](docs/release-readiness.md), [contributing](CONTRIBUTING.md), and [security reporting](SECURITY.md).

## Framework and versioning

The [framework](skills/repo-threat-model/references/framework.md) combines harm-first scope, STRIDE, privacy analysis, attacker capabilities, attack paths, business misuse, and testable security requirements. The AI profile is conditional on the assessed system containing AI. [Method comparison and primary-source attribution](skills/repo-threat-model/references/comparison.md) explain the synthesis and its tradeoffs; this project does not claim ownership of those methods.

Schema 0.2 accepts non-code sources as well as repositories. Earlier 0.1 models require explicit migration: preserve the original, review input provenance, change the schema version only after that review, and revalidate relationships and closure claims. A validator version is distinct from a schema version.

## License

Licensed under [Apache 2.0](LICENSE). The portable skill folder also includes the license so copied installations retain it.
