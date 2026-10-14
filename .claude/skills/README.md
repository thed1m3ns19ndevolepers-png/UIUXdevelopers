# Installed skills

Project-local Claude skills for this repo. Anything in `.claude/skills/<name>/SKILL.md`
is picked up automatically, so these are available as plain `/replica-recon`,
`/replica-build` and so on, with no plugin namespace prefix.

## replica-skill

Eleven skills that clone an app: reverse-engineer it, rebuild it, test it for bugs,
then fix what its users hate.

- Source: https://github.com/Jakeschincariol/replica-skill
- Version: 1.0.0
- Commit: `77c9436fb3d18c3d58169efb8caf4fe906b0dc51` (2026-10-03)
- Installed: 2026-10-14
- Licence: MIT, Jake Schincariol. See `LICENSE`.

Installed verbatim from upstream. The only thing left out is upstream's own
packaging (`README.md`, `.claude-plugin/`, `tests/`), which a skills directory
does not use.

| command | what it does |
| --- | --- |
| `/replica-recon` | Reverse-engineers an app: screens, flows, components, data model. |
| `/replica-architect` | Plans the stack, database schema and API. |
| `/replica-design` | Rebuilds the design system as tokens, with your own assets. |
| `/replica-build` | Rebuilds the app screen by screen from the recon map. |
| `/replica-backend` | Auth, database, payments and integrations. |
| `/replica-test` | Clicks through every flow and tests it for bugs. |
| `/replica-diff` | Scores your clone against the original, and what is missing. |
| `/replica-entrepreneur` | Reads real reviews, turns complaints into fixes and an angle. |
| `/replica-brand` | Names and rebrands your version so it is yours. |
| `/replica-launch` | Landing page, pricing and store listing. |
| `/replica-deploy` | Ships it live on your domain. |

Run them in order; each reads what the last one wrote into a `replica/` folder at
the repo root. Any one also runs on its own.

```
recon -> architect -> design -> build -> backend -> test ->
diff -> entrepreneur -> brand -> launch -> deploy
```

### Tools

Six standard-library Python tools, no pip install, nothing touches the network.
They need Python 3.8+ and use paths relative to their own folder, so keep the
eleven folders as siblings. From the repo root:

```
python3 .claude/skills/replica-diff/imgdiff.py original.png clone.png --out diff.png
python3 .claude/skills/replica-diff/parity.py replica/features.csv
python3 .claude/skills/replica-entrepreneur/reviews.py replica/reviews.csv
python3 .claude/skills/replica-design/contrast.py replica/design/tokens.json
python3 .claude/skills/replica-brand/sweep.py . --avoid "Original App"
python3 .claude/skills/replica-launch/listing.py replica/launch/listing.json
```

Verified on install with Python 3.11: upstream's 57-test suite run against this
installed tree, 55 pass. The 2 that do not are upstream's own repo-packaging
assertions (that a root `README.md` and `.claude-plugin/plugin.json` exist), which
do not apply to a skills-directory install. Every tool test passes, as do
upstream's checks that exactly eleven skill folders are present and that every
`SKILL.md` has valid frontmatter.

### Scope

It rebuilds what an app does, never what it owns: features and flows, clean-room.
Not its code, logo, copy or content. It reads only public pages and your own
account, and always rebrands before launch. See upstream's README for the full
fine print.
