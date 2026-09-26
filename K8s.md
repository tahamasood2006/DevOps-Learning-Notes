## *Kubernetes Architecture (Diagram Imp for interview)*

!image.png

1. We have 1 master node and other nodes which are called worker nodes 
2. Master Node is like a Head Quarter, it doesn’t do anything, all its task is to give other worker nodes tasks and manage them. 
3. Our Docker Container runs inside the worker node
4. API SERVER is used to communicate with worker and master node
5. Scheduler work is to schedule pods to run, they are responsible for running pods
6. etch is like a db which stores all the information about the pods Eg: when the pod was created with what configs it was created
7. Controller Manager overseas everything like our nodes are working pods are up, all services are working or not 
8. Kubelet is like a manager for the worker node, sees whether our pods inside it working fine 
9. Kube-proxy is used if our outside user wants to interact with our worker node  
10. Kubectl is used to give instructions to our master node eg: tell me how many containers are running…
11. CNI network is used to make communication happen in between our worker node and master node

## Ways to Create a Cluster:

<aside>
💡

Can see here how to setup for each type: 

https://github.com/LondheShubham153/kubestarter

</aside>

## Type 1: Kubeadm

In this we make one server a master node and other servers a worker node and joins them through kubadm to make a K8s cluster

## Type 2: Minikube

Usually when we have a small local or single ec2, can be seen in github repo provided above. 

## Type 3: KIND Cluster

⇒ First Download KIND and kubeadm and other stuff

Now, we will create a yaml manifest file to create a K8’s CLUSTER

#### Example Cluster Creation Yaml File:

```jsx
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4

nodes:
- role: control-plane
  image: kindest/node:v1.31.3

- role: worker1
  image: kindest/node:v1.31.3

- role: worker2
  image: kindest/node:v1.31.3

- role: worker3
  image: kindest/node:v1.31.3 
  
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
      
  - containerPort: 443
    hostPort: 443
    protocol: TCP

```

Now, we are done with manifest file to create a cluster, will run some commands to create a cluster through our manifest file

COMMANDS:

1. kind create cluster —name=ourfirstCluster —config=manifestfileName.yml

THIS WILL CREATE A CLUSTER FROM A MANIFEST FILE

1. Kubectl cluster-info —context clusterName
2. kubectl get nodes ⇒ will give you all the info about all nodes inside a cluster 

## Type 4: EKS/AKS

## K8s Concepts:

### Namespace:

It is a group which has all the resources inside it and runs in isolation. 

!image.png

COMMANDS:

1. kubectl get namespaces ⇒ returns all the namespaces inside cluster 
2. kubectl get pod —namespace=NameofnameSpace ⇒ will return components inside the specified namespace
3. kubectl get ns ⇒ will return all of our namespaces

CREATING A NAMESPACE:

1. kubectl create namespace OurFirstNameSpace ⇒ this will create a new namespace of the specified namespace

### METHOD 2, THROUGH MANIFEST FILE:

```jsx
kind: Namespace
apiVersion: v1
metadata: 
  name: firstnamespace

```

TO RUN THIS MANIFEST FILE WE WILL RUN THIS COMMAND:

1. kubectl apply -f ourNameSpaceManifestFileName.yml

## POD Commands:

A POD can be created, deleted and paused 

1. kubectl get pods -n nameOfNameSpace ⇒ will give the pods running inside the specified nameSpace 
2. kubectl get pods -n nameOfNameSpace -o wide ⇒ will give the pods running inside the specified nameSpace along with on what worker node they are running
3. kubectl delete pod PodName ⇒ will delete the pod
4. kubectl run podName —image=imageFromDockerHub —namespace=SpecifiedNameSpace ⇒ 

## POD Cycles OR States(imp for interview):

- Pending
- Container Creating
- Running
- Terminating
- Completed

### METHOD 2, THROUGH MANIFEST FILE:

```jsx
kind: Pod
apiVersion: v1

metadata:
  name: firstpod1
  namespace: firstnamespace 

spec:
  containers:
  - name: nginx
    image: nginx:latest
    ports:
    - containerPort: 80

```

TO RUN THIS MANIFEST FILE WE WILL RUN THIS COMMAND:

1. kubectl apply -f ouFirstPodFileName.yml

TO ACCESS THE TERMINAL INSIDE THAT POD RUN:

1. kubectl exec -it pod/PodName -n NameSpaceName — bash 

## Info & deletion of Pod:

1.  kubectl describe pod/PodName -n  firstnamespace ⇒ this will tell number of things about our pod
2. kubectl delete -f firstPod.yml ⇒ will delete the Pod created by the yaml file , will not delete the yaml file itself.
3.  kubectl logs enterPodName -c enterContainerName -n specifyNamespace ⇒ will give logs of the specified container inside pod

 

## What is Replication Controller?

It manages the replicas of the pod

## Deployment in K8s:

We make deployments to make our Pod scalable,  It makes the pod scalable by adding replicas of Pod, allows ‘rolling update’,  if we update any change in any pod, it pushes that change in every pod but makes sure one pod is available atleast all the time.

RAW EXAMPLE OF A DEPLOYMENT MANIFEST

```jsx
kind: Deployment
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

COMMAND:

1. kubectl get deployment -n SpecifiedNameSpace ⇒ will give the running deployments
2. kubectl scale deployment/deploymentName -n SpecifiedNameSpace --replicas=4 ⇒ will scale the deployment, means will increase the number of replicas of the pod

## Changing the Pod Image after pod& deployment creation :

> DOING FOR NGINX FOR EXAMPLE
> 
1. kubectl set image deployment/deploymentName -n specifiedNamespace ContainerNameSpecifiedIndeploymentFile=nginx:1.23.3

## ReplicaSet in K8s:

Same as Deployment but if we update any change in any pod, it pushes that change in every pod instantly which causes some downtime sometime  

RAW EXAMPLE OF A ReplicaSet MANIFEST

```jsx
kind: ReplicaSet
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

## Difference b/w ReplicaSet & Deployments (imp for interview)

just read both above will understand the difference

## DaemonSets in K8s:

DaemonSets does the same job of replicas but the difference is that they makes sure that every worker node has atleast one 1 pod running inside it. Eg: if have 3 worker nodes we will have atleast 3 pods , can have more than 3 but atleast 3.

> RAW EXAMPLE OF A DaemonSet MANIFEST
> 

```jsx
kind: DaemonSet
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:

  selector:
    matchLabels:
      app: nginx

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80

```

## Jobs in K8s:

If we want our container to do any task only Once Like Taking a Backup, Patching, Updating the system, But dont want that task to be running 24/7 just wants that the task should run and kills itself . 

```jsx
kind: Job
apiVersion: batch/v1

metadata:
  name: firstjob
  namespace: firstnamespace
spec:
  completions: 1
  parallelism: 1
  
  template: 
    metadata:
      name: demo-job-pod
      labels:
        app: batch-task
    spec:
      containers:
      - name: ourcontainerforjob
        image: busybox:latest
        command: ["sh","-c", "echo Hello World! && sleep 10"]
      restartPolicy: Never
```

COMMANDS:

1. kubectl get jobs -n specifyNamespace ⇒ will give all the jobs
2. kubectl logs pod/PodName -n specifyNamespace ⇒ will give logs of the pod

## CronJob in K8s:

Cron jobs is same as jobs but just running it on a schedule

> See cron-guru website for commands
> 

```jsx
kind: CronJob
apiVersion: batch/v1
metadata:
  name: firstcronjob
  namespace: firstnamespace
spec:
  schedule: "*/2 * * * *"
  
  jobTemplate:
    spec:
      template:
        metadata:
          name: ourfirstcronjobpod
          namespace: firstnamespace
          labels:
            app: minute-cronjob 

        spec:
          containers:
          - name: croncontainer
            image: busybox:latest
            command: 
            - sh
            - -c 
            - >
              echo "This type of command is just multi line command" &&
              echo "see this is multiline" ;
          restartPolicy: Never

```

COMMANDS:

1. kubectl get cronjob -n specifyNamespace ⇒ will give cronjobs

## Persistent Volumes in K8s:

Persistent Volume just creates a volume to use it we need Persistent Volume Claim

Theoretical concept is same as it is in Docker. 

```jsx
kind: PersistentVolume
apiVersion: v1
metadata:
  name: firstpersistentvolume
  namespace: firstnamespace
  labels:
    app:local
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain 
  storageClassName: local-storage
  hostPath:
    path: /mnt/data

```

### What does a StorageClass mean ?

It tells Kubernetes things like:

> "When someone requests storage, create it using this storage provider with these settings."
> 

COMMANDS:

1. kubectl get pv ⇒ will give persistent volumes 

## Persistent Volumes Claim (PVC )in K8s:

```jsx
kind: PersistentVolumeClaim
apiVersion: v1
metadata: 
  name: firstpersistentvolumeclaim
  namespace: firstnamespace

spec:
  accessModes:
   - ReadWriteOnce
  resources:
    requests:
       storage: 1Gi
  storageClassName: local-storage 

```

COMMANDS:

1. kubectl get pvc ⇒ will give persistent volume claims

WE HAVE CREATED A PV AND ALSO A PVC FOR IT NOW WE NEED TO GIVE IT .

WE WILL MAKE A DEPLOYMENT WHOSE DATA WOULDN’T GET DELETED BY USING OUR PV & PVC.

HERE IS ALL 3 FILES PV MANIFEST PVC MANIFEST AND DEPLOYMENT MANIFEST

```jsx
kind: PersistentVolume
apiVersion: v1
metadata:
  name: firstpersistentvolume
  namespace: firstnamespace
  labels:
    app: local
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Retain 
  storageClassName: local-storage
  hostPath:
    path: /mnt/data

```

```jsx
kind: PersistentVolumeClaim
apiVersion: v1
metadata: 
  name: firstpersistentvolumeclaim
  namespace: firstnamespace

spec:
  accessModes:
   - ReadWriteOnce
  resources:
    requests:
       storage: 1Gi 
  storageClassName: local-storage 

```

```jsx
kind: Deployment
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        
        volumeMounts:
        - mountPath: /var/www/html
          name: firstpersistentvolumeclaim

      volumes:   
       - name: firstpersistentvolumeclaim
         persistentVolumeClaim: 
           claimName: firstpersistentvolume

```

#### Things to know when learning about storage, PV, PVC:

- If you are using hostPath, your data is stored directly on the worker node’s local disk.
- hostPath volume mounts a directory from the worker node's file system into the pod.
- If Pod is rescheduled to different Node, data won’t be available on the new node
- Or if your node gets crashed or cluster itself gets crashed data is lost
- To avoid these all we use external storage like AWS EBS, NFS etc - but for development and learning purpose hostPath is good

## Services in K8s

Services are used to make our deployments accessable for the outside world  

```jsx
kind: Service
apiVersion: v1
metadata: 
  name: firstservice
  namespace: firstnamespace
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP

```

THIS type: ClusterIP has also other types of it, search for it  … IMPORTANT 

> **containerPort = where app runs**
> 
> 
> **targetPort = where Service sends traffic**
> 
> **port = where Service receives traffic**
> 

COMMAND:

1. kubectl get all -n specifyNamespace ⇒ will give all deployment,service, jobs pv all
2. kubectl port-forward service/serviceName -n  specifyNamespace 80:80 --address=0.0.0.0
⇒ this command will expose our port 80 

In this 80:80, this first 80 is our localhost port which we can freely choose and set to any but the second 80 after colon is our service yaml file port: value

## Stateful Set in K8s

Stateful sets needs PV and Volume Claim Template inside it. 

Its service.yml remains headless.

IF YOU delete a pod in this it will automatically create a new one of it with the same name previous pod had.

```jsx
kind: StatefulSet
apiVersion: apps/v1
metadata: 
  name: mysqlstateful
  namespace: mysqlns

spec:
  serviceName: mysql-service
  replicas: 3
  
  selector:
    matchLabels:
      app: mysql-stf
  
  template:
    metadata:
      labels:
        app: mysql-stf
    
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
    
        ports:
        - containerPort: 3306
    
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: root
        - name: MYSQL_DATABASE
          value: devops 
    
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
      
  volumeClaimTemplates:
    - metadata: 
        name: mysql-data

      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi

```

```jsx
kind: Namespace
apiVersion: v1
metadata:
  name: mysqlns
```

```jsx
kind: Service
apiVersion: v1
metadata:
    name: mysql-service
    namespace: mysqlns
spec:
  clusterIP: None
  selector:
    app: stf

  ports:
  - name: mysql
    protocol: TCP
    port: 3306
    targetPort: 3306 
```

## ConfigMaps in K8s:

Here we store our variables and can change variables made in Deployments or Stateful from here

HERE YOU CAN SEE A WORKING EXAMPLE OF STATEFUL + CONFIGMAP

COMMANDS:

> kubectl apply is needed on configmap file too
> 
1. kubectl get configmaps -n specifyNamespace ⇒ will give all configmaps 

```jsx
kind: StatefulSet
apiVersion: apps/v1
metadata: 
  name: mysqlstateful
  namespace: mysqlns

spec:
  serviceName: mysql-service
  replicas: 3
  
  selector:
    matchLabels:
      app: mysql-stf
  
  template:
    metadata:
      labels:
        app: mysql-stf
    
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
    
        ports:
        - containerPort: 3306
    
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: root
        - name: MYSQL_DATABASE
          valueFrom: 
            configMapKeyRef:
              name: mysql-confmp
              key: MYSQL_DATABASE 
    
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
      
  volumeClaimTemplates:
    - metadata: 
        name: mysql-data

      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi
```

```jsx
kind: ConfigMap
apiVersion: v1
metadata:
  name: mysql-confmp
  namespace: mysqlns
data:
  MYSQL_DATABASE: devops 
```

```jsx
kind: Namespace
apiVersion: v1
metadata:
  name: mysqlns
```

```jsx
kind: Service
apiVersion: v1
metadata:
    name: mysql-service
    namespace: mysqlns
spec:
  clusterIP: None
  selector:
    app: stf

  ports:
  - name: mysql
    protocol: TCP
    port: 3306
    targetPort: 3306 
```

## Secrets:

A way to write our env and other variables, mostly same as configmap but just need some base64 encoding here

```jsx
kind: StatefulSet
apiVersion: apps/v1
metadata: 
  name: mysqlstateful
  namespace: mysqlns

spec:
  serviceName: mysql-service
  replicas: 3
  
  selector:
    matchLabels:
      app: mysql-stf
  
  template:
    metadata:
      labels:
        app: mysql-stf
    
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
    
        ports:
        - containerPort: 3306
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi
    
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom: 
            secretKeyRef:
              name: ourfirstsecret
              key: MYSQL_ROOT_PASSWORD
        - name: MYSQL_DATABASE
          valueFrom: 
            configMapKeyRef:
              name: mysql-confmp
              key: MYSQL_DATABASE 
    
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
      
  volumeClaimTemplates:
    - metadata: 
        name: mysql-data

      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi

```

```jsx
kind: Secret
apiVersion: v1
metadata:
  name: ourfirstsecret
  namespace: firstnamespace
data:
  MYSQL_ROOT_PASSWORD: cm9vdAo=
```

TO BASE 64 we can run a command , echo  “anypassword” | base64

## Resource Quotas and limits

This simply means giving our pod min and max resources of memory and cpu to consume

```jsx
kind: StatefulSet
apiVersion: apps/v1
metadata: 
  name: mysqlstateful
  namespace: mysqlns

spec:
  serviceName: mysql-service
  replicas: 3
  
  selector:
    matchLabels:
      app: mysql-stf
  
  template:
    metadata:
      labels:
        app: mysql-stf
    
    spec:
      containers:
      - name: mysql
        image: mysql:8.0
    
        ports:
        - containerPort: 3306
        resources:
          requests:
            cpu: 100m
            memory: 128Mi
          limits:
            cpu: 200m
            memory: 256Mi
    
        env:
        - name: MYSQL_ROOT_PASSWORD
          value: root
        - name: MYSQL_DATABASE
          valueFrom: 
            configMapKeyRef:
              name: mysql-confmp
              key: MYSQL_DATABASE 
    
        volumeMounts:
        - name: mysql-data
          mountPath: /var/lib/mysql
      
  volumeClaimTemplates:
    - metadata: 
        name: mysql-data

      spec:
        accessModes: ["ReadWriteOnce"]
        resources:
          requests:
            storage: 1Gi

```

## Probes:

Probe is a way to check condition of our pod by making a request on it

3 Types of it are:

1. Liveness Probe
2. Readiness Probe
3. Startup Probe 

EXAMPLE OF LIVENESS PROBE: ALSO CAN SET A INTERVAL TIME HERE FOR LIVENESS OR READINESS … 

```jsx
kind: Deployment
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginxcontainer
        image: nginx:latest
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
        
        volumeMounts:
        - mountPath: /var/www/html
          name: firstpersistentvolumeclaim

      volumes:   
       - name: firstpersistentvolumeclaim
         persistentVolumeClaim: 
           claimName: firstpersistentvolume

```

EXAMPLE OF Readiness PROBE:

```jsx
kind: Deployment
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginxcontainer
        image: nginx:latest
        ports:
        - containerPort: 80
        readinessProbe:
          httpGet:
            path: /
            port: 80
        
        volumeMounts:
        - mountPath: /var/www/html
          name: firstpersistentvolumeclaim

      volumes:   
       - name: firstpersistentvolumeclaim
         persistentVolumeClaim: 
           claimName: firstpersistentvolume

```

## Taints & Tolerance in K8s:

We can limit or stop our any worker node from running anymore new pod inside it by using taints

Our Control plane is already tainted that is the reason we cannot run any pod on it.

COMMANDS:

1. kubectl taint node specifyNodeName prod=true:NoSchedule ⇒ will taint our specified worker node and this prod is a key which can be any  
2. kubectl taint node specifyNodeName prod=true:NoSchedule- ⇒ this (-)sign on the last will make our node untainted 

Now, This Below means if any of our node has prod=true:NoSchedule , tolerate it and run on that node simply;

```jsx
kind: Deployment
apiVersion: apps/v1

metadata: 
  name: firstdeployment
  namespace: firstnamespace  

spec:
  replicas: 2

  selector:
    matchLabels:
      app: nginx
  tolerations:
  - key: "prod"
    operator: "Equal"
    value: "true"
    effect: "NoSchedule"

  template:
   
     metadata:
      name: nginx-dep-pod
      labels:
        app: nginx
    
     spec:
      containers:
      - name: nginxcontainer
        image: nginx:latest
        ports:
        - containerPort: 80
        livenessProbe:
          httpGet:
            path: /
            port: 80
        
        volumeMounts:
        - mountPath: /var/www/html
          name: firstpersistentvolumeclaim

      volumes:   
       - name: firstpersistentvolumeclaim
         persistentVolumeClaim: 
           claimName: firstpersistentvolume

```

## Ingress:

Ingress is used to route kubernetes services, we need an ingress controller to do this

There is an nginx-ingress controller available on github . The thing to keep in mind is only that we need to make this for our type of Cluster like we are running KIND , we might be running minikube so just need to tune-it for the type of cluster.   

COMMAND FOR KIND 

> kubectl apply -f https://kind.sigs.k8s.io/examples/ingress/deploy-ingress-nginx.yaml
> 

After this you will see a new namespace, HERE we can see we will have services and pods running inside this newly created namespace, 

WILL CREATE A INGRESS.YML  

```jsx
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: django-nginx-ingress
  namespace: notes-app-ns
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  ingressClassName: nginx 
  rules:
  - http:
      paths:
      - pathType: Prefix
        path: /
        backend:
          service:
            name: django-app-service
            port:
              number: 8001
      - pathType: Prefix
        path: /nginx
        backend:
          service:
            name: nginx-app-service
            port:
              number: 8090
```

After applying this we need to do port forwarding of a service running inside our newly created namespace, so we will run the command to do port forwarding of our service running inside the newly created namespace we created above by a github hosted yaml file

Will see a service of ingress-nginx-controller inside our newly namespace now port forward it.

COMMAND:

1. kubectl port-forward service/ingress-nginx-controller -n ingress-nginx 7070:80 --address=0.0.0.0

When doing ingress type of thing, our this ingress-nginx service makes a namespace and runs a service inside it , we need to do port forwarding of it. 

## HPA & VPA AutoScaling (also KEDA)

In HPA we increase number of replicas of pod and In VPA we increase the resource power of the pod, VPA is generally used with stateful.

We can see our resources usage of pod and nodes by using kubectl top node and kubectl top pod commands but they use something known as metrics to calculate the usage of resource so we need to install it first 

WE NEED to setup metrics server which will run inside our kube-system namespace created by default there. 

TO SETUP METRICS SERVER:

You can search about Metrics server install on KIND or whethever type of cluster you are using 

now we will do some edits to make it run without any problem 

RUN THESE:

1.  kubectl -n kube-system edit deployment metrics-server 

this command will open a file you will need to add the security bypass to deployment under container.args  

add this by using - and then double dash 

  kubelet-insecure-tls 
- --kubelet-preferred-address-types=InternalIP,Hostname,ExternalIP

now after adding this restart the deployment using this command

1. kubectl -n kube-system rollout restart deployment metrics-server

NOW see if our pod in kube-system namespace is running :

1. kubectl get pods -n kube-system

SHOULD SEE SOMETHING LIKE THIS :

“metrics-server-587dc6c4cd-4vz7f” RUNNING

NOW AFTER SETUP WE CAN RUN COMMANDS AND SEE LIKE:

1. kubectl top node
2. kubectl top pod -n specifyNamespace 
3. kubectl get hpa -n specifyNamespace

```jsx
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler

metadata:
  name: notes-autoscaler
  namespace: notes-app-ns

spec:
  scaleTargetRef:
    kind: Deployment
    name: django-app-deployment
    apiVersion: apps/v1
  
  minReplicas: 1
  maxReplicas: 4
  metrics:
    - type: Resource
      resource:
        name: cpu
        target: 
          type: Utilization
          averageUtilization: 90 # can change these key values 

```

## Creating a POD to generate load on our website

COMMAND: 

change this image before — to double dash and this —tty to double dash too

1. kubectl run -i -t-ty load-generatorORanyName —image=busybox -n specifyNamespace /bin/sh

WILL open a terminal type thing here then write

> while true; do wget -q -O-http://0.0.0.0:portnum; done
> 

## VPA:

For Vertical pod autoscaling we need to clone kubernetes/autoscaler from github 

1. git clone https://github.com/kubernetes/autoscaler.git
2. cd autoscaler/vertical-pod-autoscaler
3. ./hack/vpa-up.sh

NOW we have got all setup will create a vpa.yaml file now

```jsx
kind: VerticalPodAutoscaler
apiVersion: autoscaling.k8s.io/v1
metadata:
  name: nginx-vpa
  namespace: notes-app-ns
spec:
  targetRef:
    kind: Deployment
    apiVersion: apps/v1
    name: nginx-app-deployment
  
  updatePolicy:
    updateMode: "Auto"
```

## Role Based Access Control in K8s

## SERVICE ACCOUNT

!image.png

COMMANDS:

1. kubectl auth whoami ⇒ gives information about current user
2. kubectl auth can-i get pods -n specifyNamespace
3. kubectl get role -n specifyNamespace
4. kubectl get serviceaccount -n specifyNamespace 
5. kubectl auth can-i get pods —as=usera -n specifyNamespace 
6. kubectl get rolebinding -n specifyNamespace

```jsx
kind: Role 
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: cluster-manager
  namespace: notes-app-ns
rules:
  - apiGroups: ["","batch","apps","rbac.authorization.k8s.io"] # or ["*"] for giving all access
    resources: ["pod","deployment","service"]
    verbs: ["apply","get","delete","watch","create","patch"]

```

SERVICE ACCOUNT, ALSO WE NEED TO DO BINDING B/W SERVICE ACCOUNT AND ROLE

```jsx
kind: ServiceAccount
apiVersion: v1
metadata:
  name: usera
  namespace: notes-app-ns
  
```

```jsx
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: usera-manager-rolebinding
  namespace: notes-app-ns

subjects:
- kind: User
  name: usera
  apiGroup: rbac.authorization.k8s.io
roleRef:
    kind: Role
    name: cluster-manager
    apiGroup: rbac.authorization.k8s.io
```

## CLUSTER ACCOUNT WITH K8s Monitoring Dashboard Setup:

First need to apply manifest files of K8s dashboard 

1. kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

WILL SEE IT HAS CREATED THESE AND OTHER STUFF 

> (DONT CLICK THESE ARE NOT LINKS, THESE ARE OUTPUT OF UPPER COMMAND)
> 

role.rbac.authorization.k8s.io/kubernetesdashboard created
clusterrole.rbac.authorization.k8s.io/kubernetesdashboard created
rolebinding.rbac.authorization.k8s.io/kubernetesdashboard created

namespace/kubernetes-dashboard created
clusterrolebinding.rbac.authorization.k8s.io/kubernetesdashboard created

---

BUT IT HAS NOT CREATED A SERVICE ACCOUNT ALONG WITH A CLUSTEROLEBINDING, SO we will have to create it

ALSO WHEN CREATING A SERVICE ACCOUNT WE HAVE TO BE CAREFUL ABOUT THE NAMESPACE , THE NAMESPACE OUR UPPER COMMAND HAS CREATED WE HAVE TO ENTER THAT NAMESPACE IN SERVICEACCOUNT MANFIEST

```jsx
kind: ServiceAccount
apiVersion: v1
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
  

```

```jsx
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1

metadata:
  name: admin-user-binding
  namespace: kubernetes-dashboard
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
```

After applying these we can run kubectl proxy but to run it we need some access tokens for our this service account, so lets first get it , this is the command to create a token:

1. kubectl -n kubernetes-dashboard create token admin-user

Now Run :

1. kubectl proxy --port=8001 --address=0.0.0.0 --accept-hosts='.*’

NOW OPEN BROWSER AND HIT THIS URL :

URL : http://0.0.0.0:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/#/login

THEN, AFTER THAT 

FOLLOW THIS:

1. kubectl -n kubernetes-dashboard port-forward svc/kubernetes-dashboard 8443:443

THEN this;

1. kubectl proxy --port=8001

NOW ACCESS THIS ON BROWSER AND PASTE THE TOKEN ACCESS :

URL: http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:443/proxy/

## Init Containers & SideCar Containers

Init container basically runs before our main container and then goes off while our sidecar container runs side by side our main container

INIT CONTAINERS

```jsx
kind: Pod
apiVersion: v1
metadata:  
  name: initcontainerdemo
spec:
  initContainers:
  - name: initcont
    image: busybox:latest
    command: ["sh","-c","echo 'HelloWorld' ; sleep 5; whoami"]

  containers:
    - name: maincont
      image: busybox:latest
      command: ["sh","-c","echo 'HelloMainContainer' "]

```

## CRD (will do later)

## Annotations (remaining)

## Node Affinity (remaining)

## HELM :

Helm is the package manager for Kubernetes. It helps you define, install, and upgrade applications on a Kubernetes cluster, from a single container to an application with many interdependent parts.

Helm packages these related manifests into a single unit called a *chart*, which you can version, share, install, and roll back as one release. 

- **Install and manage applications.** Deploy off-the-shelf applications, such as databases, monitoring stacks, and ingress controllers, from a chart instead of assembling manifests yourself.
- **Package and share your own applications.** Bundle your Kubernetes resources into a chart, version it, and distribute it to your team or the wider community.
- **Configure applications per environment.** Supply different values to the same chart to deploy to development, staging, and production without duplicating manifests.
- **Upgrade and roll back safely.** Move a release to a new version, and return to a previous revision when an upgrade does not go as planned.
- **Manage dependencies.** Declare the other charts your application needs, and let Helm install them together.

## Helm Charts;

A *chart* is a Helm package. It contains all of the resource definitions needed to run an application, tool, or service inside a Kubernetes cluster. Think of it as the Kubernetes equivalent of a Homebrew formula, an apt `dpkg`, or a yum RPM file. To learn how to build one, see the Charts guide.

## Helm Repository;

A *repository* is the place where charts are collected and shared.

## Helm Release;

A *release* is an instance of a chart running in a Kubernetes cluster. You can install one chart many times into the same cluster, and each installation creates a new release with its own release name. For example, if you want two databases running in your cluster, you can install a MySQL chart twice, and each installation is tracked as a separate release.

### Creating an apache from helm

1. helm create apache-helm
2. Do the changes we want in values.yaml 
3. helm package apache-helm
4. helm install anyname apache-helm
5. helm uninstall namegiveIninstall (for deleteing)
6. helm install anyName apache-helm -n anyNamespacewewantTocreate --create-namespace ( to create it with in a new name space) 

NOW, if want to update anything in values.yaml or Chart.yaml we can , and then to update it run these:

1. helm package apache-helm

THEN,

1. helm upgrade (Here: nameEnteredwhile helm install ) ./apache-helm -n specifyNamespace

NOW, WE ALSO CAN ROLLBACK TO OUR PREVIOUS VALUES 

1. helm rollback (Here: nameEnteredwhile helm install )  1 (means the first, if 2 that means secondone)  -n specifyNamespace 

ANOTHER EXAMPLE:

1. helm create node-js-app   

> Need to change the repository: in values.yaml
> 

## Istio Service Mesh

First download it search for it  and then do these after downloading we have to go inside istio bin folder and there will be a file named as “istioctl” have to run this command then mv istioctl /usr/local/bin; 

IMPORTANT FOR INTERVIEW THIS THEORY:

Istio uses envoy which is a sidecar container proxy, 

“Istiod” is the control plane
