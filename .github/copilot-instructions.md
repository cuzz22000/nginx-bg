# Copilot / Agent Instructions — nginx-bg

Short, focused guidance to help AI coding agents be productive in this repository.

## Big picture
- Purpose: a tiny Nginx-based demo that shows blue/green state by serving static files. See root README for intent. The runtime is an Nginx container built from `static-files`.
- Runtime components:
  - Static frontend served by Nginx (files in [static-files](static-files)).
  - Kubernetes manifest in [k8s/nginx-gb.yaml](k8s/nginx-gb.yaml) deploys image `cuzz22000/nginx-bg:latest` with service `nginx-bg`.

## Key files to inspect
- [Dockerfile](Dockerfile) — copies `static-files` into the official `nginx` image.
- [static-files/index.html](static-files/index.html) — frontend polling logic; repeatedly requests `/message.json` to update the page color and HTTP headers shown.
- [static-files/message.json](static-files/message.json) — the JSON payload that controls the color state used by the UI.
- [k8s/nginx-gb.yaml](k8s/nginx-gb.yaml) — Deployment + Service; image, labels, service type (NodePort) are important for CI/CD and cluster tests.

## Project-specific conventions and patterns
- The app is intentionally static: think in terms of file edits and container builds instead of backend code changes.
- Tagging convention: Docker `latest` corresponds to the "blue" baseline; a `green` tag exists for the alternate state — tests and deploys may rely on tag switching.
- UI behavior: frontend polls `/message.json` every 500ms (see [static-files/index.html](static-files/index.html)); avoid changing that polling frequency unless explicitly needed for demonstration.

## Common developer workflows (commands you can rely on)
- Build locally: `docker build -t nginx-bg .` (root README shows examples).
- Run locally: `docker run -p 8000:80 nginx-bg` then open `http://localhost:8000`.
- Pull/run published image: `docker run -p 8000:80 cuzz22000/nginx-bg:latest`.
- Deploy to k8s: `kubectl create -f k8s/nginx-gb.yaml` (or `kubectl apply -f ...` for idempotent deploys).

## What changes typically mean here
- Editing `static-files/message.json` changes the visible color state immediately when the container is rebuilt and redeployed (or when you replace the file in a running container image and reload). The UI shows response headers for debugging (useful to confirm which image/node served the response).
- Changing `index.html` affects both the demo behavior (polling, rendering) and what headers/info the page displays.

## Integration points and checks an AI agent should perform
- Confirm Dockerfile still copies `static-files` into `/usr/share/nginx/html` (see [Dockerfile](Dockerfile)).
- Verify `message.json` JSON shape remains {"color":"<css-color>"} and that `index.html` expects `data.color`.
- When proposing an automated change that affects deployment, update `k8s/nginx-gb.yaml` image tag if relevant and run a smoke test: curl the service and confirm the color/state.

## Examples to reference in PRs or automated edits
- To change demonstration color, update `static-files/message.json` (example file exists in repo root under `static-files`).
- To test deployment behavior locally, build and run the image, then `curl -i http://localhost:8000/message.json` to check content and headers.

## Do NOT assume
- There is no backend code or build system beyond Docker + static files — do not scaffold node/python services unless requested.

## When in doubt, ask for these confirmations
- Should new container tags be pushed to Docker Hub under `cuzz22000/nginx-bg`?
- Should changes to the demo be validated in a k8s cluster versus a local container run?

---
Please review this draft and tell me any missing repository details or workflows you'd like included. I'll iterate quickly.
