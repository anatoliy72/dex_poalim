
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
  type: NodePort  
  selector:
    app: my-app
  ports:
    - protocol: TCP
      port: 80          
      targetPort: 80    
      nodePort: 30007   


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

![image](https://github.com/user-attachments/assets/201ef232-7074-4b8a-82d9-9ae4c7d9cea0)

