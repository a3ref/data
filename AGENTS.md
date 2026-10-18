# AGENTS.md

Instructions for AI agents working in this repository. File formats and download URLs are in
[README.md](README.md); the full record format is [schema.json](schema.json).

**a3ref/data** is generated data: every Arma 3 scripting command, function and core concept page of the BI Community
Wiki as JSON. A workflow rewrites it twice a day with the code from
[a3ref/a3ref.github.io](https://github.com/a3ref/a3ref.github.io). Nothing here is edited by hand except `README.md`,
`AGENTS.md`, `CLAUDE.md`, `.gitattributes` and `.github/`.

## Reading the data without wasting context

The bundles are large (`commands.json` 7 MB, `functions.json` 4 MB, `index.json` 2 MB, `topics.json` 1 MB,
`syntax.json` 0.5 MB, `raw/` 7 MB). Never read a whole bundle. Every record sits on its own line, so one `grep` returns
one complete record:

```sh
grep -m1 '^{"name":"setPos"' commands.json            # full record of one command (~5 KB)
grep -m1 '"name":"setPos"' index.json                 # its index entry: syntax, summary, groups, locality, status
grep -i '"name":"[^"]*spawngroup' index.json | cut -c1-300   # find by part of the name
grep -m1 '^{"name":"Position"' topics.json            # a concept page as Markdown with sections
```

Names are as written in scripts (`setPos`, `diag_fps`, `BIS_fnc_spawnGroup`); operators are named like their wiki
page (`a != b`) and have an `operator` field (`!=`). For one page as readable Markdown, use the site instead:
`https://a3ref.github.io/commands/setPos.md`.

With Python or `jq`, load `index.json` first and only the records you need:

```sh
jq -c '.items[] | select(.status == "deprecated") | {name, syntax}' index.json
```

## Changing something

- **Wrong or missing content in a record**: the parser is in [a3ref/a3ref.github.io](https://github.com/a3ref/a3ref.github.io)
  (`biwiki/parse.py`, `biwiki/wikitext.py`); fix it there. Its AGENTS.md says how to rebuild these files.
- **Wrong text on the wiki**: fix it on https://community.bistudio.com/wiki; the next sync picks it up.
- **Sync schedule or steps**: `.github/workflows/sync.yml`.
- In Claude Code a hook blocks edits to the generated files.

English only in everything committed. Commit messages are written by the sync workflow (`Sync from BI wiki <date>`);
manual commits use one plain sentence. Pushing to `main` redeploys the site; get the maintainer's consent first.
