# Cohesivity agent skill

The agent playbook for [Cohesivity](https://cohesivity.ai): on-the-fly backend
infrastructure purpose-built for AI agents. One HTTP API provisions databases,
hosting, auth, realtime, storage, and AI model access, and the agent provisions
on the user's behalf.

## Install

With the [`skills`](https://github.com/vercel-labs/skills) CLI:

```bash
npx skills add cohesivity-org/cohesivity-skill
```

Or set up a whole project (skill + a managed tenant + config) with the npm package:

```bash
npx @cohesivity/init
```

## What's here

- `skills/cohesivity/SKILL.md` — the skill, for the `skills` CLI.
- `cohesivity.skill.md` — the same content at the path the `@cohesivity/init`
  package pins.

Both are generated from the canonical source and carry a `version:` content
hash in their frontmatter. This repo is a published mirror; it is not edited by
hand.

## Docs

<https://cohesivity.ai/llms.txt>

## Updating this mirror (manual)

This repo is a published mirror of the skill; the canonical source lives in
`cohesivity-org/cohesivity` at `worker/src/skill/cohesivity.skill.md`. To ship a
skill change:

1. Edit the source, then regenerate: `node scripts/generate-skill.mjs`. This
   stamps a new `version:` content hash and rewrites `skill-content.generated.js`.
   Ship it through the normal PR + deploy flow so `cohesivity.ai/skill.md` serves it.
2. Copy the generated markdown into **both** files in this repo, so the npm and
   `skills`-CLI channels stay in lockstep:
   - `cohesivity.skill.md` (pinned by the `@cohesivity/init` npm package)
   - `skills/cohesivity/SKILL.md` (discovered by `npx skills add`)
   Commit as `skill: publish generated skill (version <hash>)` and push.
3. Bump `SKILL_PIN` in `@cohesivity/init/bin/cli.js` to this repo's new commit
   SHA, then republish the npm package.

Both files must carry the same content and `version:`. There is no automated
sync yet, so this is a manual step per release.
