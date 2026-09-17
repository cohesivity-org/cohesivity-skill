# Changelog

## 2026-09-16 — Publish four-tool MCP handoff guidance

Copy generated skill `d309e051978d` into both mirror paths. It enumerates the
four supported MCP tools, requires a human handoff for unsupported mutations,
removes obsolete deployment/billing/claim tool instructions, and pins the
coordinated initializer 0.7.1 fallback. Confirmation and the prohibition on
direct control-plane writes are preserved.

Both files are byte-identical to the canonical candidate's generated export:
16,060 bytes, SHA-256
`10b03850ecd87564b457d2df0fcb1a5e6cf3ae205fb695c27114be6c95018e59`.
The metadata hash was recomputed from the body and matches. This is a generated
mirror candidate for the coordinated core/plugin/initializer release; it does
not deploy the canonical skill or publish initializer 0.7.1.

## 2026-09-16 — Publish guest and account MCP guidance

Copy generated skill `7f2fbc207f1d` into both mirror paths. The four-tool
workflow now distinguishes guest ephemeral creation from account-owned claimed
creation, documents browser-authorized credential downloads, and makes local
creation run the complete quickstart flow. Optional local login/logout keeps
account credentials outside projects. Confirmation and the unsupported-write
handoff remain mandatory; a guest grant does not acquire account access when
its tenant is claimed.

Both mirror files match the canonical candidate byte-for-byte: 18,933 bytes,
SHA-256 `755f0fed995635cc722ea7ca0987b91e16749b5dcc3fe2a80ae0e540389005db`.
The body hash matches `metadata.version`, and whitespace verification passes.
This is coordinated release guidance for plugin 4.0.0 and initializer 0.8.0,
not a claim that either candidate is published or the Worker is deployed.

## 2026-09-17 — Hand hosted credentials to the calling agent

Publish generated skill `c098834bea25` in both mirror paths. Hosted
`create_tenant` now supplies `credentials_file` for the calling agent to write
verbatim into a private, gitignored project file. Browser download is optional
for clients without a writable workspace. The guidance explicitly covers
retained tool history, existing-tenant protection, and reporting failed writes;
local MCP remains metadata-only and other hosted results retain redaction.

Both files match the generated candidate: 20,265 bytes, SHA-256
`ccdc71a865d775709339869a1ae029f88a8f9d789e0cb6fde17e81fe60423d9c`.
The body hash matches the metadata version, and 28 discovery/skill tests pass.
This coordinates plugin 4.0.1 with unpublished initializer 0.8.0; no merge,
publication, or deployment is performed here.
