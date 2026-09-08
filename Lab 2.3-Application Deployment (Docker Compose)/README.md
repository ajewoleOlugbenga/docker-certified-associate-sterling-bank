# Lab 2.3: Application Deployment (Docker Compose)

**Goal:** Transition from imperative CLI commands to a declarative YAML blueprint to deploy a multi-tier capstone architecture.

---

## Step 1: Set Up the Project Directory

Create a dedicated folder for the Compose project and navigate into it:

```bash
mkdir capstone_project
cd capstone_project
```

---

## Step 2: Author the Compose Blueprint

Open your text editor (VS Code, Notepad, etc.) and create a new file named `docker-compose.yml`.

Type or paste the following deployment configuration:

```yaml
version: '3.8'
services:
  database:
    image: redis:alpine
    volumes:
      - dashboard_data:/data

  web:
    image: nginx:alpine
    ports:
      - "8080:80"
    volumes:
      - ./index.html:/usr/share/nginx/html/index.html
    depends_on:
      - database

volumes:
  dashboard_data:
```

Save the file.

---

## Step 3: Launch the Multi-Tier Stack

In your terminal, execute the Compose deployment command to instantly spin up the entire architecture:

```bash
docker compose up -d
```

Observe the output as Compose automatically creates the default network, provisions the `dashboard_data` volume, and starts both services in the correct dependency order.

---

## Step 4: Verify and Teardown

Run the following command to see the status of all containers managed by this specific blueprint:

```bash
docker compose ps
```

Open your browser to [http://localhost:8080](http://localhost:8080) to verify the Nginx frontend is live.

Once verified, gracefully tear down the entire environment (stopping containers and deleting the default network) by running:

```bash
docker compose down
```

---

## Common Issues & Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| `version` attribute obsolete warning | Newer Compose (v2) deprecates the top-level `version` key | Harmless — safe to ignore, or delete the `version: '3.8'` line |
| YAML parse error (`did not find expected key`) | Inconsistent indentation or mixed tabs/spaces | Use 2-space indentation throughout; retype instead of pasting from formatted sources like PDFs/docs |
| `bind: address already in use` on port 8080 | Another process/container is already using port 8080 | Check with `docker ps` / `lsof -i :8080`, or change the host port mapping (e.g. `"8081:80"`) |
| `web` starts before `database` is actually ready | `depends_on` only controls container **start order**, not service readiness | Not an issue for this lab (nginx doesn't call Redis), but for real dependencies use healthchecks + `condition: service_healthy` |
| `docker compose ps` shows nothing | Running the command from the wrong directory | `cd` back into `capstone_project` — Compose scopes to the current folder |
| Browser shows default "Welcome to nginx!" page | Expected — no custom content has been mounted | This confirms success, not a failure |
| Volume/network name collisions | Leftover volumes/networks from a previous lab with the same name | Check with `docker volume ls` and `docker network ls` |
| `dashboard_data` volume persists after `docker compose down` | This is intentional — plain `down` removes containers and networks but keeps named volumes | Only use `docker compose down -v` if you specifically want to delete the volume too |

---

## Key Concepts

- **Declarative vs. imperative:** Compose lets you define your entire multi-container stack in one YAML file instead of running individual `docker run` commands.
- **Automatic networking:** Compose creates a default network so services can reach each other by service name (e.g. `database`, `web`).
- **Named volumes:** `dashboard_data` persists independently of container lifecycle, surviving `docker compose down` (but not `docker compose down -v`).
- **`depends_on`:** Controls startup order only — not a readiness/health check.