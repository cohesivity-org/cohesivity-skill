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

As a managed Claude Code plugin:

```text
/plugin marketplace add cohesivity-org/cohesivity-skill
/plugin install cohesivity@cohesivity
```

The install id is `<plugin>@<marketplace>`. Both halves are `cohesivity`
because this repo is the marketplace and the only plugin in it: the first
command reads `.claude-plugin/marketplace.json`, whose single `plugins[]` entry
has `"source": "./"`, and the second installs that entry.

Or set up a whole project (skill + a managed tenant + config) with the npm package:

```bash
npx @cohesivity/init
```

## What's here

- `skills/cohesivity/SKILL.md` — the skill, for the `skills` CLI.
- `cohesivity.skill.md` — the same content at the path the `@cohesivity/init`
  package pins.
- `.claude-plugin/` — the native Claude Code plugin and single-plugin
  marketplace manifests. `plugin.json` declares the plugin and points
  `"skills": "./skills"` at the skill file above, so the plugin serves the same
  bytes as the other channels. `marketplace.json` is the catalog that lists it.

The two skill files are generated from the canonical source and carry a
`version:` content hash in their frontmatter. This repo is a published mirror;
it is not edited by hand.

## Channels and updates

Three channels serve the same generated markdown, but they pick up a new
version differently:

- **Claude Code plugin** — tracks this repo. `claude plugin marketplace update`
  re-reads the source, so a merge to `main` is the release.
- **`skills` CLI** — resolves `cohesivity-org/cohesivity-skill` at install time,
  so a merge to `main` is likewise the release.
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
   stamps a new `version:` content hash and rewrites `skill-content.generated.js`.
   Ship it through the normal PR + deploy flow so `cohesivity.ai/skill.md` serves it.
2. Copy the generated markdown into **both** files in this repo, so every
   channel stays in lockstep:
   - `cohesivity.skill.md` (pinned by the `@cohesivity/init` npm package)
   - `skills/cohesivity/SKILL.md` (discovered by `npx skills add`, and served
     by the Claude Code plugin via `plugin.json`'s `"skills": "./skills"`)
   Commit as `skill: publish generated skill (version <hash>)` and push.
3. Bump `SKILL_PIN` in `@cohesivity/init/bin/cli.js` to this repo's new commit
   SHA, then republish the npm package.

Both files must carry the same content and `version:`. There is no automated
sync yet, so this is a manual step per release.

The `skills`-CLI channel needs no further step — merging step 2 to `main` is
its release. Only npm needs step 3.

For the plugin channel, also bump `version` in `.claude-plugin/plugin.json`.
It is the plugin's own version rather than the skill's content hash, but Claude
Code caches an installed plugin under a version-keyed path
(`~/.claude/plugins/cache/<marketplace>/<plugin>/<version>/`) and records both
that version and the source `gitCommitSha`. Bumping it makes the refresh
unambiguous for already-installed users.

Before pushing, verify:

```bash
cmp cohesivity.skill.md skills/cohesivity/SKILL.md
curl -sS -H 'User-Agent: cohesivity-mirror-check' https://cohesivity.ai/skill.md \
  | cmp - cohesivity.skill.md
claude plugin validate . --strict
```
