# MariaDB Master/Slave Cluster on Kubernetes (StatefulSet)

This project demonstrates how to deploy a **MariaDB master-slave replication setup** using Kubernetes **StatefulSet** and **Longhorn** for persistent storage.

⚠️ **Status: Experimental / Work in Progress**

---

## 🚀 Project Overview

This setup consists of:

- A `StatefulSet` with **2 MariaDB pods**:
  - `mariadb-0` acts as the **master**
  - `mariadb-1` acts as the **slave**
- **Master/Slave configuration** is handled via `initContainers` and `ConfigMap`
- Persistent volumes are provisioned dynamically using **Longhorn**
- Uses **ConfigMaps** to inject different `.cnf` and `.sql` files based on pod index

---

## 📁 Project Structure

- `ConfigMap`: Contains separate config files and init SQL for master and slave
- `StatefulSet`: Creates two MariaDB pods with specific initialization logic
- `Service`: Headless service to allow DNS-based internal communication between pods

---

## ⚙️ Requirements

- Kubernetes cluster (v1.30+ recommended)
- [Longhorn](https://longhorn.io/) installed as the default or named `storageClass`
- `kubectl` configured and connected to your cluster

---

## ⚠️ Important Notes

- **Default Credentials**:
  - `root` password: `123`
  - Replication user: `radin`
  - Replication password: `123`
- **Please make sure to change the default passwords** before using this in any real environment.
- This project is **not production ready**. Stability, fault tolerance, and failover mechanisms are still being improved.
- Longhorn is used with `ReadWriteMany` mode — make sure your Kubernetes setup and Longhorn are properly configured for this.

---

## 🧪 How It Works

- Pod index `0` is detected as the **master**, and the appropriate configuration is mounted from the `ConfigMap`
- Pod index `1` is treated as a **slave**, which connects to the master using the headless service DNS name
- MariaDB image auto-runs the SQL scripts on first initialization

---

## 📦 Deployment Steps

```bash
# Create the namespace
kubectl create namespace db

# Apply the config map
kubectl apply -f configmap.yaml

# Deploy the StatefulSet and Service
kubectl apply -f statefulset.yaml
kubectl apply -f service.yaml
```

> Replace the above filenames with your actual manifest filenames if different.

---

## 🤝 Contributions

Contributions, issues and feature requests are welcome!

