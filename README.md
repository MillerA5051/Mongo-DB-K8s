# mongo-express-k8s

A simple Kubernetes cluster running MongoDB and Mongo Express, deployed locally with Minikube. Built while learning kubectl and Minikube fundamentals — based on the [Kubernetes Tutorial by TechWorld with Nana](https://www.youtube.com/watch?v=X48VuDVv0do).

## Project structure

```
mongo-DB-k8s/
├── README.md
├── mongoDB/
│   ├── mongo.yaml            # MongoDB Deployment + internal Service
│   ├── mongo-express.yaml    # Mongo Express Deployment + external Service (LoadBalancer)
│   ├── mongo-configmap.yaml  # ConfigMap: database_url used by Mongo Express
│   └── mongo-secret.yaml     # Secret: MongoDB root username/password
└── .gitignore
```

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Setup

1. Start Minikube:
   ```bash
   minikube start
   ```

2. Apply the secret and configmap first (the deployments reference them):
   ```bash
   kubectl apply -f mongoDB/mongo-secret.yaml
   kubectl apply -f mongoDB/mongo-configmap.yaml
   ```

3. Deploy MongoDB and Mongo Express:
   ```bash
   kubectl apply -f mongoDB/mongo.yaml
   kubectl apply -f mongoDB/mongo-express.yaml
   ```

4. Check that everything is running:
   ```bash
   kubectl get pods
   kubectl get services
   ```

5. Open Mongo Express in the browser:
   ```bash
   minikube service mongo-express-service
   ```

## Notes on credentials

`mongo-secret.yaml` currently contains base64-encoded placeholder credentials (`username` / `password`), **not encryption** — anyone with the file can decode them (`echo <value> | base64 -d`). Treat this file as a template:

- Don't commit any real credentials to this or a repo.
- Confirm `.gitignore` actually excludes `mongo-secret.yaml` if you swap in real values locally.
- For anything beyond local learning, use a real secrets manager (e.g. Sealed Secrets, External Secrets Operator, or your cloud provider's secret store).

## What's next

- [ ] Add resource limits/requests to both Deployments
- [ ] Add a namespace instead of using `default`
- [ ] Add liveness/readiness probes
- [ ] Document teardown steps (`kubectl delete -f mongoDB/`)
# Mongo-DB-K8s
