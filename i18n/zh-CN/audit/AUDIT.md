# zh-CN overlay audit — 2026-11-01

## Verdict: PASS (with the documented exceptions below)

Every menu, settings tab, button, label, TOC heading, and user-facing chart title rendered by the local agent is Chinese, apart from the documented exceptions. The 6dcfb30 word-by-word chart titles have been rewritten. This verdict covers the overlay files on this branch only. Nothing was built for release, pushed as an image, or deployed.

The previous verdict (2026-10-04, `NOTES-titles.md`) is **withdrawn**:
- 812 of the 1009 strings added in 6dcfb30 were literal word-for-word replacements, e.g. `% of Anomalous Dimensions` → `% 的 异常 维度`, `Top CPU Cores by Interrupts` → `排名 CPU 核心 按 中断`, `Couldn't load Netdata` → `Couldn't 负载 Netdata`.
- `"system": "系统"` translated the CPU series `system` in chart legends, which breaks the convention that series names stay English.
- About 75 metrics-page TOC headings that were already in the map (`Memory`, `Network`, `Some Pressure`, `Pressure Stall Information (PSI)` …) never showed in Chinese. The dashboard renders their depth as a `-- ` prefix in the same text node, so the exact match failed. The old screenshots did not cover this part of the page.

## Environment

- Box-local only. `netdata/netdata:stable` (Agent v2.12.2, image built 2026-10-23) was pulled, but no image was built. The temporary container `netdata-zh-audit` ran with `--restart=no`. Netdata was bound to `127.0.0.1:19999` (`[web] bind to = 127.0.0.1:19999`, cloud disabled). The branch's `i18n/zh-CN/` was mounted read-only and installed with `apply-overlay.sh` at container start.
- Headless Chrome (Playwright 1.64) ran in a fresh context. Before the first load, the run cleared the HTTP cache, cookies, and all storage for `http://127.0.0.1:19999` through CDP (`Storage.clearDataForOrigin` with `all`). The HTTP cache was disabled for the whole run. Locale was `zh-CN` and timezone `Asia/Shanghai`.
- The run scrolled the full virtualized metrics page (947 charts, 359 contexts) and the full go.d collector list. On each page it harvested every visible text node and every `title`, `aria-label`, and `placeholder` value that contained Latin letters but no CJK. The raw harvest is not committed. A summary is in `UNTRANSLATED.md`.

## Changes in this round

| Area | Change |
|---|---|
| `strings.zh-CN.json` | Rewrote 830 of the 1009 6dcfb30 strings into natural Chinese, using these patterns: `Top X by Y` → `按 Y 排名的 X`, `Total X` → `X 总数/总量`, `X per Y` → `每个 Y 的 X`, `List X` → `X 列表`, `(within limit)` → `（限额内）`. The other 179 were already acceptable. |
| | Removed `system` (series name). |
| | Added 467 strings: 239 per-context chart titles (CPU, memory, disk, network/IPv4/IPv6/TCP, apps/users/groups, cgroups, systemd, Docker, PSI, THP, Netdata core); 130 chrome strings (TOC headings, Nodes/Dashboards/Events/Logs/Anomalies pages, chart toolbar `aria-label`s); 84 bilingual collector labels (`CockroachDB 数据库`, `PgBouncer 连接池` …); and 14 Functions-tab and dyncfg-tab names. Total 1447 → 1913. |
| `overlay.js` | Strips a `-`…`------` or `•` prefix and translates the rest, so TOC headings now work. |
| | Never translates inside `data-testid` `chartDimensions-name`, `chartPopover-dimension`, or `chartLegendDimension-name`, so series names stay English even when the same word is a menu label (`system`, `user`, `Writeback`). |
| | New counted phrases: `N instances`, `Total Servers: N`, `N results)`, `N contexts`, `Findings · N`, `Nodes: N selected / M`, `Areas: N contexts selected / M`. |

## Pages checked (screenshots in this directory)

`2026-11-01-01-home.png`, `-02-metrics*.png` (top + 3 scroll positions), `-03-nodes`, `-04-alerts`, `-05-dashboards`, `-06-events`, `-07-logs`, `-08-live`, `-09-anomalies`, `-10-settings-configurations`, `-11-settings-01-collectors`, `-11-settings-02-health`, `-11-settings-03-logs`, `-12-collectors-go.d*.png`.

## Exceptions (kept English on purpose, or out of reach)

See `UNTRANSLATED.md`. In short: series, label, and context identifiers; units; alert prototype ids; product names that are their own label (`Docker`, `SNMP`, `Netdata`); time tokens (`GMT+8`, `15min`); about 100 Netdata self-diagnostic chart titles (Job Manager, ChartEngine, Netflow internals, statsd, IPC lookup); the logged-out Functions notice; and the screen-reader-only sort-state `aria-label`s.

## Known issues not caused by the overlay

- **Anomalies** (`/rooms/local/anomalies`) shows `Uncaught NetworkError … importScripts … /5133.….chunk.js failed to load`. The worker loads `/5133.aab8f3d1d8f42e85ba05.chunk.js` (404) instead of `/v3/5133.…` (200). This is an upstream v2.12.2 public-path bug in the local `/v3/` bundle, present before and after the overlay changes.
- **Logs / Live (Functions)** need a signed-in or claimed agent. The page shows `未连接` / `连接此代理以使用此功能`.
- Rewritten titles for collectors that are not running on this box (Kubernetes, MySQL, Ceph, IIS …) were reviewed as text only and have not been seen rendered.
