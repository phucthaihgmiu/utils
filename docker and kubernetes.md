A - Docker Images:

# Delete image 
```
$ docker rmi authentication:latest -f
```

# Build image
```
$ docker build -t authentication:latest .
$ docker build --platform=linux/amd64 -t authentication:latest .  //for Mac M1/M2
```
Sample docker build file
```

```


# Get image
```
$ docker images | grep order
```

# Tag image
```
$ docker tag authentication:latest phucthaihg/authentication:1.0.1
```

# Push image
```
$ docker push phucthaihg/authentication:1.0.1
```


B - Kubernetes

# Show clusters (contexts) :
```
$ kubectl config get-contexts
```

# Switch to wanted context
```
$ kubectl config use-context gke_bubbly-enigma-366417_us-central1-c_sa-final-project-cluster
```

# Deploy
```
kubectl apply -f authentication-service.yml
```

Sample deployment file (service & deployment)
```
apiVersion: v1
kind: Service
metadata:
  name: authentication-service
  labels:
    app: authentication-service
spec:
  ports:
    - name: http
      port: 80
      targetPort: 8080
  selector:
    app: authentication-service
  type: LoadBalancer
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: authentication-service
  labels:
    app: authentication-service
spec:
  replicas: 2
  selector:
    matchLabels:
      app: authentication-service
  template:
    metadata:
      labels:
        app: authentication-service
    spec:
      containers:
        - name: authentication-service
          image: phucthaihg/authentication:1.2
          env:
            - name: SPRING_PROFILES_ACTIVE
              value: "prod"
          envFrom:
            - configMapRef:
                name: service-configmap
            - secretRef:
                name: service-secret
          ports:
            - name: http
              containerPort: 8080
          readinessProbe:
            httpGet:
              path: /actuator/health
              port: 8080
```

# Show all info (pods, services ...)
```
$ kubectl get all
```

# Show only services
```
$ kubectl get svc
```

# Monitor logs of a pod
```
kubectl logs authentication-service-6b6dcdf5fd-8ldcf -f
```

minikube start

