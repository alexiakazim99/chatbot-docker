# Chatbot (Docker / Compose / Kubernetes)

A minimal chatbot: a small Flask web page where you type a question,
it calls the OpenAI API, and shows the answer. The same app is run
three ways — as a Docker container, with Docker Compose, and on a
single-node Kubernetes cluster.

## Files

- `app.py` — the Flask app and OpenAI call
- `requirements.txt` — Python dependencies
- `Dockerfile` — builds the image
- `compose.yaml` — runs the app with Docker Compose
- `k8s.yaml` — Deployment + Service for Kubernetes

## Requirements

- Docker Desktop
- An OpenAI API key in a `.env` file: `OPENAI_API_KEY=your-key`
  (`.env` is not committed)

## a) Docker

```bash
docker build -t chatbot .
docker run -p 5001:5001 --env-file .env chatbot
```

Open http://localhost:5001

## b) Docker Compose

```bash
docker compose up
```

Open http://localhost:5001, then stop with `docker compose down`.

## c) Kubernetes (single node)

Enable Kubernetes in Docker Desktop, then:

```bash
kubectl create secret generic openai-secret --from-env-file=.env
kubectl apply -f k8s.yaml
kubectl port-forward deployment/chatbot 8080:5001
```

Open http://localhost:8080

The API key is passed to the pod as a Kubernetes Secret, not stored
in `k8s.yaml`.
