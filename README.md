# home-cloud

Home cloud, home lab, and local AI — one repo, three tracks.

- **[PLAN.md](PLAN.md)** — goals, ground rules, checklists, decisions log. Start here.

## Layout (folders get created when the work starts, not before)

| Folder | Track | Contents |
|---|---|---|
| `nas/` | A — Protect family data | NAS setup notes, share layout, backup jobs, restore-test log |
| `lab/` | B — Pi 5 k3s lab | k3s install notes, manifests / Helm values (GitOps), AdGuard, Prometheus, Grafana |
| `ai/` | C — Local AI on the Mac | Ollama notes, RAG experiments, MCP server |

## Rules

- Nothing the family depends on may depend on the lab.
- No secrets in this repo (`.env`, keys, tokens are gitignored).
- Buy hardware only when a measured need appears.
