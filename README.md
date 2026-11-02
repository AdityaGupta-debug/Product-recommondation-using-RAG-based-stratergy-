# Flipkart Product Recommender (RAG)

A production-style **Retrieval-Augmented Generation (RAG)** chatbot that recommends Flipkart products with answers grounded in retrieved product data. It is served through a **FastAPI** backend, containerized with **Docker**, deployed on **Kubernetes (Minikube) on a GCP VM**, and monitored with **Prometheus** and **Grafana**.

![Python](https://img.shields.io/badge/Python-3.10+-blue)
![FastAPI](https://img.shields.io/badge/FastAPI-009688)
![LangChain](https://img.shields.io/badge/LangChain-RAG-1C3C3C)
![Docker](https://img.shields.io/badge/Docker-2496ED)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5)
![Prometheus](https://img.shields.io/badge/Prometheus-E6522C)
![Grafana](https://img.shields.io/badge/Grafana-F46800)

---

## Features

- **RAG pipeline** built with LangChain: retrieves the most relevant product data and passes it to the LLM, so recommendations come from your data rather than the model's memory.
- **Fast LLM inference** using Groq.
- **Semantic search** with HuggingFace embeddings and an AstraDB vector store.
- **FastAPI backend** exposing the chatbot as an API.
- **Containerized** with Docker for reproducible builds.
- **Kubernetes deployment** with manifests, run on a GCP VM using Minikube.
- **Observability** with Prometheus metrics and Grafana dashboards.

---

## Architecture

```
User Query
    |
    v
FastAPI  --->  LangChain RAG Chain
                  |            |
                  v            v
        HuggingFace        AstraDB
        Embeddings    <--  Vector Store   (top-k product retrieval)
                  |
                  v
             Groq LLM  --->  Grounded product recommendation
                  |
                  v
     Prometheus (metrics)  --->  Grafana (dashboards)
```

---

## Tech Stack

| Layer | Tools |
|---|---|
| Language | Python |
| RAG / Orchestration | LangChain |
| Embeddings | HuggingFace |
| Vector Database | AstraDB |
| LLM Inference | Groq |
| Backend | FastAPI |
| Containerization | Docker |
| Orchestration | Kubernetes, Minikube |
| Cloud | Google Cloud Platform (VM, Firewall) |
| Monitoring | Prometheus, Grafana |

---

## Getting Started

### Prerequisites

- Python 3.10+
- Docker
- An [AstraDB](https://astra.datastax.com/) account (database and application token)
- A [Groq](https://console.groq.com/) API key
- A HuggingFace token (if your embedding model requires one)

### 1. Clone the repository

```bash
git clone https://github.com/AdityaGupta-debug/Product-recommondation-using-RAG-based-stratergy-.git
cd Product-recommondation-using-RAG-based-stratergy-
```

### 2. Configure environment variables

Create a `.env` file in the project root:

```env
GROQ_API_KEY=your_groq_api_key
ASTRA_DB_API_ENDPOINT=your_astra_db_endpoint
ASTRA_DB_APPLICATION_TOKEN=your_astra_db_token
ASTRA_DB_KEYSPACE=your_keyspace
HF_TOKEN=your_huggingface_token
```

> Variable names must match what the code reads. Adjust them to your project's config if they differ.

### 3. Run locally

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt

uvicorn app:app --host 0.0.0.0 --port 5000   # adjust module:app and port to your project
```

### 4. Run with Docker

```bash
docker build -t flipkart-recommender .
docker run -p 5000:5000 --env-file .env flipkart-recommender
```

---

## Kubernetes Deployment (GCP VM + Minikube)

1. Create a GCP VM and install Docker, `kubectl`, and Minikube.
2. Open the required ports in the **GCP Firewall** (application, Prometheus, and Grafana ports).
3. Start the cluster and apply the manifests:

```bash
minikube start --driver=docker
kubectl create secret generic app-secrets --from-env-file=.env
kubectl apply -f k8s/          # adjust to your manifests folder
kubectl get pods
```

4. Expose the service and access it:

```bash
minikube service <service-name> --url
```

---

## Monitoring

Prometheus scrapes application metrics and Grafana visualizes them in real time.

```bash
kubectl apply -f monitoring/   # adjust to your monitoring manifests
```

- **Prometheus:** `http://<vm-ip>:<prometheus-port>`
- **Grafana:** `http://<vm-ip>:<grafana-port>`

---

## Project Structure

```
.
├── app.py                # FastAPI application      (adjust to your layout)
├── Dockerfile
├── requirements.txt
├── k8s/                  # Kubernetes manifests
├── monitoring/           # Prometheus and Grafana configs
└── README.md
```

---

## Author

**Aditya Upendra Gupta**
B.Tech, Artificial Intelligence and Data Science, IIIT Raichur

[GitHub](https://github.com/AdityaGupta-debug) | [LinkedIn](https://www.linkedin.com/in/aditya-upendra-gupta-3a8816314/)
