![01](https://github.com/user-attachments/assets/ceaf031e-881e-4719-b604-920192b87211)
![03](https://github.com/user-attachments/assets/c69025a2-b74d-4f60-84f4-e42bf031b7b2)

apiVersion: v1
kind: Namespace
metadata:
  name: first-namespace
---
apiVersion: v1
kind: Namespace
metadata:
  name: second-namespace

apiVersion: v1
kind: Service
metadata:
  name: nginx-service
  namespace: first-namespace
  labels:
    app: my-app
spec:
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP


apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  namespace: first-namespace
  labels:
    app: my-app
spec:
  replicas: 1
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80



