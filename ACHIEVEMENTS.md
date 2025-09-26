## ShopMicro Lab Achievements

### Easter Eggs Discovered

- **Secret Endpoint**
  - Path: backend hidden route discovered during API exploration
  - Proof: [evidence/secret_endpoint.PNG](../evidence/secret_endpoint.PNG)

- **Konami Code (Frontend Dev Mode)**
  - Trigger: ↑↑↓↓←→←→BA on the frontend
  - Proof: [evidence/konami_mode.PNG](../evidence/konami_mode.PNG)

- **Metrics Detective (Coffee Consumption)**
  - Found in ML service custom metrics; visible in Grafana metrics panel
  - How: Inspect ML service metrics endpoint and Grafana query for coffee metric

- **Pod Whisperer**
  - Action: Named a pod with a Kubernetes mascot pattern to trigger special logs
  - Proof: [evidence/pod_whisperer.PNG](../evidence/pod_whisperer.PNG)

- **Time Traveler (Retro Mode)**
  - Action: Set namespace annotation `retro.mode: "1985"` to enable retro timestamps
  - Proof: in namespaces.yaml annotations [retro](../k8s/namespace.yaml)

### Bonus Challenges Completed

- **Horizontal Pod Autoscaling (backend)**
  - File: `k8s/services/hpa-backend.yaml`
  - Command:
    ```bash
    kubectl apply -f k8s/services/hpa-backend.yaml
    kubectl -n shopmicro get hpa
    ```

- **Persistent Storage (PostgreSQL)**
  - File: `k8s/deployments/postgres.yml` (PVC + Deployment + Service)
  - Notes: Replaced `emptyDir` with PVC `postgres-pvc` and mount `/var/lib/postgresql/data`
  - Command:
    ```bash
    kubectl apply -f k8s/deployments/postgres.yml
    ```

- **Ingress Configuration**
  - File: `k8s/ingress/ingress.yaml`
  - Host: `shopmicro.local`
  - Commands:
    ```bash
    kubectl apply -f k8s/ingress/ingress.yaml
    # Linux host mapping
    MINIKUBE_IP=$(minikube ip)
    sudo sed -i.bak '/shopmicro\.local/d' /etc/hosts
    echo "$MINIKUBE_IP shopmicro.local" | sudo tee -a /etc/hosts
    ```

- **Resource Requests & Limits (backend)**
  - File: `k8s/deployments/backend.yaml` (resources.requests/limits added)
  - Command:
    ```bash
    kubectl apply -f k8s/deployments/backend.yaml
    ```

- **Health Checks (Comprehensive)**
  - File: `k8s/deployments/backend.yaml` (startupProbe, liveness, readiness tuned)
  - Command:
    ```bash
    kubectl apply -f k8s/deployments/backend.yaml
    ```

- **Chaos Engineering (Pod kill & recovery)**
  - Commands:
    ```bash
    kubectl -n shopmicro delete pod $(kubectl -n shopmicro get pods -l app=backend -o jsonpath='{.items[0].metadata.name}')
    kubectl -n shopmicro rollout status deploy/backend
    ```


### Verification

- Pods healthy:
  ```bash
  kubectl get pods -n shopmicro
  ```
- Metrics visible:
  ```bash
  kubectl -n shopmicro get hpa
  ```

---

All artifacts live under `k8s/` and screenshots under `evidence/`.


