# Nexus-installation-on-kuberntes
**Step 1: Create a namespace called devops-tools**

kubectl create namespace devops-tools

**Step 2:  Create a deployment.yaml file**.

vi deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nexus
  namespace: devops-tools
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nexus-server
  template:
    metadata:
      labels:
        app: nexus-server
    spec:
      containers:
        - name: nexus
          image: sonatype/nexus3:latest
          resources:
            limits:
              memory: "4Gi"
              cpu: "1000m"
            requests:
              memory: "2Gi"
              cpu: "500m"
          ports:
            - containerPort: 8081
          volumeMounts:
            - name: nexus-data
              mountPath: /nexus-data
      volumes:
        - name: nexus-data
          emptyDir: {}
		  
**Step 3: Create the deployment using kubectl command**.

kubectl create -f deployment.yaml

Check the deployment pod status

kubectl get po -n devops-tools

**Step 4: Create a service.yaml file**

vi service.yaml

apiVersion: v1
kind: Service
metadata:
  name: nexus-service
  namespace: devops-tools
  annotations:
      prometheus.io/scrape: 'true'
      prometheus.io/path:   /
      prometheus.io/port:   '8081'
spec:
  selector: 
    app: nexus-server
  type: NodePort  
  ports:
    - port: 8081
      targetPort: 8081
      nodePort: 32000
	  
Check the service configuration using kubectl.

kubectl describe service nexus-service -n devops-tools	  

Step 1: Create a namespace called devops-tools

kubectl create namespace devops-tools

Step 2:  Create a deployment.yaml file.

vi deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nexus
  namespace: devops-tools
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nexus-server
  template:
    metadata:
      labels:
        app: nexus-server
    spec:
      containers:
        - name: nexus
          image: sonatype/nexus3:latest
          resources:
            limits:
              memory: "4Gi"
              cpu: "1000m"
            requests:
              memory: "2Gi"
              cpu: "500m"
          ports:
            - containerPort: 8081
          volumeMounts:
            - name: nexus-data
              mountPath: /nexus-data
      volumes:
        - name: nexus-data
          emptyDir: {}
		  
Step 3: Create the deployment using kubectl command.

kubectl create -f deployment.yaml

Check the deployment pod status

kubectl get po -n devops-tools

Step 4: Create a service.yaml file

vi service.yaml

apiVersion: v1
kind: Service
metadata:
  name: nexus-service
  namespace: devops-tools
  annotations:
      prometheus.io/scrape: 'true'
      prometheus.io/path:   /
      prometheus.io/port:   '8081'
spec:
  selector: 
    app: nexus-server
  type: NodePort  
  ports:
    - port: 8081
      targetPort: 8081
      nodePort: 32000
	  
Check the service configuration using kubectl.

kubectl describe service nexus-service -n devops-tools	  

**Step 5: Now you will be able to access nexus on any of the Kubernetes node IP on port 32000 **

First list the pods and get the nexus pod name.

kubectl get pods -n devops-tools
 

First list the pods and get the nexus pod name.

kubectl get pods -n devops-tools
