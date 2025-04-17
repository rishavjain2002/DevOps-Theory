
```yaml
---
apiVersion: v1
kind: Namespace
metadata:
  name: game-2048
---
apiVersion: apps/v1
kind: Deployment
metadata:
  namespace: game-2048
  name: deployment-2048
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: app-2048
  replicas: 5
  template:
    metadata:
      labels:
        app.kubernetes.io/name: app-2048
    spec:
      containers:
      - image: public.ecr.aws/l6m2t8p7/docker-2048:latest
        imagePullPolicy: Always
        name: app-2048
        ports:
        - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  namespace: game-2048
  name: service-2048
spec:
  ports:
    - port: 80
      targetPort: 80
      protocol: TCP
  type: NodePort
  selector:
    app.kubernetes.io/name: app-2048
---
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  namespace: game-2048
  name: ingress-2048
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
    alb.ingress.kubernetes.io/target-type: ip
spec:
  ingressClassName: alb
  rules:
    - http:
        paths:
        - path: /
          pathType: Prefix
          backend:
            service:
              name: service-2048
              port:
                number: 80

```




### **Ingress Resource** (Defines how external traffic enters the cluster)

- **Purpose**: The Ingress resource defines **how HTTP/HTTPS traffic** from the **outside world** (external requests) gets routed to specific services inside the cluster.
    
- **How it works**:
    
    - It uses **rules** to map incoming HTTP(S) requests (based on paths, hostnames, etc.) to **services** inside your Kubernetes cluster.
        
    - It's typically used in conjunction with an **Ingress controller** (like the AWS Load Balancer Controller or NGINX Ingress Controller) that manages the actual creation and configuration of an HTTP proxy (like an ALB or NGINX) to handle this routing.
        
- **Example**: In your case, the Ingress resource:
    
    - Routes HTTP requests from `/` (root path) to the `service-2048` service.
        
    - The routing is determined by the path (e.g., `/`), the host (e.g., `example.com`), and other possible conditions.
        

### **Service Resource** (Defines how traffic is routed among Pods within the cluster)

- **Purpose**: The **Service** defines how traffic is distributed to the **Pods** that belong to a particular application. It acts as a stable endpoint for a set of Pods, hiding the dynamic nature of Pod IPs from external consumers.
    
- **How it works**:
    
    - It selects **Pods** (based on labels or selectors) and forwards traffic to them.
    - It acts as a proxy within the cluster, directing requests to one of the available Pods that are part of the service.
    - It allows Kubernetes to handle **load balancing** and **service discovery** automatically, so Pods can be added/removed dynamically without impacting traffic.
        
- **Example**: In your `service-2048` service, it routes traffic from port 80 to the Pods labeled `app.kubernetes.io/name: app-2048`, which is how the traffic from the Ingress gets distributed to the Pods in the Deployment.

