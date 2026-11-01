# Netdata Agent Web UI — zh-CN locale overlay

This directory is a **runtime locale overlay** for the Netdata Agent Web UI. It translates visible **menus, settings, buttons, and labels** into Simplified Chinese (简体中文).

It does **not** translate:

- metric names, chart titles that are collector/dimension identifiers, or chart tick labels that are numbers and units
- log lines or log message text
- API payloads

## Install

Copy this directory into the installed web root and inject the overlay script:

```bash
sudo ./apply-overlay.sh /usr/share/netdata/web
```

`WEB_DEST` defaults to `/usr/share/netdata/web` when no argument is given. The script:

1. Copies `i18n/zh-CN/` to `${WEB_DEST}/i18n/zh-CN/`
2. Injects `<script src="/i18n/zh-CN/overlay.js"></script>` into these files when they exist and are not already patched:
   - `${WEB_DEST}/index.html`
   - `${WEB_DEST}/v3/index.html`
   - `${WEB_DEST}/v3/agent.html`

The Agent dashboard is not vendored in this repository. It is fetched at build/install time and installed under `WEB_DEST` (see `PATCH_LANDING.md`).

## Test

Prefer the **local** `/v3/` bundle:

- `http://<agent>:19999/v3/`

The live Netdata Cloud UI (`app.netdata.cloud`) will not load this overlay. Hard-refresh after applying so `index.html` and `overlay.js` are not served from cache.

## How it works

`overlay.js` fetches `/i18n/zh-CN/strings.zh-CN.json` (with a same-directory fallback), walks text nodes under `document.body`, and replaces **exact** trimmed English strings that appear as keys in the map. It skips `SCRIPT`, `STYLE`, `TEXTAREA`, and `INPUT`, and any element with `data-i18n-skip` (including ancestors). A `MutationObserver` covers UI rendered after load. Strings that look like numbers or units only are left alone so chart ticks stay intact.

Add or correct entries in `strings.zh-CN.json`. Keys must match the English label exactly, including capitalization.

Metrics TOC headings are matched after stripping their `-`/`•` depth prefix. Chart series names (`data-testid="chartDimensions-name"` and the popover/legend equivalents) are never translated, so do not rely on removing a word from the map to protect a series name.

Mark a subtree to leave untranslated:

```html
<div data-i18n-skip>...</div>
```
