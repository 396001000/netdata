# Untranslated inventory — 2026-11-01

Source: full harvest of visible text and `title`/`aria-label`/`placeholder` on the pages listed in `AUDIT.md`, local agent v2.12.2, after this round's fixes.

## In scope but held back (deliberate)

| Text | Where | Why |
|---|---|---|
| `system` in `1 system` | chart toolbar | `system` is its own text node, and the bare word is also the CPU series name. Series names stay English. |
| `GMT+8`, `15min`, `15 min` | time picker / anomalies toolbar | time tokens |
| `Docker`, `SNMP`, `Netdata`, `Netflow`, `Snmp`, `nvme`, `sql`, `tor`, `WireGuard VPN` | TOC, collector list, Functions list | bare product names. Mapping the bare word would also rename TOC sections and Functions tabs. |
| `alerts`, `prototype`, `10min_cpu_usage` … | Settings → 健康 | config ids from dyncfg |
| `Address, sortable, not sorted` etc. | table header `aria-label` | screen-reader text built from a template. Not visible. |
| `Functions expose sensitive information … logged-in users and claimed agents …` | Logs / Live, logged out | long paragraph split across several nodes. The heading above it (`未连接` / `连接此代理以使用此功能`) is translated. |
| `Uncaught NetworkError … importScripts …` | Anomalies | upstream error text (see AUDIT.md) |
| `areas` in `无匹配 areas.` | Anomalies empty state | interpolated token |
| `KS2` | Anomalies toolbar | algorithm name |

## Netdata self-diagnostic chart titles (about 100, kept English)

These charts report the agent's own internals. Their titles embed snake_case counter names, so a translation would be half English anyway:

- `Current/Cumulative/Age of the Job Manager <counter>` (go.d / scripts.d jobmgr)
- ChartEngine: `BuildPlan …`, `Build phase duration in seconds`, `Route cache …`, `Series … by planner/routing`, `Planner actions by kind`, `Chart instances / Inferred dimensions in the latest successful build rollup`, `1 when build sequence monotonicity is currently violated`
- `Netflow …` decoder/journal/tier internals, `Cisco NSEL …`, `sFlow Samples`
- `… netdata statsd server/database`, `statsd server TCP …`
- `Apps Plugin APPS_LOOKUP/CGROUPS_LOOKUP …`, `Network Viewer APPS_LOOKUP …`, `CGROUPS_LOOKUP Server …`, `Apps Plugin CPU/Files`
- `SNMP topology internal …`

## Out of scope (convention)

Series/dimension names (`user`, `system`, `softirq`, `iowait`, `received`, `sent`, `order0`…), context and chart ids (`system.cpu`, `mem.balloon`, …), label keys (`Label:cgroup_name`, `app_group`, `service_name`), aggregation functions (`SUM()`, `MIN()`, `MAX()`, `LATEST()`), units (`bits per second`, `KiB/s`, `threads`, `percent`), host names, log bodies.

Dates follow the browser locale. With a `zh-CN` browser they render as `2026年11月01日周日`. With an English browser they stay `Sun, Nov 01, 2026`. This is not an overlay string.

## Verified translated this round (examples)

TOC: `计算`, `压力停滞信息（PSI）`, `部分压力`, `完全压力`, `内存气球`, `碎片`, `回写`, `进程间通信（IPC）`, `互联网控制消息协议（ICMP）`.
Chart titles: `CPU 使用率（100% = 1 核）`, `磁盘 I/O 带宽`, `IPv4 TCP 握手问题`, `透明大页内存`, `应用组内存 RSS 用量`, `systemd 服务内存`, `内存部分压力停滞时间`.
Chrome: `服务器总数：1`, `在线：`, `过期：`, `离线：`, `分组方式：节点状态`, `默认视图`, `集群地图`, `播放列表`, `最后修改`, `无数据可显示`, `节点异常随时间变化`, `受影响节点`, `影响范围分析`, `N 个实例`.
Collectors: `CockroachDB 数据库`, `PgBouncer 连接池`, `Pi-hole 广告拦截`, `NVIDIA 显卡`, `PAN-OS 防火墙` … (84 added; total bilingual about 130).
