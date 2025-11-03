Observability
 ``` 	What is the difference between monitoring and observability? ```
When Monitoring systems ran to an error where cpu/ memory/ disc issues they have dashboards through alerting systems 
Why the system down or why the error occurred, observe it uses logs, metrics, and traces to find the current state of the system.

	``` How to emit custom logs and metrics in your application? ```
Log at info level  Log at error level  log at debug level there will be frame works for the all the languages in java log4j for logging 
For the custom metrics using Prometheus client  instrumentation we will get the metrics
As a vendor neutral and  opensource we use open telemetry

	``` What kind of matrix do you work with in your project? ```
In our current organization we use Prometheus
For infrastructure we use node exporter to scrape this metrics 
node_cpu_seconds_total- for cpu usage per core
node_memory_MemAvailable_bytes – for free memory
node_disk_io_time_seconds_total -Disk IO latency
node_network_receive_bytes – network traffic
 <img width="469" height="108" alt="image" src="https://github.com/user-attachments/assets/e907b2d7-b1e0-435e-9739-f6b3e36962f2" />

We also scrape metrics related to kubelt for container related metrics, we also scrape metrics from API server just to know the state of the API we use kubestate metrics to get this information
 <img width="409" height="77" alt="image" src="https://github.com/user-attachments/assets/6796aada-ce16-49db-9ccb-ddd743b30d1e" />

Metrics related to application.
 <img width="500" height="133" alt="image" src="https://github.com/user-attachments/assets/c1f7a9f4-d441-45e0-bd85-ce3a8106aa56" />


	``` Have you worked on observablity? If yes, explain what you did? ```
Observability is the ability to understand internal state of the system using the internal system we can get metrics,logs,traces
Suppose a payment metric is down; we'll be creating a custom metric to analyze when it was down.
Logs are used to information, debug, and error
All the logs from the payments are pushed to Elastic ELK and visualization through Kibana.
To visualize, like in CPU, when the payments metric takes more memory or CPU, we need a dashboard.
With Prometheus(node exporter) fror cpu memory and others  and grafana for visualization 
For custom metrics we use open telemetry 
Tracing we use jaeger—if there is any latency involved when the payment is connecting to the database or other microservices requesting it back. We'll be checking through tracing.

	``` What is the difference b/w metrics log and traces ```
Logs are used to capture what happened with the application. Time stamp often used to debug and troubleshoot the issues with application
Metric provides historical information. For example if we want total number of http requests 
Or it can be total number of POD resarts 
Traces helps you understand the journey of the request, when user request the application when services speaking with other service everything will be traced. Traces are used to any latency is involved or bottleneck 
 <img width="697" height="408" alt="image" src="https://github.com/user-attachments/assets/d151aaa8-98e0-485e-832c-c0750157357d" />


	``` What is the difference between push and pull based monitoring? ```
Prometheus, a popular tool for monitoring, works on a PULL-based approach. Suppose there is an application which possesses /metrics endpoints. Where Prometheus scrapes the metrics from the /metrics endpoint or through the Kube state endpoint. If we use node exporter Prometheus scrape metrics from node 
In pull based, it pulls the Metrics from the app or node.

 
<img width="716" height="186" alt="image" src="https://github.com/user-attachments/assets/ec24ba01-be3b-43f3-8611-e7200d5e569d" />

Stats , telegraph are examples which follow push based approach where we forward the metrics to them it is the responsibility of the app or target node to choose the metrics to them
 <img width="769" height="38" alt="image" src="https://github.com/user-attachments/assets/b6fda9dd-93fc-4654-9b81-9dcb0119cb11" />


	``` Which tool have you used to build the observability stack?```
Used Dynatrace and explain reg it. Or 
	``` User reports slowness in the app logs don't show error, CPU is good. Fix it? ```
I will recheck the logs in debug mode and also check cpu and memory from Grafana
If everything is okay I will check average request time (promthesues or garafana)
http.requests.duration
request type currently taking 
with jeager will trace out the request most of the times it will be backend issue connecting with database or connecting with other microservice
will identify the distributed tracing journey and identify the latency

	``` How do you trace a service across multiple microservices in a Kubernetes cluster ```
 When the request goes through multiple microservices,  to trace it through each of the microservices, we call it tracing through all the microservices the process we call it distributed tracing.
We use tools like jeager  it is popular tool for distributive tracing.
Jeager collects the information when the request is originated a trace id is created the trace ID goes through one microservice to another microservice, likewise it reaches one hop to another hop and gets back.
The tracing ID transfers from one microservice to other microservices. The tracing SDK will create a span. These spans are created to check the issue with db or cart service or any particular service when jeager collects all the spans we can see clearly at which point struck the key part of this tracing SDK that application uses.
 ```	Pod crashes randomly with OOM(out of memory) killed. How do you identify and fix this? ```
 
<img width="975" height="361" alt="image" src="https://github.com/user-attachments/assets/d2ec7efb-d8cc-471d-a6c0-dd939b9fd451" />

Alert tuning approach—
SLO based alerting 
<img width="975" height="419" alt="image" src="https://github.com/user-attachments/assets/c63b3945-2e5d-4a3e-9f41-2f8c31295eac" />

<img width="975" height="238" alt="image" src="https://github.com/user-attachments/assets/36461eea-0a6f-470c-a946-228e49e54932" />


 <img width="534" height="298" alt="image" src="https://github.com/user-attachments/assets/7f7ef3f4-4b45-4fef-89af-ec5f12707067" />




 
 
