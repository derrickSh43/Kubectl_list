# 🧠 Kubernetes Admin Command Reference

---

## 📘 Help & Docs

# Show general help
```kubectl --help```

# Get help for specific command
```kubectl get --help```

# Explain field structure for a resource
```kubectl explain pod.spec.containers```

---

## 🟢 Core Commands

# List all nodes
```kubectl get nodes```

# List all pods in all namespaces
```kubectl get pods -A```

# Get all services with IPs and ports
```kubectl get svc -o wide```

# View events sorted by time
```kubectl get events --sort-by=.metadata.creationTimestamp```

# Get current contexts
```kubectl config get-contexts```

# Switch context
```kubectl config use-context <context-name>```

---

## 📆 Scheduling

# Show node labels
```kubectl get nodes --show-labels```

# Label a node
```kubectl label node <node-name> zone=us-west1```

# Taint a node to prevent scheduling
```kubectl taint nodes <node-name> key=value:NoSchedule```

# View where pods are scheduled
```kubectl get pods -o wide```

---

## 📊 Logging & Monitoring

# Get logs from a pod
```kubectl logs <pod-name> -n <namespace>```

# Follow logs from a pod
```kubectl logs -f <pod-name> -n <namespace>```

# Get logs from previous failed pod
```kubectl logs --previous <pod-name> -n <namespace>```

# View resource usage for all pods
```kubectl top pod -A```

# View node CPU/memory
```kubectl top node```

# Port-forward to Prometheus
```kubectl port-forward svc/prometheus 9090 -n monitoring```

# Port-forward to Grafana
```kubectl port-forward svc/grafana 3000 -n monitoring```

---

## 🚀 App Lifecycle

# Create a deployment
```kubectl create deployment nginx --image=nginx```

# Restart a deployment
```kubectl rollout restart deployment <name> -n <namespace>```

# Scale a deployment
```kubectl scale deployment <name> --replicas=3 -n <namespace>```

# Delete a deployment
```kubectl delete deployment <name> -n <namespace>```

# View rollout status
```kubectl rollout status deployment <name> -n <namespace>```

# Roll back to previous revision
```kubectl rollout undo deployment <name> -n <namespace>```

---

## 🔄 Cluster Maintenance

# Cordon a node (unschedulable)
```kubectl cordon <node-name>```

# Drain a node (evacuate pods)
```kubectl drain <node-name> --ignore-daemonsets```

# Uncordon a node (reschedule allowed)
```kubectl uncordon <node-name>```

# Force delete a stuck pod
```kubectl delete pod <pod-name> --grace-period=0 --force -n <namespace>```

# View core component health
```kubectl get componentstatuses```

---

## 🔐 Security & Access Control

# Create a ServiceAccount
```kubectl create serviceaccount my-sa -n dev```

# Bind role to service account
```kubectl create rolebinding my-rb --role=view --serviceaccount=dev:my-sa -n dev```

# Check RBAC permissions
```kubectl auth can-i create pods --as=dev:my-sa -n dev```

# Create a Secret from literal values
```kubectl create secret generic app-secret --from-literal=password=admin123 -n <namespace>```

# View and decode a secret
```kubectl get secret app-secret -o jsonpath="{.data.password}" | base64 -d```

# Edit a secret inline
```kubectl edit secret app-secret -n <namespace>```

---

## 💾 Storage

# List all storage classes
```kubectl get sc```

# List all persistent volume claims
```kubectl get pvc -A```

# Describe a PVC
```kubectl describe pvc <name> -n <namespace>```

# Delete a PVC
```kubectl delete pvc <name> -n <namespace>```

---

## 🌐 Networking

# Get services in all namespaces
```kubectl get svc -A```

# Get all Ingress resources
```kubectl get ingress -A```

# List all Network Policies
```kubectl get networkpolicy -A```

# Port-forward to a pod
```kubectl port-forward pod/<pod-name> 8080:80 -n <namespace>```

# Port-forward to a service
```kubectl port-forward svc/<service-name> 9090:80 -n <namespace>```

---

## 🧰 Modify Without YAML

# Edit a live deployment
```kubectl edit deployment <name> -n <namespace>```

# Change deployment image
```kubectl set image deployment/<name> <container>=<image> -n <namespace>```

# Patch replica count
```kubectl patch deployment <name> -p '{"spec":{"replicas":3}}' -n <namespace>```

# Patch container image (strategic)
```kubectl patch deployment <name> --type='strategic' -p='{"spec":{"template":{"spec":{"containers":[{"name":"app","image":"nginx:1.25"}]}}}}' -n <namespace>```

---

## 🧩 Labels & Selectors

# Add label to a pod
```kubectl label pod <pod-name> env=dev --overwrite -n <namespace>```

# Get pods by label
```kubectl get pods -l app=myapp -n <namespace>```

# Delete pods by label
```kubectl delete pod -l app=myapp -n <namespace>```

---

## 🧪 Dry Runs & Inline YAML

# Preview deployment as YAML
```kubectl create deployment nginx --image=nginx --dry-run=client -o yaml```

# Apply inline YAML block
```
kubectl apply -f - <<EOF
apiVersion: v1
kind: Namespace
metadata:
  name: dev-env
EOF
```
---

## 📁 Copy Files

# Copy file from pod to local
```kubectl cp <pod-name>:/path/in/container ./local-dir -n <namespace>```

# Copy file from local to pod
```kubectl cp ./myfile.txt <pod-name>:/tmp -n <namespace>```

---

## 📦 Helm Basics

# Add Helm repo
```helm repo add prometheus https://prometheus-community.github.io/helm-charts```

# Update Helm repo
```helm repo update```

# Install chart
```helm install prometheus prometheus/prometheus -n monitoring --create-namespace```

# List Helm releases
```helm list -A```

# Upgrade release
```helm upgrade <release> <chart> -f values.yaml```

# Uninstall release
```helm uninstall <release> -n <namespace>```

---

## 🛠️ Kustomize Basics

# Apply a kustomization directory
```kubectl apply -k ./overlays/dev```
```
# Example structure:
# base/
# └── deployment.yaml
# overlays/dev/
# ├── kustomization.yaml
# └── patch.yaml
```
# Example kustomization.yaml
```
resources:
  - ../../base
patchesStrategicMerge:
  - patch.yaml
```
---

## ⚙️ Common Flags

# Use these with most commands
-n <namespace>      # Target namespace
-A                  # All namespaces
-o wide             # Extended info (IPs, nodes, etc.)
-o yaml             # Output as YAML
--dry-run=client    # Simulate the command
--grace-period=0    # Skip graceful termination
--force             # Force delete
-l key=value        # Filter by label
--sort-by=<field>   # Sort (e.g., creationTimestamp)

---
