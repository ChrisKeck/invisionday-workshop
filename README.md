# invisionday-workshop

* kubectl config view

* kubectl config use-context docker-desktop

* kubectl get pods --all-namespaces  -o wide

* kubectl create namespace firstrun

* kubectl config set-context --current --namespace=firstrun

* Hello-Pod erstellen
```
cat <<EOF | kubectl apply -f -
	apiVersion: v1
	kind: Pod
	metadata:
	  name: hello-pod
	  labels:
		app: web
		zone: prod
		version: v1
	spec:
	  containers:
	  - name: hello-ctr
		image: nigelpoulton/k8sbook:latest
		ports:
		- containerPort: 8080
	EOF
```
* kubectl describe pod hello-pod

* Hello-Pod-Service erstellen
```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: Service
metadata:
  name: svc-np
  labels:
    app: web
spec:
  type: NodePort
  ports:
    - port: 8080
      nodePort: 30001
  selector:
    app: web
EOF
```
* kubectl get svc

* kubectl describe svc svc-np

* curl localhost:30001

* kubectl delete pod hello-pod

* Fertig!

## Backup

* Deployment erstellen
```
cat <<EOF | kubectl apply -f -
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web
  labels:
    app: web
    zone: prod
    version: v1
spec:
  selector:
    matchLabels:
      app: web
  replicas: 1
  strategy:
    type: RollingUpdate
  template:
    metadata:
      labels:
        app: web
        zone: prod
        version: v1
    spec:
      containers:
      - image: nigelpoulton/k8sbook:latest
        name: web-ctr
        ports:
        - containerPort: 8080
EOF
```
