# Case #9: Windows Performance Optimization (8GB RAM)

**Date:** April 2026
**Environment:** Windows 11 — Version 10.0.26200.8039
**Hardware:** SSD (C:) + HDD (D:, E:) — 8GB RAM

---

## Problem
System performance not fully optimized — unnecessary visual effects, startup apps, background services, and disk space consuming resources.

---

## Goal
Maximize performance on a mid-range machine without reinstalling Windows or touching personal files.

---

## Step 1 — Disable Startup Apps

**Path:** Settings → Apps → Startup

Disabled all non-essential startup applications:
- EpicGamesLauncher → Off
- Firefox → Off
- Intel Graphics Command Center → Off
- Internet Download Manager (IDM) → Off
- Microsoft 365 Copilot → Off
- Microsoft OneDrive → Off
- Microsoft Teams (x2) → Off
- Microsoft To Do → Off
- Mobile devices → Off
- Phone Link → Off
- Power Automate Desktop → Off
- Terminal → Off

Kept On (required):
- Realtek HD Audio Universal Service → On (audio driver)
- Windows Security notification icon → On (security)

![Startup apps - top section](images/case9-performance/01-startup-apps-top.png)
![Startup apps - bottom section](images/case9-performance/02-startup-apps-bottom.png)

**Why:** Every enabled startup app delays boot time and consumes RAM from the moment Windows loads.

---

## Step 2 — Optimize Visual Effects

**Path:** `sysdm.cpl` → Advanced → Performance → Settings → Visual Effects

Selected **Custom** and enabled only:
- ✅ Show thumbnails instead of icons
- ✅ Smooth edges of screen fonts

Everything else: **disabled**

![Visual Effects - Custom with only 2 options enabled](images/case9-performance/03-visual-effects-custom.png)

**Why:** Windows animations consume CPU and GPU cycles — disabling them frees resources for actual tasks.

---

## Step 3 — Configure Power Plan

**Path:** `powercfg.cpl`

Using **My Custom Plan 1** with:
- Turn off display: 10 min (battery) / 15 min (plugged in)
- Sleep: **Never** (both battery and plugged in)

![Run - powercfg.cpl](images/case9-performance/04-run-powercfg.png)
![Custom power plan settings](images/case9-performance/05-power-plan-basic.png)

Advanced settings via **Change advanced power settings**:

| Setting | Value |
|---------|-------|
| PCI Express → Link State Power Management | Off (battery + plugged in) |
| Processor → Minimum processor state | 5% (battery) / 100% (plugged in) |
| Processor → Maximum processor state | 100% (both) |

![Power Options Advanced settings](images/case9-performance/06-power-advanced-settings.png)

---

## Step 4 — Services Review

**Path:** `services.msc` (Run as Administrator)

| Service | Status Found | Decision |
|---------|-------------|---------|
| SysMain (Superfetch) | Automatic | Left — SSD handles it fine |
| Windows Search | Automatic (Delayed Start) | Left — delayed start already optimized |
| Print Spooler | Automatic | Left — printer in use |

![Run - services.msc](images/case9-performance/07-run-services.png)
![SysMain service](images/case9-performance/08-sysmain.png)
![Windows Search - Automatic Delayed Start](images/case9-performance/09-windows-search.png)
![Print Spooler service](images/case9-performance/10-print-spooler.png)

**Note:** On SSD, SysMain causes no harm. Windows Search delayed start is already the optimized default.

---

## Step 5 — Virtual Memory Check

**Path:** `sysdm.cpl` → Advanced → Performance → Settings → Advanced → Virtual Memory

| Field | Value |
|-------|-------|
| Setting | System managed size ✅ |
| Drive | C: (SSD) |
| Currently allocated | 4352 MB |
| Recommended | 1899 MB |
| Processor scheduling | Programs ✅ |

![Performance Options - Advanced tab](images/case9-performance/11-performance-advanced.png)
![Virtual Memory - System managed](images/case9-performance/12-virtual-memory.png)

**Decision:** Left as System managed — allocating more than recommended is correct for 8GB RAM. SSD ensures fastest paging speed.

---

## Step 6 — Disable Delivery Optimization

**Path:** Settings → Windows Update → Advanced Options → Delivery Optimization

- Allow downloads from other devices: **Off**

![Delivery Optimization - Off](images/case9-performance/13-delivery-optimization.png)

**Why:** Prevents Windows from using your bandwidth to upload updates to other PCs on the internet.

---

## Step 7 — Background App Permissions

**Path:** Settings → Apps → Installed Apps → [App] → Advanced options

| App | Background Permission |
|-----|-----------------------|
| Mail and Calendar | Never |
| Microsoft Store | Never |
| Windows Security | Never |

![Mail and Calendar - background Never](images/case9-performance/14-mail-calendar-background.png)
![Microsoft Store - background Never](images/case9-performance/15-store-background.png)
![Windows Security - background Never](images/case9-performance/16-security-background.png)

---

## Step 8 — Taskbar Cleanup

**Path:** Settings → Personalization → Taskbar

| Item | Set To |
|------|--------|
| Search | Hide |
| Task View | Off |
| Widgets | Off |
| Resume | Off |

![Taskbar items cleaned up](images/case9-performance/17-taskbar-items.png)

**Why:** Widgets and Search indexing consume background RAM and CPU even when not actively used.

---

## Step 9 — Firefox Performance Tuning

### General Settings
**Path:** Firefox → Settings → General → Performance
- Use hardware acceleration when available: ✅ Enabled

![Firefox Performance settings](images/case9-performance/18-firefox-performance.png)

### about:config — Memory Cache Limit
**Path:** Firefox address bar → `about:config` → Accept risk

Set `browser.cache.memory.capacity` from `-1` (automatic) to `262144` (256 MB)

![about:config warning page](images/case9-performance/19-about-config-warning.png)
![browser.cache.memory.capacity default -1](images/case9-performance/20-cache-default.png)
![browser.cache.memory.capacity set to 262144](images/case9-performance/21-cache-set.png)

**Why:** Defines a hard memory cache limit — prevents Firefox from expanding RAM unboundedly on 8GB systems.

---

## Step 10 — Disk Cleanup

**Path:** Search → Disk Cleanup → Run as Administrator → Clean up system files

- Round 1: **1.12 GB** freed — Defender logs, upgrade logs, temp internet files
- Round 2: **698 MB** freed — Defender files, DirectX Shader Cache
- **Total freed: ~1.8 GB**

> ⚠️ System Restore / Shadow Copies cleanup intentionally skipped — restore points preserved for recovery safety.

---

## Step 11 — Drive Health Check

**Path:** Run → `dfrgui`

| Drive | Type | Status |
|-------|------|--------|
| C: | Solid State Drive | ✅ OK (0 days since last retrim) |
| D: | Hard Disk Drive | ✅ OK (0% fragmented) |
| E: | Hard Disk Drive | ✅ OK (0% fragmented) |

![Run - dfrgui](images/case9-performance/22-run-dfrgui.png)
![Optimize Drives - all healthy](images/case9-performance/23-optimize-drives.png)

**Key finding:** Windows running from SSD — optimal configuration. Both HDDs at 0% fragmentation — no action needed.

---

## Step 12 — Memory Analysis (Resource Monitor)

**Path:** Task Manager → Performance → Open Resource Monitor → Memory tab

| Metric | Value |
|--------|-------|
| Physical Memory In Use | 6039 MB |
| Available | 1980 MB |
| Standby (cached) | 1879 MB |
| Free | 101 MB |
| Hard Faults/sec | **0** ✅ |
| Total Installed | 8192 MB |

Top consumers: Firefox (~20 tabs open) + Memory Compression — both normal

![Resource Monitor - Memory tab](images/case9-performance/24-resource-monitor-memory.png)
![Task Manager after optimization - CPU 3%, Memory 72%](images/case9-performance/25-task-manager-after.png)

**Conclusion:** Hard Faults/sec = 0 → **No memory leak detected**. 72% usage with 20 browser tabs is expected and normal. Standby memory is cached — Windows reclaims it instantly when needed.

---

## Storage Overview

| Drive | Type | Free | Total |
|-------|------|------|-------|
| C: (System/SSD) | SSD | 158 GB | 236 GB |
| D: | HDD | 388 GB | 488 GB |
| E: | HDD | 300 GB | 443 GB |

![File Explorer drives overview](images/case9-performance/26-drives-overview.png)

---

## Final Results Summary

| Optimization | Result |
|---|---|
| Startup Apps | All non-essential disabled ✅ |
| Visual Effects | Minimal custom set ✅ |
| Power Plan | Custom — sleep never, CPU max 100% ✅ |
| Services | Reviewed — appropriately configured ✅ |
| Virtual Memory | System managed on SSD ✅ |
| Delivery Optimization | Off ✅ |
| Background Apps | Key apps set to Never ✅ |
| Taskbar | Widgets, Search, Task View hidden ✅ |
| Firefox | Hardware acceleration + 256MB cache cap ✅ |
| Disk Cleanup | ~1.8 GB freed ✅ |
| Drive Health | All drives OK ✅ |
| Memory Leak Check | Hard Faults/sec = 0 — clean ✅ |

---

## Key Takeaway

> Maximum performance doesn't require new hardware.
> A systematic approach — startup apps, visual effects, power settings, background permissions,
> browser tuning, and disk cleanup — significantly improves responsiveness on 8GB RAM systems.
> Always validate with Resource Monitor after optimization to confirm no memory leaks
> and that changes had the intended effect.

---

## Tags
`windows` `performance` `optimization` `ssd` `hdd` `startup` `visual-effects` `disk-cleanup` `virtual-memory` `power-plan` `firefox` `background-apps` `delivery-optimization` `resource-monitor` `8gb-ram` `memory-analysis`
