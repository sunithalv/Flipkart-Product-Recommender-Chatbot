# Flipkart Product Recommender Chatbot

This project is a containerized Flask-based chatbot application that recommends Flipkart products using LLM-powered workflows. It is designed for deployment on Kubernetes (Minikube) and includes monitoring with Prometheus and Grafana.

## Features

- Chatbot interface for product recommendations
- Data ingestion and conversion utilities
- RAG (Retrieval-Augmented Generation) chain for enhanced responses
- Containerized with Docker
- Kubernetes deployment manifests
- Monitoring with Prometheus and Grafana

## Project Structure

```
.
├── app.py
├── Dockerfile
├── flask-deployment.yaml
├── requirements.txt
├── setup.py
├── FULL DOCUMENTATION.md
├── data/
│   └── flipkart_product_review.csv
├── flipkart/
│   ├── __init__.py
│   ├── config.py
│   ├── data_converter.py
│   ├── data_ingestion.py
│   └── rag_chain.py
├── grafana/
│   └── grafana-deployment.yaml
├── prometheus/
│   └── prometheus-configmap.yaml
├── static/
│   └── style.css
├── templates/
│   └── index.html
└── utils/
```

## Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/data-guru0/TESTING-9.git
cd TESTING-9
```

### 2. Build Docker Image

```bash
docker build -t flask-app:latest .
```

### 3. Deploy on Kubernetes (Minikube)

- Start Minikube and set Docker environment:

  ```bash
  minikube start
  eval $(minikube docker-env)
  ```

- Create Kubernetes secrets:

  ```bash
  kubectl create secret generic llmops-secrets \
    --from-literal=GROQ_API_KEY="" \
    --from-literal=ASTRA_DB_APPLICATION_TOKEN="" \
    --from-literal=ASTRA_DB_KEYSPACE="default_keyspace" \
    --from-literal=ASTRA_DB_API_ENDPOINT="" \
    --from-literal=HF_TOKEN="" \
    --from-literal=HUGGINGFACEHUB_API_TOKEN=""
  ```

- Deploy the app:

  ```bash
  kubectl apply -f flask-deployment.yaml
  ```

- Port forward to access the app:

  ```bash
  kubectl port-forward svc/flask-service 5000:80 --address 0.0.0.0
  ```

### 4. Monitoring with Prometheus and Grafana

- Create monitoring namespace and apply configs:

  ```bash
  kubectl create namespace monitoring
  kubectl apply -f prometheus/prometheus-configmap.yaml
  kubectl apply -f prometheus/prometheus-deployment.yaml
  kubectl apply -f grafana/grafana-deployment.yaml
  ```

- Port forward to access Prometheus and Grafana:

  ```bash
  kubectl port-forward --address 0.0.0.0 svc/prometheus-service -n monitoring 9090:9090
  kubectl port-forward --address 0.0.0.0 svc/grafana-service -n monitoring 3000:3000
  ```

## Usage

- Open your browser at `http://<external-ip>:5000` to interact with the chatbot.
- Access Prometheus at `http://<external-ip>:9090` and Grafana at `http://<external-ip>:3000` (default credentials: admin/admin).

## Documentation

See [FULL DOCUMENTATION.md](FULL DOCUMENTATION.md) for detailed setup and deployment instructions.

## License

This project is for educational purposes.