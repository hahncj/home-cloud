# HomeCloud Plan (reset October 2026)

The single source of truth for the home cloud, lab, and AI work. Update it as things get done; record decisions from chats here.

## Goals

1. **Protect family data**: replace the unsupported NAS, add backups that are actually tested.
2. **Home lab for learning**: Kubernetes, DNS/ad blocking, Prometheus and Grafana, later Home Assistant.
3. **Understand AI end to end**: run models locally and build the layers underneath an agent (inference, retrieval, tools).

## Ground rules

- **Production vs. lab.** Nothing the family depends on (storage, backups, internet, DNS) may depend on the lab staying up.
- **Buy hardware only when a measured need appears.** No speculative GPU, switch, or gateway purchases.
- **No custom tooling for tracking.** This file is enough.

## Current inventory

| Item | Status |
|---|---|
| WD NAS | Working, but no longer supported by the vendor (no security updates) |
| Raspberry Pi 5 | Unused; boots from SD card; RAM unknown (check with `free -h`) |
| MacBook, Apple M2, 24 GB | Daily machine; hosts local AI experiments (Track C) |
| UniFi USG-3P, Cloud Key, 8-port PoE switch, 2× NanoHD APs | In service; check firmware/support status of USG-3P and Cloud Key |
| Mini PC, UPS, new NAS | Not purchased |

## Track A: Protect family data (priority, keep it boring)

- [ ] Inventory what is on the WD NAS and how much space it uses; identify data that exists nowhere else
- [ ] Interim safety: copy irreplaceable data to a USB drive
- [ ] Reduce exposure of the old NAS: disable vendor cloud/remote access and port forwards
- [ ] Choose NAS (4-bay, Btrfs snapshots, Time Machine, built-in cloud backup) and drives sized from the inventory
- [ ] Choose a UPS with USB/network shutdown for NAS + network gear
- [ ] Migrate data; define shares
- [ ] Time Machine to the NAS
- [ ] Independent Apple Photos originals archive on the NAS
- [ ] Snapshots enabled
- [ ] Encrypted off-site backup
- [ ] **Restore test**: one file, one photo (repeat quarterly)
- [ ] Retire the WD NAS

## Track B: Pi 5 lab (learning, allowed to break)

- [ ] Check Pi RAM (`free -h`). 8 GB+ is comfortable for k3s + monitoring; 4 GB means a lighter stack
- [ ] Move off the SD card: M.2 HAT + NVMe SSD, or USB SSD boot
- [ ] Install Raspberry Pi OS Lite (64-bit) and k3s (single node)
- [ ] GitOps from day one: manifests/Helm values in `lab/`
- [ ] AdGuard Home in k3s; router DHCP hands out a fallback DNS so the family stays online when the lab is down
- [ ] Prometheus + Grafana (lightweight setup sized to the Pi's RAM)
- [ ] Scrape real targets: Pi node, new NAS (SNMP), UniFi gear, internet uptime/latency probe
- [ ] Alert on: backup job failures, NAS disk health, UPS on battery, internet down

## Track C: Local AI on the M2 Mac (learning, no purchases)

- [ ] **Inference:** Ollama + Open WebUI; compare small (~3–4B), medium (~7–9B) and larger (~12–14B, quantized) models for speed and quality; note how context length affects memory
- [ ] **Retrieval:** build a RAG pipeline by hand: chunking, local embedding model, pgvector in Postgres, question answering over a non-sensitive document set
- [ ] **Tools / agents:** a local model with tool calling; a small MCP server that queries Prometheus (Track B) to ask about the home network's health
- [ ] Record where it hits limits (tokens/sec, model size, context). Those numbers decide whether AI hardware is ever worth buying

Note: 24 GB of unified memory comfortably runs quantized models up to roughly the 14B class; ~30B-class is possible but tight.

## Later (decide when triggered)

| Item | Trigger |
|---|---|
| Mini PC | Track A done and Home Assistant, Paperless-ngx, or AI needs more capacity than Mac/Pi provide; can join k3s as a second node |
| Home Assistant | On the mini PC as HAOS in a VM (discovery needs host networking; add-ons need HAOS) |
| Paperless-ngx | After the new NAS and backups are proven |
| Gateway upgrade | USG-3P out of security support, or IDS/IPS limits throughput, or faster fiber arrives |
| VLANs (IoT/guest/mgmt) | With the gateway upgrade or when Home Assistant arrives |
| Multi-gig switching | Measured transfers justify it |
| AI hardware | Track C produces a measured limit |

## Decisions log

- 2026-10-09: Plan reset to three independent tracks in a single `home-cloud` repo.
- 2026-10-09: HomeCloud Planner app abandoned; old `homecloud-workspace` deleted locally (four repos remain on GitHub under hahncj).
- 2026-10-09: WD NAS is not failing; replacement is driven by lack of vendor support.
- 2026-10-09: Local AI starts on the M2 Mac; no AI hardware purchase.
- 2026-10-09: Pi 5 becomes the k3s lab (DNS + observability); Home Assistant deferred to a future mini PC.

## Open questions

- Pi 5 RAM size?
- How much data is on the WD NAS, and how much growth to plan for?
- USG-3P / Cloud Key support status?
