KUBERNETES:
```	Explain the architecture of Kubernetes? ```
Kubernetes is broadly divided into two: 
1.	Control plane (master)
2.	Data plane (worker)
Initially request is sent to control plane the components like 1.api sever,2.etcd, 3.scheduler and control manager
Api server is the main component of Kubernetes where its endpoint is exposed whatever request we made to Kubernetes initially serverd the API server.
When you create a resource or deploy a pod in Kubernetes api server talks to scheduler and scheduler takes the responsibility of identifying the right node for your pod to deploy. once identified api server goes to kubelt and provide the information to it where kubelet is part of worker node, where kublet forwards the request to container runtime or kubelt take the help of container runtime to the application where container runtime is also a part of worker node popular container runtimes are container d, docker shim. Once pod is created on the worker node a service is created and this service is watched by kubeproxy when the service is created and endpoint is created and this endpoint is watched by kubeproxy where as kubeproxy updates the Ip tables of the worker nodes and each of the worker node to enable routing to the pod. Once the resource is created the information is stored in a keyvalue pair data store called ETCD we can consider it as a Kubernetes data base. We can assume all the information of Kubernetes is stored in ETCD and finally there is component in control plane called controller manager. This takes care of running the default containers in Kubernetes such as replicaset controller.
Interview explanation: Kubernetes architecture is classified into two parts
1.	Control plane(master)
2.	Data Plane (worker)
•	In Control plane there are components like api server, scheduler, etcd, controller manager
•	In data plane there are components like kubelet, kubeproxy, container runtime
<img width="708" height="194" alt="image" src="https://github.com/user-attachments/assets/37199365-b53b-4e10-89ec-9eb9e7fedbf5" />

 <img width="472" height="615" alt="image" src="https://github.com/user-attachments/assets/9f3f7a51-5853-4ab0-831e-9dae22d4f4f4" />

     
```	How various components of Kubernetes interacts? ```
When user request to create a pod through kubectl to create a pod, initially the user request is sent to api server, where api server is part of the control plane where api server validates it performs authentication and authorization if user has the right permissions to apply  the api server forward the request to scheduler, where scheduler identifies which is the right node for the pod based on the pod specifications like node affinity, node level selector, any tolerations on the pod and it makes a decision which node is right for the pod  to be scheduled. Api server talks to the kubelet of that particular node and kubelet invokes container runtime and container runtime is needed to run the container this is how pod is running. And once the pod is running api server updated that Information to etcd, if the pod is running fine and It goes down suddenly, there is replica set which takes care of the pod without going down and this is managed by controller manager.
 <img width="890" height="368" alt="image" src="https://github.com/user-attachments/assets/d8132d3f-e28e-442b-98cf-ad537b26b8a5" />


 ```	What is the purpose of services in Kubernetes? ```
 In Kubernetes services takes care of service discovery when pod A can not communicate with Pod B with ip address because the pods are ephemeral in nature(like any time they can go down) in Kubernetes Pod A communicates with service associated with Pod B in and service communicates with labels and selectors.  Request from pod A to Pod B is never terminated, service acts as a middle man this concept is called as service discovery 
Service in Kubernetes also helps in default load balancing, if there are multiple replicas of the pod request are sent to service and service ensure that request are split between two replicas or multiple replicas of the pod it follows round robbin algorithim

	``` Why is hardcoding Pod ip communication is bad 	practice? ```
Kubernetes pods are ephemeral in nature pods can go down because of resource issue, crashloop back off or multiple other reasons so when pod goes down in kubeenetes ip address is changed so when we hardcode the ip address of the pod B in Pod A when pod b goes down and comes back ip address changes and the communication between pod A and Pod B fails
 <img width="598" height="170" alt="image" src="https://github.com/user-attachments/assets/d8b0386b-3c6d-4b52-8310-b94341c33c6c" />

And this might lead to 502 bad gateway errors to the users. To avoid this kubernets uses a concept called service, service is used for service discovery in kubernetes not depends on the ip addres but a concept called labels and selectors through which it identifies the pods

```	What are the types of services in Kubernetes? ```
In Kubernetes, the type of service defines the scope of the Kubernetes service.
When you create the type cluster ip the Kubernetes cluster only accessed in internal cluster. Any pod within the Kubernetes cluster irrespective of namespace can access the service with cluster ip
Nodeport: Kubernetes exposes special port on Kubernetes nodes can access the service through node ip:port  that is exposed 
Load balancer: external or public access to Kubernetes, it only work on  Kubernetes clusters that has cloud control managers running on it.
Special services:
External name: it enables external access to DNS names, we can use external dns name to access pods in the cluster
Head less service: when we define cluster ip to none a head less service is created in Kubernetes, it is used for stateful sets or dns discovery  mostly statefull sets such as for data base

``` 	What are labels and selectors in Kubernetes? ```
In Kubernetes labels are key value pairs that are added to the metadata of the resources for identification or grouping of the resources.
Where as selectors are used for querying purpose services and replica sets in Kubernetes
Using selectors we can identify pods based on labels
 <img width="975" height="372" alt="image" src="https://github.com/user-attachments/assets/0a14b25d-809e-46f4-8dcf-0c574fbdc3fa" />

 Kind: service/ replica sets uses selectors
Kind: Pod uses labels
Selectors used for querying the pods using the label associated with the pod

	``` What would you recommend Nodeport service or Load Balancer service and why? ```
Nodeport service is only accessible to nodeport ip:port for the port that is exposed on the node ip on accessible to the people who are having Kubernetes nodes if created in VPC then people who have  access to VPC can have access to the node
Load balancer: enables external access to the service any one can access the IP (Public) load balancers are available in Kubernetes cluster which have cloud control manager (CCM)enabled in other services without the CCM if we create the load balancer it will act as nodeport service only cannot access for external.
```	How Kubernetes service is related to kube proxy? ```
In Kubernetes primary purpose of services is discovery where communication between the pod A to pod B
Not with the ip address they communicate with a concept of services using labels and selectors
 <img width="613" height="223" alt="image" src="https://github.com/user-attachments/assets/f89171cc-a3ee-4fc5-bcc1-aab9866e6f86" />

When service is created suppose created a service of cluster ip  and the ip address of the service is 10.90.7.5, Kubernetes creates a endpoint for the pods suppose have replicas of it end points will have the information of that two pods
And this endpoints are observed by kubeproxy, kubeproxy  in default mode updates the iptables when the request comes to 10.90.7.5, then it will forward the request to two replica sets of the pod. With in the cluster when someone access the service ip address the the request is forwarded to pods where the pods observed by kubeproxy like this kubeproxy and services works together 
 <img width="624" height="448" alt="image" src="https://github.com/user-attachments/assets/e4f1cd69-6998-4efc-9334-9dada750bf62" />

1—a Kubernetes service is created labels are associated to the pods and in Kubernetes selectors are applied to the service
2.using selectors service identifies the right pod
3.kubernetes creates endpoints 
4.These end points are read by kubeproxy
5. kubeproxy updates the ip tables 
Because the information is updated in ip tables when request is sent to services requests are routed to replicas of the pods.

 ```	What are the disadvantages of load balancer service type? ```
The main disadvantage of load balancer service type is that it provisions a separate IP for each service. Which can be expensive and inefficient especially in cloud environment

	``` What is headless service in Kubernetes and when did you use it? ```
In Kubernetes headless service is a special service which you create as cluster ip is none, unlike traditional services it does not load balance between the pods , it enables DNS-A records for each of the pod replica so that one pod can talk to the other pod with DNS name.
 <img width="691" height="347" alt="image" src="https://github.com/user-attachments/assets/9092ab30-5b7b-452b-97bc-109d275391e5" />


	``` Can a pod access a service in a different namespace, if yes how ? ```
When you create a service default one is cluster ip which is having least service level, which can access your pod is in same name space, default namespace or different name space. The service can be accessed by cluster ip or DNS name
Name of the service.namespace.svc.cluster.local
Mysvc.default.svc.local
 <img width="975" height="306" alt="image" src="https://github.com/user-attachments/assets/9cd824bd-4042-4cd3-a666-2f30a4ef097b" />


	``` How can you restrict access to the DB pod when only one app is running in the namespace? ```
There is a concept in Kubernetes called network policies 
<img width="214" height="203" alt="image" src="https://github.com/user-attachments/assets/0f31be78-cb35-4304-8064-3cac325d90cd" />

  In a namespace called payments, there are three pods and one database MySQL. Only one pod needs to be communicated with the MySQL database, and the rest of two should be disabled. To achieve this, we have a concept called Network Policies.
     <img width="239" height="153" alt="image" src="https://github.com/user-attachments/assets/0dea0f16-9832-4cac-80f4-f1182460ed89" />   <img width="220" height="161" alt="image" src="https://github.com/user-attachments/assets/2765d71e-4df1-4ea5-a433-b654fe88315e" />
<img width="368" height="365" alt="image" src="https://github.com/user-attachments/assets/8d3fe56b-6c0d-49be-ae7d-21eea572d9ee" />


The DB pod which is having labels of app :DB, whereas the application pod having the labels app: my app,  we need to create a network policy resource where kind is network policy. In this, we should block the access to the pod. PolicyType is ingress means any incoming requests for this pod network policy is applied and the network policy is only allow from app: myapp

 ```	Explain the deployment strategy that you follow in your organization? ```
There are different deployment strategies: 
1.	Canary
2.	Blue-green
In canary deployment strategy where 10% of the users have the new version of the application. We can test it by load testing any bugs on the 10% of the users and increasing it to 20%, 30%, and 50%, and finally complete 100% of the users.
To implement this deployment strategy, we need: 
1.	A new ingress
2.	A new service
3.	A new version of the pod
We need to add an annotation to the new ingress that 10% of the traffic should route to the new ones, and it is up to the load balancer. Like 90% of the traffic is going to the old ingress that is old service, and the remaining 10% to the new service after completing the 100% I will delete the old ingress, service and pod

	``` Explain the rollback strategy that you follow in your current organization? ```
We have a deployment strategy canary deployment where we rollout new version of the application where 10% of users in the intal and gradually increase to 100% still if there is an issue in the production we use a roll out strategy
As we follow gitops approach all our files are available in git repository 
Deployment file, config mapp, secretes, and helm charts, as our helm chart is also available in  as a rollback strategy I will roll back the helm chart to previous version when change is made in the git repository argocd is watching the repository when the change is made in git because the autosync is on it will automatically pick up and deploy the version to the helm chart

	``` Design a solution to avoid rollbacks? ```
We we're already using a canary deployment strategy where 10% of the users release the new version of the application. We run few tests on it, before going 100% live we take the feedbacks on different levels with 10-20% and 20-30% respectively. Running load test once 100% we are almost sure that there is no issue in production, if we face anything, we follow the GitOps approach of rolling back the Helm chart to the previous version. Then Argo CD picks up the old version and deploys it to Kubernetes cluster
 <img width="648" height="359" alt="image" src="https://github.com/user-attachments/assets/f9901f45-c822-45fb-95ab-337e69a70ce4" />


	``` Explain the deployment strategies that you used in the past? ```
In past, I have worked with Canary and Blue-Green deployment strategies.
Blue-green : the current application is v1, and we want to release the v2 version of the application we try to create a completely new environment for the v2 version of the application we configure the same exact same resource of v1 to the v2 version as well like memory, CPU. And we point the load balancer to the new version of the application. Users will continue to use the new version, incase any issues that was not caught in dev and stage we point that to old version this deployment is called blue-green strategy  
<img width="383" height="300" alt="image" src="https://github.com/user-attachments/assets/4b0036f7-95d7-466e-9939-d7780f687b92" />

Canary: new version is gradually rolled out to the users, suppose there are two versions, v1 and v2, only for certain uses the new version is released and if no issues by the users eventually released to more number of users and gradually to 100% of the users 

Bule- green is costly compared to canary and in canary where some of the users having new version and some are using old version.

	``` Explain rule of core DNS in Kubernetes? ```
CoreDNS is the default DNS server in Kubernetes, It takes care of of domain to ip address translation in Kubernetes 
Example: pod wants to communicate with payments service with in environment variables or associated config mapp you hardcode the DNS name like payments.default.cluster.local then it translates to ip address using a DNS called coreDNS.

	``` A Devops engineer tainted a node to “noschedule” can you still schedule a pod?``` 
Taints: taints means marking a node if no schedule then no pod is scheduled to this node
Yes we can schedule a pod if it is tainted as no schedule also with tolerations, we can apply tolerations to a pod in pod configuration so that pod can be deployed on tainted node 
<img width="602" height="25" alt="image" src="https://github.com/user-attachments/assets/93aca2fe-cfb8-4838-a2f7-b1cfe0f5cafe" />

 <img width="434" height="448" alt="image" src="https://github.com/user-attachments/assets/4edbae15-f3b1-4b34-a91f-ffa5172e0ad2" />

 
basically on the node app:NoSchedule taint an exception is provided. where we need to add tolerations in spec and key : app  operator equal and effect is NoSchedule 
in other case there are ARM node : node with arm processor only few of the applications can run on that arm processer node so with in the pod we apply tolerations we can schedule the pod Noschedule.

	``` Pod is  struck and crash loop backoff  what steps will you take? ```
CrashLoopBack is not an error it is a state in Kubernetes for some reasons your pod is continuously crashing 
1.Will check Kubectl log pod, if application is crashing  then there is thread dump or stack dump which will share with the developers 
2. kubectl describe pod to see any signs of error
3.liveness probe as well 

	``` What is the difference between the liveness probe and readiness probe?```
Liveness in Kubernetes is used to check the container or application running in pod alive or not. As load balancer sends requests to the healthy virtual machines. Similarly in Kubernetes liveness probe is configured for a container in the pod configuration
 <img width="461" height="408" alt="image" src="https://github.com/user-attachments/assets/156b9227-17cd-433a-9e4b-33a8633767d5" />

When the application is built they built an end point called /healthz
So when the request is made to the endpoint /app/health, it shows whether it is live or not and the application is running or not
In Kubernetes, we provide the endpoint path /healthz  and the port number 8080 and we also tell it to try it for every 5 seconds, that is periodic seconds. And if the health check does not return 200 or success code, then Kubernetes will automatically restart the pod or containers this will avoid manual restarts


Readiness probe.: This will not check if the application is live or not. This will check whether the application is ready to accept traffic or not. Sometimes your application might be in a running state but not able to take traffic. Means suppose a pod consists of a backend that was live and running. There is a request to the backend service to fetch some data from the database, but the database is not up. So it is not in a a state of readiness to avoid this while configuring the pod, we have something endpoint as /ready it will send a ping request to db and checks whether it is up or not.  but when the readiness probe fails, the pod is not restarted, our container doesn't run to crash loop back will see readiness probe failed 
 <img width="214" height="105" alt="image" src="https://github.com/user-attachments/assets/aa0a4813-ec47-46fd-b822-62e60b2384bf" />

Liveness probe and readiness probe are the two probes that are configured in the pod definition.
Liveness probe checks whether the container is alive if not the Kubernetes will restart the pod.
Readiness probe checks the container is ready to serve traffic or not. If not, it will not restart the pod. It will remove the service endpoint from the pod, so that the pod won't be receiving traffic.

	``` Explain the difference between Ingress and Load Balancer service types? ```
Both Ingress and LoadBalancer service types are used to expose the application to the external world. Or to allow public access to the application
In Load Balancer for each service to expose to external world a Load Balancer is created so for each service, a load balancer is created, so that it is very expensive and the Load Balancer is not in our control, it is managed by the Cloud Control Manager.
 <img width="427" height="163" alt="image" src="https://github.com/user-attachments/assets/ef4e817c-37b8-48d5-9867-461e5d98bd32" />

Ingress exposes the application to the external world, but it provides you with a lot of configuration options. For multiple services we can use same ingress load balncer where we define target groups and the request is routed to multiple services. And we can customize it and choose the type of load balancing required. Where we can choose the loadbalncing technique also like path, host, weight bases load balancing 
 

	``` Your app works with ClusterIP but fails with Ingress. How do you troubleshoot? ```
1.	Check if the ingress controller is installed or not. because ingress controller is the one which watches the ingress resource and creates the load balancer 
<img width="261" height="232" alt="image" src="https://github.com/user-attachments/assets/c55e110a-17d6-4a6d-8a04-b051de80c027" />

2.	Check ingress controller logs need to see the ingress resource created is being watched by the ingress controller or not
3.	IngressClassName: when there are multiple ingress controllers in Kubernetes to define which ingress controller needs to watch specific ingress resources the ingress resources are setup with a name suppose if we are using nginx ingress controller, we need to define that ingress class name in the nginix resource and also in the Ingress controller, the same class.
4.	If there is no problem in the above three, then there might be a problem in the yaml file that we wrote. Wrong service or port  as backend in ingress


	``` Why do I need to set up an ingress controller after creating ingress? ```
Ingres resources similarly of pod deployment, we can create the ingress also typically kind is ingress, no controller is watching the ingress resource we need to create the ingress controller which will watch the ingress resource and create the load balancer
When we create a deployment manifest the replica set is watching the pods, and creating them if down, with  ingress controller also watching the ingress resource and creates the load balancer or configure the load balancer.
Different ingress controllers :nginix, cong,f5

	``` We have an in-house load balancer can we use Ingress with our load balancer? ```
Yes the how the ingress controllers like nginx,f5 are proving the ingress controller if the company wants to use there own load balancer they need to develop there own ingress controller  that is a Kubernetes controller which is easy because there are multiple frameworks like controller runtime, operator sdk using this will devlop a ingress controller which will  watch the ingress resource.
 
<img width="938" height="319" alt="image" src="https://github.com/user-attachments/assets/9500d310-a08a-49cb-a04c-e0cc62fb296a" />




	``` Your deployment had replicas 3 but only one pod is running what could be go wrong? ```
As one of the pod is running we can eliminate common Kubernetes issues like 
Image not found, image pull secrete, crash loop backoff
The most possible issue is lack of resource, nodes in that cluster does not have enough resources, suppose nodes have 2 cpu but pods requires 4 cpu 
Other possible reason is nodes might be tainted, so If the node is tainted so pod can not be scheduled until applied the toleration
1.	As a first step I will look into the pod configuration or deployment configuration will look is there any resource(cpu, memory) request and tolerations on pod.
Resource request checking is whether the pod has huge requests like 2 cpus, 4 cpus 6 cpu similarly ram 
2.	Will check the Kubectl describe  on deployment if  pods are not being scheduled because of the resource reason if 0/3 nodes available to schedule this pods then it is resource issue. If the issue with resource then I can add a new node or configure cluster autoscaler or carpenter, if it is issue with the resources you already have nodes but those nodes are tainted I will add toleration to the pod, if toleration can not be added then I will add new node to the cluster
3.	If nothing with kubectl describe then kubectl get events because the  events maintain the historical data of Kubernetes issues may be in events I can check the error message

	``` What is toleration in pods? ``` 
Key Difference
•	Taint = Node’s restriction (gatekeeper).
•	Toleration = Pod’s permission (pass).
•	A Node can have multiple taints; Pods need tolerations to "pass" those gates
Taint
•	Applied on a Node.
•	Purpose: repel Pods from that Node unless the Pod explicitly tolerates the taint.
•	Defines “which Pods should NOT run here unless allowed.”
Toleration
•	Applied on a Pod.
•	Purpose: allows Pods to be scheduled on Nodes with matching taints.
•	Defines “this Pod is okay to run on a tainted Node.”

	``` Your pod mounts a config map, but the changes to the configMap are not reflected? ```
 <img width="367" height="211" alt="image" src="https://github.com/user-attachments/assets/a3b818fd-0723-48f1-9b31-29a6baf1bad5" />

There are two ways of reading data from a config map on to the pod 
1.	Pod can read the data from config map through ENV variable 
Any data changed in config map read through ENV variables can not be reflected in the pod unless pod is restarted because in container env variable cannot be updated without restarting.
2.	A pod can mount config map as a volume 
Config map as volume any data change to the data of the config map is automatically updated because this is reading the data from the disk

	``` Explain how node affinity works and when will you use it? ```
in Kubernetes, Node Affinity is a way to influence the pods' scheduling. it tells Kubernetes which node a pod prefers or requires to run on based on labels assigned to the nodes
suppose in a Cluster, there are three nodes: 
1.	One is GPU
2.	Two others are CPU 
application of AI/ML needs to be run only on the GPU node, then scheduling with a matching node is required. Through node lables
 <img width="406" height="111" alt="image" src="https://github.com/user-attachments/assets/85498232-d365-4732-9299-fe69945540b8" />

node affinity is modern way of nodelabelselector
there are two options in node affinity:
1.RequiredDuringSchedulingIgnoredDuringExecution  this is a hard rule this tells Kubernetes that pod can be scheduled on on the matching node – node according to the label
2. PreferredDuringSchedulingIgnoredDuringExecution this works as node label selector as this is not mandatory preferred
 <img width="651" height="434" alt="image" src="https://github.com/user-attachments/assets/122514d1-6408-4714-9c20-c66f53e54d79" />

Similarly how a service selects the pods

	``` What is the difference between node affinity and node label selector? ```
Node-Affinity is the modern version of Node-Label Selector where it comes with hard affinity and soft affinity.
note selector is a simple way of assigning pods to the nodes through key=value.
now definitely does the same but with more control including more conditions and soft and hard rules

	``` What is container runtime in Kubernetes? ```
Container runtime is available in all the worker nodes of a cluster. suppose when the user makes a request to create a pod, the request initially goes to the API server it perform authentication and autheriztion, and the API, with the help of the scheduler, identifies the right node and once the right node is identified, it talks to the kubelet where the kubelet invokes the container runtime. It not only runs the container it runs series of steps 
1.	Pulse the image that is specified in the pod specification from container registry  (docker hub)
2.	Once the image is pulled, it creates a Linux namespace, cgroups, and other kernel requirements.
3.	And finally it runs the container within the POD
Popular container runtimes are cri-o, container d, runc
