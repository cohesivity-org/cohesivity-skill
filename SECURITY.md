# Security Policy

## Reporting a vulnerability

Report suspected vulnerabilities privately to
[accounts@cohesivity.ai](mailto:accounts@cohesivity.ai). Include:

- the affected skill version or commit;
- the behavior you observed and its security impact;
- minimal reproduction steps; and
- any suggested mitigation.

Do not open a public issue for an unpatched vulnerability. Do not include live
`coh_management_key`, `coh_application_key`, wait tokens, tenant data, or other
secrets in a report. If a credential may have been exposed, stop using it and
include only a redacted prefix or tenant identifier in the report.

This repository is a generated public mirror. Reports about the live API,
bootstrap flow, credential handling, or instructions served from
`cohesivity.ai` should use the same private contact.

## Supported version

Security fixes target the latest skill served at
<https://cohesivity.ai/skill.md> and then propagate to this mirror. Compare the
`version` field in the installed `SKILL.md` with the live version when reporting
an issue.
