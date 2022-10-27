# A - Docker Images:

## Delete image 
```
$ docker rmi authentication:latest -f
```

## Build image
```
$ docker build -t authentication:latest .
$ docker build --platform=linux/amd64 -t authentication:latest .  //for Mac M1/M2
```
### Sample docker build file
```
FROM eclipse-temurin:17-alpine as builder
WORKDIR extracted
ADD ./target/AuthenticationService-0.0.1-SNAPSHOT.jar app.jar
RUN java -Djarmode=layertools -jar app.jar extract

FROM eclipse-temurin:17-alpine
WORKDIR application
COPY --from=builder extracted/dependencies/ ./
COPY --from=builder extracted/spring-boot-loader/ ./
COPY --from=builder extracted/snapshot-dependencies/ ./
COPY --from=builder extracted/application/ ./

EXPOSE 8080

ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]
```


## Get image
```
$ docker images | grep order
```

## Tag image
```
$ docker tag authentication:latest phucthaihg/authentication:1.0.1
```

## Push image to Docker Hub
```
$ docker push phucthaihg/authentication:1.0.1
```


# B - Kubernetes

## Show clusters (contexts) :
```
$ kubectl config get-contexts
```

## Switch to wanted context
```
$ kubectl config use-context gke_bubbly-enigma-366417_us-central1-c_sa-final-project-cluster
```

## Deploy
```
kubectl apply -f authentication-service.yml
```

### Sample deployment file (service & deployment)
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

## Show all info (pods, services ...)
```
$ kubectl get all
```

## Show only services
```
$ kubectl get svc
```

## Monitor logs of a pod
```
$ kubectl logs authentication-service-6b6dcdf5fd-8ldcf -f
```

## Login to a Pod (Cassandra/ MySQL/ Services) 
```
$ kubectl exec -it pod/cassandra-1-cassandra-0 bash
```

## Start Cassandra inside Pod
```
$ cqlsh
```

## Start MySQL inside Pod
```
$ mysqlsh
```

# C - Cassandra

## Create a Keyspaces
```
$ CREATE KEYSPACE IF NOT EXISTS authentication WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };
```

## Describe all Keyspaces
```
$ DESC keyspaces;
```

## Query in a table
```
$ SELECT * FROM authentication.user;
```

# MySQL

## login with root
```
$ mysql -u root -p
```

## show databases
```
$ SHOW DATABASES;
```

## use databases
```
$ USE authenticationdb;
```


# Ect.

## Mount to minikube
```
$ minikube start --mount-string="kc_data:/opt/keycloak/data/import" --mount
```
