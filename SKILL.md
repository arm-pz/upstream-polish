---
name: upstream-polish
description: Audit unpushed work with upstream-check and listing mismatches with listing-drift, then generate polished fixes with Wordsmith. Use when the user wants to reconcile repo state with published listings (marketplace, site index, README), fix descriptions or topics that drifted, or clean up skill boundaries after a push — and wants both the diagnosis and concrete rewrites. Chains upstream-check (push state) → listing-drift (listings) → wordsmith (alternatives) in one workflow. Not for creating skills from scratch (use draft-skill) or tuning trigger language only (use tune-skill).
version: 1.0.0
argument-hint: "[--root <dir>] [--owner <github-user>] [--site <url>]"
user-invocable: true
metadata:
  version: "1.0.0"
---

# Upstream-Polish

Chain three existing skills into a single workflow: **Upstream-Check** audits repos for push readiness, **Listing-Drift** checks the published listings against the repos, then **Wordsmith** generates polished fixes for descriptions, topics, README openings, and skill page copy that no longer match reality.

## Invocation

```text
/upstream-polish --root ~/Downloads/Qoder
/upstream-polish --owner arm-pz --site https://skillsmith.tech
Check what hasn't been pushed and fix the listings.
```

If no arguments are provided, ask the user for the root directory of their clones and/or the GitHub owner + site URL.

## Workflow

### Phase 1: Run the Audits

Invoke the **Upstream-Check skill** with the root directory:

1. Scan the folder of git clones, fetch each, classify as push-ready/level/behind/diverged/no-upstream/no-remote.
2. Screen outgoing diffs for credential-shaped lines; abort on hits unless `--allow-secret-hits`.

Then invoke the **Listing-Drift skill** with the owner and site URL:

3. Compare ground truth (repos with SKILL.md) against published sources (site index, homepage, llms.txt, marketplace manifest, READMEs), judge every typed count, and flag links to a retired host.
4. Output: a structured report with the verdict table from upstream-check + drift findings from listing-drift (absent repos, not-in-ground-truth entries, two indexes disagreeing, stale counts).

**Handoff rule:** Capture both reports verbatim. Do not summarize yet. Run listing-drift after any push, so it reads what is now on GitHub.

### Phase 2: Generate Alternatives with Wordsmith

Pass listing-drift's findings to the **Wordsmith skill** with this instruction:

```
For each listing drift finding identified below, generate 2–3 alternative versions
that fix the mismatch while preserving factual accuracy. Focus on:

- GitHub About text and topics for repos whose descriptions drifted
- README opening lines that no longer match the skill's actual scope
- Site index entries or marketplace manifest lines that list wrong counts
- Skill page copy that references outdated features or commands

Original audit:
[Paste listing-drift's full report here]

Target repos/listings:
[List the specific repos or listing entries flagged]
```

Wordsmith should:
- Treat each drift finding as a separate rewrite task
- Provide 2–3 options per finding (labeled Option A/B/C)
- Keep facts aligned with the current SKILL.md and repo state
- Highlight which option best resolves each drift issue

### Phase 3: Present Results

Return a three-part response:

1. **Audit Summary** (upstream-check's push-ready repos first, then listing-drift's findings by severity)
2. **Wordsmith Alternatives** (grouped by drift issue, with options labeled)
3. **Recommended Actions** (which combination of alternatives + push commands to run, in 2–3 sentences)

Format the alternatives so the user can copy-paste directly into their files or run the suggested `gh repo edit` commands.

## Example

```
User: /upstream-polish --owner arm-pz --site https://skillsmith.tech

Assistant:

## Upstream-Check Verdicts

**Push Status:**
- findable-polish: push-ready (3 commits ahead)
- keyword-resolver: level
- seo-audit: behind (pull needed)

**Listing Drift:**
1. **Site index lists 15 skills, ground truth has 17** — absent: findable-polish, keyword-resolver. Severity: High
2. **Marketplace manifest lists "release-gate" but repo is "release-gate-chair"** — not in ground truth. Severity: Medium
3. **findable's README says "12 skills" but 17 exist** — stale count. Severity: Low

## Wordsmith Alternatives

### Issue 1: Site index missing 2 skills

**Option A:** Add entries for findable-polish and keyword-resolver with their current SKILL.md descriptions.
**Option B:** Regenerate the entire index from ground truth (all 17 repos with SKILL.md) to prevent future drift.
**Option C:** Add the two missing entries now + set up an automated sync script to rebuild the index on every push.

→ Best: Option C (immediate fix + prevents recurrence)

[... continues for each issue ...]

## Recommended Actions

1. Add the two missing entries at the source the site index is generated from, then re-run listing-drift to confirm the index now matches.
2. Update findable's README count from 12 to 17 using Option A wording.
3. Push findable-polish: `cd ~/arm-pz/findable-polish && git push origin main`.
4. Pull seo-audit before pushing anything there.
```

## Boundaries

- **Does not** push diverged repos or use `--force` — it reports them and stops.
- **Does not** create new skills or tune trigger language (use draft-skill or tune-skill).
- **Does not** execute `gh repo edit` or push changes — it drafts the fixes and hands over commands for the user to run.
- **Does not** screen for secrets on its own — relies on upstream-check's built-in credential scanner.

## Dependencies

This skill requires `upstream-check`, `listing-drift` and `wordsmith` to be installed. If any is missing, tell the user which and stop.
