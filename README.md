# a3ref/data — Arma 3 scripting reference dataset

Every Arma 3 scripting command and function from the
[Bohemia Interactive Community Wiki](https://community.bistudio.com/wiki), as JSON.
Updated daily by the workflow in [a3ref/a3ref.github.io](https://github.com/a3ref/a3ref.github.io),
which also publishes the readable site at **https://a3ref.github.io/**.

The commit history doubles as a changelog of the wiki: each sync commit contains only the pages edited since the last one.

## Files

| File | Contents |
|---|---|
| `commands.json` | every scripting command, fully parsed: syntaxes, parameters, return values, examples, locality, notes |
| `functions.json` | every `BIS_fnc_*` and other function, same format |
| `index.json` | lightweight list: name, syntaxes, one-line summary, groups, locality, and the page paths on the site |
| `raw/commands.json`, `raw/functions.json` | source wikitext of each page with its revision id, keyed by page id |

Each of the top three files is `{"updated": ..., "count": ..., "items": [...]}` with one item per line,
so diffs stay readable. `updated` is the time of the newest wiki edit in the data, not the build time,
so a file only changes when the wiki does (or when the parser improves).

The record format is documented in the
[site repository README](https://github.com/a3ref/a3ref.github.io#json-record).

## Use it directly

```sh
curl -L https://raw.githubusercontent.com/a3ref/data/main/index.json
```

Or through a CDN: `https://cdn.jsdelivr.net/gh/a3ref/data@main/commands.json`.
Single pages are also available from the site, e.g. `https://a3ref.github.io/commands/setPos.json`.

## Source and attribution

All content comes from the authors of the BI Community Wiki. Every record keeps its `url` and `revid`,
so it can be traced back to the exact wiki revision.
