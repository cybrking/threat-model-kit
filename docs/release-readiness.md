# Release readiness

Approved project name: **Threat Model Kit**. Approved GitHub repository: **threat-model-kit**. Keep the portable installed command `repo-threat-model` for compatibility. The descriptive name received a limited collision search; the selected GitHub destination was checked before creation; trademark clearance is not established.

Before public alpha publication:

- Owner `cybrking`, repository `threat-model-kit`, and Apache 2.0 were approved on 2026-10-01. LICENSE is included at the repository and portable-skill roots.
- Review the exact candidate file list and content; publish only this isolated tree.
- Pass local regression, CLI, format, relative-reference, and extracted-package checks. Run `python scripts/check_release.py --require-license` before publication.
- Review the behavioral evaluation evidence and describe untested harness paths honestly.
- Enable private vulnerability reporting or supply an appropriate private route.
- Create the repository and run its CI; a prepared workflow is not evidence that GitHub CI has run.
- Tag an alpha release only after these checks, with a release description listing known limitations.

Before describing harness support as verified: run the latest packaged entrypoint through both authenticated harnesses using repo and non-code inputs, independently validate all outputs, and review engineering handoff quality. Claude authentication has previously blocked this work.

Before production claims: run representative web/API, privacy-sensitive, and AI/tool-system pilots, measure actionable findings and false/missed findings, verify lifecycle updates and scoped AWS observation behavior, and demonstrate remediation outcomes. Alpha publication does not meet those production criteria.

The [release notes](release-notes.md) supply the approved repository metadata and alpha announcement text.
