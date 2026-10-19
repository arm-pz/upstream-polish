# upstream-polish

**Fix listing drift and polish descriptions after upstream changes.**

When your published listings (marketplace, site index, READMEs) no longer match your repos, or after you've pushed skill updates and need to sync the public surfaces, upstream-polish diagnoses the drift and generates polished fixes. It runs Upstream-Check to audit push readiness and listing mismatches, then passes the findings to Wordsmith to generate improved descriptions, topics, README openings, and skill page copy that match reality.

## Usage

```text
/upstream-polish --root ~/Downloads/Qoder
/upstream-polish --owner arm-pz --site https://skills.caffeinity.com
Check what hasn't been pushed and fix the listings.
```

## When to use

- Your marketplace says 19 skills but your repos ship 12
- You pushed skill updates and need to sync public descriptions
- Repo descriptions no longer match what the skills actually do
- Someone asks "why do your site and README list different numbers?"
- You want to clean up skill boundaries after a batch push

## What it produces

- An upstream audit from Upstream-Check (push status, credential screening)
- A drift report comparing published sources against ground truth
- Polished fixes from Wordsmith for descriptions, topics, READMEs
- Skill page copy that matches current reality
- Boundary clarifications where skills overlap after changes

## Install

```bash
npx skills add arm-pz/upstream-polish
```

## Dependencies

Requires `upstream-check` and `wordsmith` skills to be installed.

## Not for

- Creating skills from scratch → use [draft-skill](https://github.com/arm-pz/draft-skill)
- Just tuning trigger language → use [tune-skill](https://github.com/arm-pz/tune-skill)
- Making a repo discoverable initially → use [findable](https://github.com/arm-pz/findable)

## Related

- **[upstream-check](https://github.com/arm-pz/upstream-check)** — Audit drift and push safety (phase 1)
- **[wordsmith](https://github.com/arm-pz/wordsmith)** — Generate polished alternatives (phase 2)
- **[findable](https://github.com/arm-pz/findable)** — Initial discoverability setup

## License

MIT © Abdul Rahman P Z
