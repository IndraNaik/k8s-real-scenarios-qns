# Kubernetes Scenario-Based Interview Questions & Solutions

## 1. How to Expose an Application to the Internet?
**Scenario:** You need to expose a service externally.

**Solution:** Use a **LoadBalancer Service** or an **Ingress Controller**.

**Example:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  type: LoadBalancer
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
```

## 2. How to Autoscale Pods Based on CPU Usage?
**Scenario:** High traffic causes resource exhaustion.

**Solution:** Use **Horizontal Pod Autoscaler (HPA)**.

**Example:**
```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 2
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

## 3. How to Share Data Between Containers in a Multi-Container Pod?
**Scenario:** Multi-container pod needs shared data.

**Solution:** Use a **shared volume**.

**Example:**
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  volumes:
    - name: shared-storage
      emptyDir: {}
  containers:
    - name: container1
      image: app1
      volumeMounts:
        - mountPath: /data
          name: shared-storage
    - name: container2
      image: app2
      volumeMounts:
        - mountPath: /data
          name: shared-storage
```

## 4. How to Restrict Resource Usage for Certain Pods?
**Scenario:** Prevent certain pods from consuming too many resources.

**Solution:** Use a **ResourceQuota**.

**Example:**
```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: namespace-quota
  namespace: my-namespace
spec:
  hard:
    requests.cpu: "2"
    requests.memory: "4Gi"
    limits.cpu: "4"
    limits.memory: "8Gi"
```

## 5. How to Inject Configuration into Pods?
**Scenario:** A pod is failing due to a missing configuration file.

**Solution:** Use a **ConfigMap**.

**Example:**
```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
  namespace: default
data:
  config.json: |
    {"setting": "value"}
```

## 6. How to Ensure a Pod Runs on a Specific Node?
**Scenario:** A pod requires specific hardware.

**Solution:** Use **nodeSelector**.

**Example:**
```yaml
spec:
  nodeSelector:
    kubernetes.io/hostname: my-specific-node
```

## 7. How to Ensure Zero Downtime During Application Updates?
**Scenario:** Users experience downtime during updates.

**Solution:** Use a **RollingUpdate strategy**.

**Example:**
```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

## 8. How to Securely Pass Database Credentials to Pods?
**Scenario:** Securely pass credentials to pods.

**Solution:** Use **Secrets**.

**Example:**
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  username: base64-encoded-username
  password: base64-encoded-password
```

## 9. How to Expose Services Using a Custom Domain?
**Scenario:** Expose services using a custom domain.

**Solution:** Use **Ingress**.

**Example:**
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app
spec:
  rules:
  - host: myapp.example.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: my-service
            port:
              number: 80
```

## 10. How to Troubleshoot a Frequently Crashing Pod?
**Scenario:** A pod frequently crashes.

**Solution:**
1. Check logs: `kubectl logs <pod-name>`
2. Describe pod: `kubectl describe pod <pod-name>`
3. Check events: `kubectl get events --sort-by=.metadata.creationTimestamp`

## 11. How to Schedule Pods on Nodes with Certain Resource Constraints?
**Scenario:** Ensure pods are scheduled on specific nodes.

**Solution:** Use **taints and tolerations**.

**Example:**
```sh
kubectl taint nodes my-node key=value:NoSchedule
```
```yaml
spec:
  tolerations:
  - key: "key"
    operator: "Equal"
    value: "value"
    effect: "NoSchedule"
```

## 12. How to Prevent Accidental Deletion of Critical Resources?
**Scenario:** A production deployment was deleted accidentally.

**Solution:** Use **RBAC** or `change-cause` annotation.

**Example:**
```sh
kubectl annotate deployment my-app kubernetes.io/change-cause="Protected from deletion"
```

## 13. How to Ensure Pods Are Ready Before Receiving Traffic?
**Scenario:** Ensure pods are ready before receiving traffic.

**Solution:** Use **readinessProbe**.

**Example:**
```yaml
readinessProbe:
  httpGet:
    path: /healthz
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 10
```

## 14. How to Perform Maintenance on a Node Without Causing Downtime?
**Scenario:** Perform maintenance without downtime.

**Solution:**
1. Cordon the node: `kubectl cordon <node-name>`
2. Drain the node: `kubectl drain <node-name> --ignore-daemonsets --delete-emptydir-data`
3. Uncordon after maintenance: `kubectl uncordon <node-name>`

---
## Conclusion
These Kubernetes scenarios cover real-world challenges. Understanding these solutions will help you handle Kubernetes deployments efficiently and ace Kubernetes interviews! 🚀

