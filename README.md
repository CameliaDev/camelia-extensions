# Camelia extensions

Official plugin and skill registry for [Camelia](https://github.com/CameliaDev/Camelia-Agent).

Camelia-Agent defaults to this catalog on `main`:

```
https://raw.githubusercontent.com/CameliaDev/camelia-extensions/main/catalog.json
```

`CAMELIA_CATALOG_URL` replaces that address. If the default cannot be fetched and no override is set, Camelia falls back to `extensions-registry/catalog.json` in Camelia-Agent. That fallback copy uses the same ids and points at Camelia-Agent.

## Layout

```
catalog.json
plugins/<id>/     camelia.plugin.json required
skills/<id>/      SKILL.md required
```

Each entry's `repo` is this repository. `path` is the directory inside it. Install shallow-clones the repo (`git clone --depth 1`) and copies that path. Plugins land in `~/.camelia/plugins/<id>`. Skills land in `~/.camelia/skills`. Restart Camelia after installing a plugin; the engine loads plugins when the process starts.

## Add an entry

See `skills/catalog-maintainer`. Put the folder under `plugins/` or `skills/`, append it to `catalog.json`, and commit to `main` so the raw URL above stays current.
