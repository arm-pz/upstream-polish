# upstream-polish

Audit upstream drift and listing mismatches with upstream-check, then generate polished fixes with Wordsmith. Use when the user wants to reconcile repo state with published listings (marketplace, site index, README), fix descriptions or topics that drifted, or clean up skill boundaries after a push — and wants both the diagnosis and concrete rewrites. Chains upstream-check (audit) → wordsmith (alternatives) in one workflow.

## Usage

```text
/upstream-polish --root ~/Downloads/Qoder
/upstream-polish --owner arm-pz --site https://skills.caffeinity.com
Check what hasn't been pushed and fix the listings.
```

## Workflow

1. **Upstream-Check** audits repos for push readiness and listing drift
2. **Wordsmith** generates polished fixes for descriptions/topics/READMEs
3. Returns verdict table + alternatives + recommended actions

## Dependencies

Requires `upstream-check` and `wordsmith` skills to be installed.

## License

MIT © Abdul Rahman P Z
