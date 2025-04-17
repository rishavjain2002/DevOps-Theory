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

```




### 1. **`selector.matchLabels`**

```yaml

selector:
  matchLabels:
    app.kubernetes.io/name: app-2048

```
This is telling Kubernetes **which Pods this Deployment should manage**. It does this by matching Pods that have **labels** with the specified key-value pair:

- key: `app.kubernetes.io/name`
- value: `app-2048`

In short, this line means: _"This Deployment should manage all Pods that are labeled with `app.kubernetes.io/name=app-2048`."_



### 2. **`labels` inside `template.metadata`**

```yaml

labels:
  app.kubernetes.io/name: app-2048

```

This is saying that **the Pods created by this Deployment will have this label**.
So each Pod launched will include:

```yaml

metadata:
  labels:
    app.kubernetes.io/name: app-2048

```

---

### Why both are needed?

- The `selector.matchLabels` defines **what Pods the Deployment will control**.
- The `template.metadata.labels` ensures that **new Pods created by the Deployment match that selector**.



---


In your YAML definition, this line determines how many Pods are created:
`replicas: 5`

✅ **That means 5 Pods** will be created and maintained by the Deployment.

Each of those 5 Pods will:
- Run one container (`app-2048`)
- Be labeled with `app.kubernetes.io/name: app-2048`
- Listen on port `80`
- Be managed by the Deployment named `deployment-2048` in the `game-2048` namespace
    

If any Pod crashes or is deleted, Kubernetes will automatically spin up another to maintain 5 running Pods.






---



