# Day 19 — Apache Multi-Directory Static Website Hosting

## Platform
KodeKloud Engineer — xFusionCorp Industries

## Task Description

xFusionCorp Industries is planning to host two static websites on their infra in `Stratos Datacenter`. The development of these websites is still in-progress, but we want to get the servers ready.

## Requirements

- **a.** Install `httpd` package and dependencies on `App Server 1`.
- **b.** Apache should serve on port `5004`.
- **c.** Two website backups `/home/thor/media` and `/home/thor/games` exist on `jump_host`. Set them up on Apache so that:
  - `media` works on `http://localhost:5004/media/`
  - `games` works on `http://localhost:5004/games/`
- **d.** Verify with:
  ```bash
  curl http://localhost:5004/media/
  curl http://localhost:5004/games/
  ```

## Environment

| Host       | Role          | User  | Password  |
|------------|---------------|-------|-----------|
| jump_host  | Jump Server   | thor  | mjolnir123|
| stapp01    | App Server 1  | tony  | Ir0nM@n   |