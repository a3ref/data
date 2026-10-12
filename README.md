# a3ref/data — Arma 3 scripting dataset for AI agents and tools

**Every Arma 3 scripting command (≈2,650) and function (≈2,050) from the
[Bohemia Interactive Community Wiki](https://community.bistudio.com/wiki) as structured JSON, synced daily.**

Built first for AI agents, bots and programs: the original wiki blocks automated access behind a bot challenge,
so this repository keeps a clean, versioned, machine-readable copy that anything can fetch with a plain HTTP GET.
No API key, no rate limit, CORS open. The readable site with per-page Markdown and HTML is **https://a3ref.github.io/**.

## Files

| File | Contents |
|---|---|
| `index.json` | start here: every entry with name, type, syntax lines, one-line summary, groups, locality and its page paths on the site |
| `commands.json` | every scripting command, fully parsed: syntaxes, typed parameters, return values, examples, locality, versions, community notes |
| `functions.json` | every `BIS_fnc_*` and other function, same format |
| `schema.json` | JSON Schema 2020-12 of a record (`$defs/bundle` for the two files above, `$defs/index` for `index.json`) |
| `raw/commands.json`, `raw/functions.json` | source wikitext of each page with its revision id, keyed by page id |

Each of the top three files is `{"updated": ..., "count": ..., "items": [...]}` with one item per line, so diffs stay
readable. `updated` is the time of the newest wiki edit in the data, not the build time: a file only changes when the
wiki does, or when the parser improves.

## Fetch it

```sh
curl -sL https://raw.githubusercontent.com/a3ref/data/main/index.json
curl -sL https://cdn.jsdelivr.net/gh/a3ref/data@main/commands.json
```

```python
import json, urllib.request

url = "https://raw.githubusercontent.com/a3ref/data/main/commands.json"
commands = {c["name"].lower(): c for c in json.load(urllib.request.urlopen(url))["items"]}
print(commands["setpos"]["syntaxes"][0]["syntax"])   # object setPos pos
```

Single pages, already split, live on the site: `https://a3ref.github.io/commands/setPos.json`,
`https://a3ref.github.io/functions/BIS_fnc_spawnGroup.md`. See https://a3ref.github.io/llms.txt for the full guide
written for language models.

### Give it to your agent

```markdown
## Arma 3 / SQF reference
When writing or reviewing SQF for Arma 3, check commands and BIS_fnc_* functions against
https://a3ref.github.io/ instead of relying on memory:
- https://a3ref.github.io/commands/<command>.md (e.g. commands/setPos.md)
- https://a3ref.github.io/functions/<BIS_fnc_name>.md (e.g. functions/BIS_fnc_spawnGroup.md)
- https://a3ref.github.io/index.json lists every name with its syntax and a one-line summary
Names are case-sensitive and use the wiki spelling.
```

## History as a changelog

`.github/workflows/sync.yml` runs daily, using the code from
[a3ref/a3ref.github.io](https://github.com/a3ref/a3ref.github.io): it pulls the pages edited since the last run,
re-parses them and commits only if something changed. The commit log therefore doubles as a changelog of the wiki's
Arma 3 scripting pages; `git log -p -- commands.json` shows which commands changed and how.

The record format is documented in [`schema.json`](schema.json) and in the
[site repository README](https://github.com/a3ref/a3ref.github.io#json-record).

## Source and attribution

All content comes from the authors of the BI Community Wiki. Every record keeps its `url` and `revid`,
so it can be traced back to the exact wiki revision.
