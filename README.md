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
