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
```
# Show clusters (contexts) :
```

$ kubectl config get-contexts


minikube start


