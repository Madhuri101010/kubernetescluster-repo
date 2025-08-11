---------------------------------------------
a. Install Minikube & start the cluster.
---------------------------------------------
1. curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
2. minikube start --driver=docker
3. kubectl get nodes

---------------------------------------------
b. Create deployment.yaml for an app: kubectl apply -f deployment.yaml
---------------------------------------------
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app
          image: nginx:latest
          ports:
            - containerPort: 80
---------------------------------------------
C. Expose app using service.yaml: kubectl apply -f service.yaml
---------------------------------------------
apiVersion: v1
kind: Service
metadata:
  name: my-app-deployment
  labels:
    app: my-app
spec:
  selector:
    app: my-app
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
  


---------------------------------------------
D. Use kubectl get pods to verify.
---------------------------------------------
 kubectl get pods
 
---------------------------------------------
E. Scale deployments using kubectl scale. 
---------------------------------------------
kubectl scale deployment/my-app-deployment --replicas=5


---------------------------------------------
F. Use kubectl describe for logs.
---------------------------------------------
 $ kubectl describe deploy my-app-deployment 
Name:                   my-app-deployment
Namespace:              default
CreationTimestamp:      Mon, 11 Aug 2025 16:13:30 +0530
Labels:                 app=my-app
Annotations:            deployment.kubernetes.io/revision: 1
Selector:               app=my-app
Replicas:               5 desired | 5 updated | 5 total | 5 available | 0 unavailable
StrategyType:           RollingUpdate
MinReadySeconds:        0
RollingUpdateStrategy:  25% max unavailable, 25% max surge
Pod Template:
  Labels:  app=my-app
  Containers:
   my-app:
    Image:         nginx:latest
    Port:          80/TCP
    Host Port:     0/TCP
    Environment:   <none>
    Mounts:        <none>
  Volumes:         <none>
  Node-Selectors:  <none>
  Tolerations:     <none>
Conditions:
  Type           Status  Reason
  ----           ------  ------
  Progressing    True    NewReplicaSetAvailable
  Available      True    MinimumReplicasAvailable
OldReplicaSets:  <none>
NewReplicaSet:   my-app-deployment-7c457b5497 (5/5 replicas created)
Events:
  Type    Reason             Age    From                   Message
  ----    ------             ----   ----                   -------
  Normal  ScalingReplicaSet  5m39s  deployment-controller  Scaled up replica set my-app-deployment-7c457b5497 from 0 to 3
  Normal  ScalingReplicaSet  61s    deployment-controller  Scaled up replica set my-app-deployment-7c457b5497 from 3 to 5