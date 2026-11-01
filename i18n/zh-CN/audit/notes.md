# zh-CN overlay retest (audit=1db772e)

Site data for `http://127.0.0.1:19999/` was cleared in Chrome DevTools Storage with cache storage, IndexedDB, local/session storage, cookies, and service-worker unregister selected. No welcome/sign-in splash appeared.

## Metrics (`metrics.png`)

The in-scope navigation and metrics-panel chrome is Chinese. The chart toolbar now shows `的` next to `平均值` (the old English token is fixed). The count still renders as `1 system` rather than `1 个系统`; `system` is the remaining English in the toolbar. The visible timestamp/date strings still use English month/day formatting (for example, `Sun, Oct 04, 2026`), and the global time UI retains technical `GMT+8`/`15min` formatting. Chart titles, metric names, units, and data values are intentionally not inventoried.

## Alerts (`alerts.png`)

The Raised/Running/Configuration Errors/New Alert area is translated as `已触发`, `运行中`, `配置错误`, and `新建告警`. The empty state is Chinese: `此空间没有活动告警。` and `稍后再看，或检查通知`. The remaining English visible in this view is the date portion of the last-updated timestamp, `Sun, Oct 04, 2026 15:33:18` (plus the technical `GMT+8`/`15min` global time formatting).

## Configurations (`configs.png`)

The configuration chrome is Chinese, including `配置`, `采集器`, `健康`, `日志`, and the requested subtabs: `密钥存储`, `服务发现`, `虚拟节点`. Collector/module identifiers such as `go.d`, `scripts.d`, `activemq`, and `apache` remain identifier names rather than UI strings and are not counted as untranslated chrome.

---

# Merged from the uncommitted local patch (2026-10-04 titles audit)

Source: the working-tree `i18n/zh-CN/audit/notes.md` in the old box patch (`/workspace/git-patches/netdata/uncommitted.diff.patch`). Only this file was taken. The same patch deleted upstream submodules and `src/` files, and that part was discarded. The full text is identical to `NOTES-titles.md`. The useful points are:

- **Overview cards** (`audit=titles1`): `Total Disk Reads` → 磁盘读取总量, `Avg CPU per Node` → 每节点平均 CPU, `Avg Used RAM per Node` → 每节点平均已用内存, `Total Network Inbound/Outbound` → 网络入站/出站总量, `Average Disk I/O Pressure` → 平均磁盘 I/O 压力（10 秒，部分）, `Top Nodes by CPU / Used RAM` → 按 CPU / 已用内存排名的节点, `Avg System Load` → 平均系统负载（1 分钟）. Still correct on 2026-11-01.
- **Collector list convention:** bilingual `Product 类别` labels (e.g. `ActiveMQ 消息队列`, `Redis 缓存`). The Docker child instance shows as `本地`. The 2026-10-04 list of bare English collectors (`Adaptec RAID` … `ZooKeeper`) was the backlog for this round. 84 of them now have bilingual labels (see `UNTRANSLATED.md` for the few kept bare).
- **Correction (2026-11-01):** that note said `O/S Services`, `Containers & VMs`, and `Cgroups` were "not chart-title failures". They were actually TOC headings that failed because of the `-- ` depth prefix. `overlay.js` now handles the prefix.
