Windows 11 Performance Optimization (8GB RAM)

This repository documents a systematic approach to maximizing system performance on Windows 11, specifically tailored for machines with 8GB of RAM. The goal is to reduce background resource consumption and improve responsiveness without hardware upgrades or OS reinstallation.

 Environment
* OS: Windows 11 — Version 10.0.26200.8039
* Hardware: 8GB RAM, SSD (System Drive), and HDD (Data Storage)

---

  Optimization Steps

 1. Startup Management
Disabled all non-essential applications from launching at boot (e.g., Epic Games, Microsoft Teams, OneDrive).
* **Benefit:** Reduces initial RAM footprint and speeds up boot time.

 2. Visual Effects Tuning
Configured a **Custom** profile in `sysdm.cpl`, disabling all animations except for "Smooth edges of screen fonts" and "Show thumbnails instead of icons."
* **Benefit:** Frees up CPU and GPU cycles for active tasks.

 3. Power Plan Configuration
Applied a custom power plan with "Minimum processor state" set to 100% when plugged in and disabled PCI Express Link State Power Management.

  4. Services & Background Apps
* Reviewed core services like `SysMain` and `Windows Search` for SSD compatibility.
* Set background permissions for Microsoft Store and Mail apps to **Never**.

 5. Browser Optimization (Firefox)
Implemented a hard limit on memory cache via `about:config` by setting `browser.cache.memory.capacity` to **256MB**.
* **Benefit:** Prevents the browser from expanding RAM usage indefinitely.

 6. Disk Cleanup & Virtual Memory
* Performed a system-level cleanup, freeing approximately **1.8 GB** of storage.
* Verified that Virtual Memory is **System Managed** on the SSD to ensure the fastest paging performance.

---

 Final Results

| Metric | Result |
| :--- | :--- |
| **Idle CPU Usage** | 3% |
| **RAM Usage (with 20+ tabs)** | 72% |
| **Hard Faults/sec** | **0** (No memory leaks detected) |

---

 Documentation
Detailed screenshots for each step, including Resource Monitor analysis and Drive health checks, are located in the `/images` directory.

 Key Takeaway
> "Maximum performance doesn't require new hardware. A systematic approach to managing startup apps, visual effects, and browser tuning significantly improves responsiveness on 8GB RAM systems."

---
*Disclaimer: These optimizations were performed for a specific use case. Always create a restore point before modifying system services or registry settings.*
