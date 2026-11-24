# Validation Results

Date: 2026-11-24
Skill version: 0.1.0

## Evidence

An earlier app-specific prototype was used to create an issue. That experience informed the workflow, but does not prove the new generic English-only package works across apps.

## Current revision

- The skill is generalized to arbitrary authorized app/API/database sources.
- English output is required regardless of feedback language.
- Source selection, field discovery, GitHub destination, and read-only source access are explicit.
- Stable source references and failure handling are included.
- The bundled skill-creator validator returned "Skill is valid!" for skills/report.
- Local checks passed for matching folder/frontmatter identity, version 0.1.0, UI metadata, README relative links, and removal of the previous app-specific and personal-path content. These are structural checks, not behavioral evaluations.
- Behavioral cases are defined in cases.md. No independent execution or passing score is claimed.
- No live app/database integration or GitHub issue creation was attempted for this revision.
- The desktop /report picker is documented by host behavior but has not been tested with a local installation of this package.

## Remaining acceptance checks

Run a draft with real selected feedback. Then, in an authorized test repository, verify submission, duplicate lookup, and error handling through the user's actual connectors. Install the skill and verify the desktop picker and CLI fallback. These require the relevant runtime connections and an explicit test selection.

Preparing this local package does not publish a GitHub repository. The owner must review and approve publication first.
