# <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Telegram-Animated-Emojis/main/Objects/Bar%20Chart.webp" alt="Bar Chart" width="50" height="50" /> A simple Flask application that exports Prometheus metrics.
**(There is also a self-hosted runner k8s manifests for this project, based on the myoung34/github-runner image)**

## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Telegram-Animated-Emojis/main/Symbols/Exclamation%20Question%20Mark.webp" alt="Exclamation Question Mark" width="30" height="30" /> What the app does

- When you visit `/`, it counts the number of visits.
- When you visit `/metrics`, it exposes metrics in a format that Prometheus understands.


## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Telegram-Animated-Emojis/main/Objects/Card Index Dividers.webp" alt="Card Index Dividers" width="30" height="30" /> Project Structure

```bash
.
├── app.py
├── Dockerfile
├── gh-runner
│   ├── namespace.yml
│   ├── runner-deploy.yml
│   └── runner-secret.yml
├── manifests
│   ├── deploy.yml
│   ├── service-monitor.yml
│   └── service.yml
├── README.md
└── requirements.txt
```


## <img src="https://raw.githubusercontent.com/Tarikul-Islam-Anik/Telegram-Animated-Emojis/main/Travel and Places/Rocket.webp" alt="Rocket" width="30" height="30" /> Installation Guide

### 1. Build docker image

```bash
docker build -t your-dockerhub-username/flask-app:1.0.0 .
```

### 2. Push to Docker Hub

```bash
docker login
docker push your-dockerhub-username/flask-app:1.0.0
```

### 3. Update deploy.yml

**Change path of the image**
```yml
containers:
- name: simple-flask-app
  image: your-dockerhub-username/flask-app:1.0.0
  ports:
  - containerPort: 5000
```

### 4. Install kube-prometheus-stack helm chart

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```
```bash
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring --create-namespace
```

### 5. Apply manifests

```bash
kubectl apply -f deploy.yml
kubectl apply -f service.yml
kubectl apply -f service-monitor.yml
```

### 6. Port-forward services

App:
```bash
kubectl port-forward svc/flask-app-svc 5000:5000
```

Prometheus:
```bash
kubectl port-forward svc/prometheus-operated -n monitoring 9090:9090
```

Grafana:
```bash
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80
```

### 7. Go to grafana and create dashboard

```localhost:3000```

**Login**: admin

**Password**: prom-operator

### 8. Test it

```localhost:5000```
