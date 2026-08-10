# Cohesivity agent skill

[![skills.sh](https://skills.sh/b/cohesivity-org/cohesivity-skill)](https://skills.sh/cohesivity-org/cohesivity-skill)

The agent playbook for [Cohesivity](https://cohesivity.ai): on-the-fly backend
infrastructure purpose-built for AI agents. One HTTP API provisions databases,
hosting, auth, realtime, storage, and AI model access, and the agent provisions
on the user's behalf.

## Install

With the [`skills`](https://github.com/vercel-labs/skills) CLI:

```bash
npx skills add cohesivity-org/cohesivity-skill
```

Or set up a whole project with the npm package. By default it installs the
plugin (the skill plus a local project MCP), creates or reuses an ephemeral
tenant, and writes local config:

```bash
npx @cohesivity/init
```

When Node is unavailable, use the equivalent quickstart:

```bash
curl -fsSL https://cohesivity.ai/quickstart.sh | bash
```

The plugin lets future projects call MCP `create_tenant` when backend needs
arise, so users do not need to name Cohesivity or rerun the installer. If a
user explicitly opts out of the plugin, pass `--no-plugin`; this installs the
standalone skill instead and still bootstraps the current project.

## What's here

- `skills/cohesivity/SKILL.md` — the skill, for the `skills` CLI.
- `cohesivity.skill.md` — the same content at the path the `@cohesivity/init`
  package pins.

The two skill files are generated from the canonical source and carry a content
hash at the YAML path `metadata.version` in their frontmatter. It is exactly 12
lowercase hexadecimal characters: the first 12 characters of the SHA-256 over
the UTF-8 bytes after the second `---\n` delimiter, with no trimming or newline
normalization. Parsers must read `metadata.version`, not a top-level `version`
field. This repo is a published mirror; it is not edited by hand.

## Channels and updates

Two channels serve the same generated markdown, but they pick up a new
version differently:

- **`skills` CLI** — resolves `cohesivity-org/cohesivity-skill` at install time,
  so a merge to `main` is the release.
- **`@cohesivity/init` npm** — pins an exact commit SHA in `SKILL_PIN`, so it
  does *not* move on merge. It ships only when someone bumps the pin and
  republishes the package.

What is always in the agent's context is the skill's frontmatter
`description`; the model reads it to decide whether to load the full
`SKILL.md`. A stale mirror therefore misdescribes what Cohesivity provisions
even before the skill is opened, which is why the copies are refreshed per
release rather than opportunistically.

## Docs

<https://cohesivity.ai/llms.txt>

## Security

See [SECURITY.md](SECURITY.md) for private vulnerability reporting. Do not open
a public issue for a suspected vulnerability or include live tenant credentials
in a report.

## License

MIT. See [LICENSE](LICENSE).

## Updating this mirror (manual)

This repo is a published mirror of the skill; the canonical source lives in
`cohesivity-org/cohesivity` at `worker/src/skill/cohesivity.skill.md`. To ship a
skill change:

1. Edit the source, then regenerate: `node scripts/generate-skill.mjs`. This
   stamps a new `metadata.version` content hash and rewrites
   `skill-content.generated.js`.
   Ship it through the normal PR + deploy flow so `cohesivity.ai/skill.md` serves it.
2. Copy the generated markdown into **both** files in this repo, so every
   channel stays in lockstep:
   - `cohesivity.skill.md` (pinned by the `@cohesivity/init` npm package)
   - `skills/cohesivity/SKILL.md` (discovered by `npx skills add`)
   Commit as `skill: publish generated skill (version <hash>)` and push.
3. Bump `SKILL_PIN` in `@cohesivity/init/bin/cli.js` to this repo's new commit
   SHA, then republish the npm package.

Both files must carry the same content and `metadata.version`. There is no
automated sync yet, so this is a manual step per release.

The `skills`-CLI channel needs no further step — merging step 2 to `main` is
its release. Only npm needs step 3.

Before pushing, verify:

```bash
cmp cohesivity.skill.md skills/cohesivity/SKILL.md
curl -sS -H 'User-Agent: cohesivity-mirror-check' https://cohesivity.ai/skill.md \
  | cmp - cohesivity.skill.md
```
