# Threat Model Kit

A portable threat modeling skill designed for Claude Code and Codex that turns ideas, tickets, designs, and repositories into evidence-grounded threat models and actionable engineering security controls.

**Experimental alpha · skill version 0.2.1 · schema 0.2.** The installed command remains `repo-threat-model`. This is a synthesis of established methods, not a validated industry standard. Harness and assessment evidence are documented in [verification](docs/verification.md).

## Install for Claude Code or Codex

These instructions add Threat Model Kit to an existing Claude Code or Codex setup. Choose your agent below, paste the installation prompt, then start a new session and try the example.

### Claude Code

**1. Paste this into Claude Code:**

```text
Install the repo-threat-model skill from https://github.com/cybrking/threat-model-kit/tree/v0.2.1-alpha.1/skills/repo-threat-model into ~/.claude/skills/repo-threat-model. Copy the complete skill folder, including its references, assets, scripts, and LICENSE. Preserve any existing installation and explain how to update it if one is already present.
```

**2. Start a new Claude Code session in the project you want to assess, then run:**

```text
/repo-threat-model Assess this repository and write the threat model, assessment, and engineering security controls to ./threat-model.
```

### Codex

**1. Paste this into Codex:**

```text
$skill-installer Install https://github.com/cybrking/threat-model-kit/tree/v0.2.1-alpha.1/skills/repo-threat-model into ~/.agents/skills. Preserve any existing installation.
```

Codex's built-in Skill Installer downloads the complete skill folder. The installation prompt explicitly selects the user skill directory.

**2. Start a new Codex session in the project you want to assess, then run:**

```text
Use $repo-threat-model to assess this repository and write the threat model, assessment, and engineering security controls to ./threat-model.
```

Both examples produce `security-controls.md`, `assessment.md`, and `model.json` in `./threat-model`. You can also supply an idea, feature, design, or Jira ticket text using the examples below.

<details>
<summary>Manual installation from a terminal (macOS/Linux)</summary>

Download the alpha release:

```sh
git clone --branch v0.2.1-alpha.1 --depth 1 https://github.com/cybrking/threat-model-kit.git
cd threat-model-kit
```

For **Claude Code**, copy the skill into your personal skills directory:

```sh
mkdir -p ~/.claude/skills
if [ -e ~/.claude/skills/repo-threat-model ] || [ -L ~/.claude/skills/repo-threat-model ]; then
  printf '%s\n' 'Already installed. Review your existing skill before updating.'
else
  cp -R skills/repo-threat-model ~/.claude/skills/
  printf '%s\n' 'Installed. Start a new Claude Code session and use /repo-threat-model.'
fi
```

For **Codex**, copy the skill into your user skills directory:

```sh
mkdir -p ~/.agents/skills
if [ -e ~/.agents/skills/repo-threat-model ] || [ -L ~/.agents/skills/repo-threat-model ]; then
  printf '%s\n' 'Already installed. Review your existing skill before updating.'
else
  cp -R skills/repo-threat-model ~/.agents/skills/
  printf '%s\n' 'Installed. Start a new Codex session and use $repo-threat-model.'
fi
```

</details>

**Updating:** obtain the desired release and compare its skill folder with your installed copy before replacing or merging local changes. The installation prompts and manual commands preserve existing installations.

**Validator prerequisites:** Python 3.10+ and `jsonschema` are needed for automated model validation. If `uv` is available, the [validation command](#validate-a-model) supplies the dependency automatically. If validation tools are unavailable, the skill reports that limitation in its assessment.

Installation follows [Claude Code's personal skill layout](https://code.claude.com/docs/en/skills) and [Codex's skill installation guidance](https://learn.chatgpt.com/docs/build-skills). These steps install the skill instructions; authenticated Claude behavioral testing remains outstanding as described in the [verification record](docs/verification.md).

## What engineering receives

The skill produces three linked artifacts:

| Artifact | Purpose |
|---|---|
| `security-controls.md` | Prioritized fix specs: risk, fix, where, observable done-when checks, owner, and timing |
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
