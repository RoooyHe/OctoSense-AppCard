# Working on Pantry Steward

This is a non-system script app, migrated to the unmodified official OctoSense.
Keep the id pantry-steward; do not pack it as an os.* system app or patch the
shell to provide this app with additional privileges.

- Application behaviour lives in bundle/main.splash. Only bundle/ is submitted.
- No secrets, login or secret-input fields in the bundle. AI credentials are
  entered only on the host-owned AI providers panel. Never copy ai.env.
- The official model.budget reports usage, not provider readiness.
- Speech recognition is unavailable in this tested host. Do not silently
  restore custom microphone calls or claim the icon records audio.
- Use tools/launch.py --check and tools/test_launch.py. Native UI regression:
  tools/smoke.py in a task-owned hidden official shell with separate --app-data.
  Never reuse a person's normal inventory for testing.
- --prepare-local-test generates local test keys outside the repository. Obtain
  explicit human approval first; never use these as the production identity.
- Official CLI build copies stay under target and use the host's pinned
  .sources. Never modify cached upstream sources or clone duplicate frameworks.
- Restamp after bundle changes. Keep real screenshots and bilingual READMEs
  honest. Record what was tested and what remains unverified in VALIDATION.md.
- Formal identity, privacy, signing and submission are human checkpoints.

## Agent skills

### Issue tracker

Issues and specs are local markdown under `.scratch/<feature-slug>/`; GitHub Issues are disabled on this repo. See `docs/agents/issue-tracker.md`.

### Triage labels

The five canonical triage roles, label strings equal to their names, recorded as the `Status:` line of an issue file. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one root `GLOSSARY.md` plus `docs/adr/`. See `docs/agents/domain.md`.
