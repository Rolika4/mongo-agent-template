# MongoDB Helm Chart (with Community Operator)

This Helm chart deploys a MongoDB ReplicaSet using the MongoDB Community Operator.

---

## 📦 Included Resources

This chart includes the following Kubernetes manifests under `templates/`:

- `mongo.yaml` — MongoDBCommunity custom resource
- `secret.yaml` — stores user password for MongoDB
- `sa.yaml` — ServiceAccount for MongoDB pods
- `role.yaml` — Role to allow access to secrets and pods
- `rb.yaml` — RoleBinding to bind the role to the ServiceAccount

---

## 🚀 Deployment

### 1. Prerequisites

- Kubernetes cluster with MongoDB Community Operator installed  
- Helm 3+

> To install the MongoDB Community Operator:

```bash
kubectl apply -f https://raw.githubusercontent.com/mongodb/mongodb-kubernetes-operator/master/config/crd/bases/mongodbcommunity.mongodb.com_mongodbcommunity.yaml
kubectl apply -k github.com/mongodb/mongodb-kubernetes-operator/config/default
```

### 2. Install the chart

```bash
helm upgrade –install mongo-release . -n  –create-namespace
```

Example:

```bash
helm upgrade –install mongo-release . -n a000000a –create-namespace
```

### 🔐 Access & Authentication

The database is secured using SCRAM authentication. Credentials are defined via Kubernetes Secret (secret.yaml).

Example connection string:

```
mongodb://<username>:<password>@mongoauth-0.mongoauth-svc..svc.cluster.local:27017/<db_name>?authSource=admin
```

Or via SRV:

```
mongodb+srv://<username>:<password>@mongoauth-svc..svc.cluster.local/<db_name>?replicaSet=mongoauth&ssl=false
```

### 🔁 Useful Commands

Check pod status

```bash
kubectl get pods -n 
```

Port-forward MongoDB locally

```bash
kubectl port-forward pod/mongoauth-0 27017:27017 -n 
```

<db_name> connection

```bash
mongosh “mongodb://<username>:<password>@localhost:27017/<db_name>?authSource=admin”
```

### 🧹 Uninstall

```bash
helm uninstall mongo-release -n
```