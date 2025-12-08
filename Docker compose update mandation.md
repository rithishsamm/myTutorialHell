# **Internal Engineering Memo: Standardizing Local Volume Mounts in Docker Compose for Development**

**Author**: Rithish Sam  
**Date**: 28 November 2025  
**Team**: Hexr  
**Subject**: Recommended Practice (Mandatory for Local Dev Environments)

---

## **Purpose**

To improve **local development consistency**, **debuggability**, and **data transparency**, we are standardizing how services persist and load configuration/data in `docker-compose.yml` files — specifically:

> **Prefer explicit host-path volume mounts over anonymous/named Docker volumes** for configs, state, and logs **in local development environments**.

This ensures:
- Config files are editable directly on the host (no `docker cp` gymnastics).
- Data is inspectable & version-controllable (e.g., `git diff`, `grep`, backups).
- Easier reproducibility across team members (no hidden Docker volume state).
- Clear separation between ephemeral runtime data (ok in volumes) vs. Durable/local dev assets.

## **Scope**

Applies to **local development** (`docker-compose.*.yml`) only — *not* production manifests (Helm/K 8 s) or CI environments.

> ! This does **not** mandate removing all Docker volumes. Use them for:
> - Truly ephemeral runtime data (e.g., `/tmp`, caches).
> - Cases where host-path mounts are not feasible (e.g., Windows → Linux WSL 2 path translation limits).
> - Services where local dev data isolation is preferred (e.g., per-branch databases).

## **Standard: Local Volume Mount Pattern**

### 1. **Directory Layout Convention (Recommended)**
All local dev data/config should be co-located with the project in a predictable, `.gitignore` -managed structure:

```
project-root/
├── src/
├── docker-compose.yml
├── .env
├── data/                   <-- ✅ All host-mounted data goes here
│   ├── app-name/
│   │   ├── config.yaml
│   │   └── state/          <-- e.g., db/, cache/, logs/
│   └── shared/             <-- optional: shared configs (e.g., certs, CAs)
└── ...
```

**`.gitignore` should exclude runtime data** (e.g., `data/*/state/**`, `data/mongo-data/`) but **track config files**.

### 2. **Volume Mount Syntax (Cross-Platform)**

Use **relative or absolute paths with forward slashes**, even on Windows (Docker Desktop/WSL 2 handles this well):

| Type | Example | Purpose |
|------|---------|---------|
| **Config (read-only)** | `./data/app/config.yaml:/etc/app/config.yaml:ro` | Immutable configs |
| **State (read-write)** | `./data/app/state:/var/lib/app/data` | Databases, caches, uploads |
| **Logs (optional)** | `./data/app/logs:/var/log/app` | Debugging, log inspection |

>  **Pro tip**: For Windows users, avoid drive letters in *shared* repos (e.g., `F:/...`). Prefer relative paths or environment variables:

```yaml
volumes:
- ${DATA_ROOT:-./data}/otel-collector/config.yaml:/etc/otel-collector-config.yaml:ro
 ```

Then set `DATA_ROOT=F:/rithishsamm/data` in `.env` or shell.

### 3. **Before → After: Example Transformation**

#### Legacy (Named Volume)
```yaml
volumes:
  mongo-data:

services:
  mongo:
    image: mongo:latest
    volumes:
      - mongo-data:/data/db
```

#### Standardized (Local Host Mount)
```yaml
services:
  mongo:
    image: mongo:latest
    volumes:
      - ./data/mongo/state:/data/db          # RW: DB files
      - ./data/mongo/init:/docker-entrypoint-initdb.d:ro  # Optional: init scripts
```

>  `./data/mongo/state` is `.gitignore` 'd  
>  `./data/mongo/init/*.js` can be tracked if needed

### 4. **Full Reference: Observability Stack (Your Example)**

Already compliant — great model for the team!

```yaml
volumes:
  # No top-level 'volumes' needed unless sharing between services
services:
  loki:
    volumes:
      - ./data/loki/config.yaml:/etc/loki/local-config.yaml:ro
      - ./data/loki/state:/tmp/loki
  grafana:
    volumes:
      - ./data/grafana/datasources.yml:/etc/grafana/provisioning/datasources/datasources.yml:ro
      - ./data/grafana/dashboards:/var/lib/grafana/dashboards:ro
      - ./data/grafana/state:/var/lib/grafana      # ← persists plugins, users, etc.
```

> Note: Grafana’s `/var/lib/grafana` persistence is critical for retaining state across restarts.

## **Action Items for Team Members**

1. **Audit** your `docker-compose.*.yml` files:
   - Replace named volumes (e.g., `mongo-data:`) with `./data/<service>/state`.
   - Externalize configs into `./data/<service>/config.*`.
2. **Update `.gitignore`**:
   ```
   # data/
   data/**/state/
   data/**/logs/
   !data/**/config*   # ← keep configs
   !data/**/dashboards/*.json
   ```
3. **Document** your layout in `README.md#local-dev`.
4. **Verify** on Windows (WSL 2) and macOS/Linux — paths should work universally.

## **FAQ**

**Q: What about performance (e.g., bind-mounting large DBs on macOS)?**  
A: Use `delegated` or `cached` mount options on macOS if needed (`:delegated`). For heavy I/O, consider named volumes *only* for DB data — but document the trade-off.

**Q: How do I migrate existing Docker volume data?**  
A: Use `docker run --rm -v <vol>:/from -v ./data/svc/state:/to alpine cp -r /from/. /to/`.

**Q: Should CI use this too?**  
A: No — CI should use ephemeral volumes or in-memory storage. Local dev ≠ CI.

Let’s keep the infrastructure that’s transparent, debuggable, and team-friendly.

Rithishsamm,
*DevOps & QA Engineering Team*

