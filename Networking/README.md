NETWORKING

```	What is DNS and why is it important? ```
DNS is  like Internet Phone Book like we can’t  remember the ip address so for human readable domain names are created. Translates the ip address to human friendly domain name.

```	Complete flow of OSI model ```
When we are trying to access a website, suppose devopse.com, it will go to http or https, which is called the application layer (Layer 7).

When we are using HTTPS, the encryption takes place through TLS or SSL. It is layer 6. Presentation layer 
If the request consists of cookies, cache, and the login information that will be taken care  layer 5 session layer
Layers 5,6,7 are taken care of by the web browser.
Our request is converted to packets in this layer called the Transport layer. Layer 4
Servers ip address destination is added where the request is routed in layer 3 Network layer
Mac address is added. Suppose the request is sent from your device, the mac address is added for the next hop and to router and similarly next hop is also added in layer 2 Data link layer
Finally request travels through electrics, air, cables and finally reaches physical layer where destination this is physical layer layer 1





```	Explain the difference between forward proxy and reverse proxy ```
Forward proxy: forward proxy sits in front of a client.
Example: in some companies, the websites, YouTube, or Google are not able to access because our requests are going through the forward proxy. Our request will go the forward proxy first and then to internet, forward proxy is a middleman that can block your request or procced the request
   <img width="574" height="225" alt="image" src="https://github.com/user-attachments/assets/3c3af5af-bfa3-4984-b6c8-d06a55dccf60" /> <img width="323" height="298" alt="image" src="https://github.com/user-attachments/assets/001f2b3f-dcfb-45b8-8fde-7ae7ec0deed0" />


Reverse proxy: reverse proxy sits in front of the server.
Coming to reverse proxy, it is completely different. Suppose we have servers in our organization. We deploy the applications in the servers like application 1, application 2, application3 a user from the USA wants to access the application, so there will be a Nginx before the servers. It will be acting as a reverse proxy, allowing users to access the particular service or application they require. Why Nginx? Because in Nginx we can have load balancing, whitelisting, blacklisting.
   <img width="323" height="298" alt="image" src="https://github.com/user-attachments/assets/053858eb-5995-41cd-b474-bce4ad909db3" /> <img width="339" height="326" alt="image" src="https://github.com/user-attachments/assets/78159d25-d560-42be-b37d-9102ddba2cf3" />





```	User reports there is a slowness in the app. How do you approach this problem? ```
As a devops engineer we need to check on multiple issues and give multiple commands depending on the issue we need to go to.

1.	We will run the top command or htop  to check whether any load on the server 
We will see CPU utilization, memory utilization, and disk space. To understand if there is any server suppose if there is any load application will be slow. if this is the issue, I will check all the processes that are running and consuming more load. I will de-prioritize the process or kill the process through PID.
2.	I will check if there is any network latency to check ping or traceroute command ping devopse.com and check how the ping is very quick or very slow. If network latency is the issue, we can introduce CDN(content deliver network) to the platform or deploy the application closer to the clients
3.	Bandwidth we can check this by iftop or nload.
4.	Logs of the application: error log, system logs check whether server facing any issue talking with upstream like database or other regions
5.	Netstat: we can see open connections for that server. If there are too many open connections, we can close them.

```	Curl works with IP address but fails with domain? ```
That is an issue with DNS if the internet service provider resolution fails we cannot access the application 
/etc/resolve.conf this is where the dns entry is made I will change ISP of DNS to the ip address we need add the entry of nameserver ip address example nameserver 8.8.8.8, so this is very common thing to resolve any issues with DNS	

```	Website returns http 502 status code what could be the issue ```
502 is a bad gateway error. That means when we try access the backend server, backend server might  load balanced or or front end by reverse proxy but when a reverse proxy tries to send a request to the back-end server, either it is down or the response is late.
 <img width="802" height="195" alt="image" src="https://github.com/user-attachments/assets/33109dbe-fa12-4ec9-b24f-10015bda216c" />

When when I try to reach devopse.com, it will first reach the load balancer or reverse proxy. With that, it will go to the application server. If it is delayed or down, then 502 status code errors will occur. If the response is 404, then we might send it to the wrong location.
It can also be an issue with the database as well.
When the error 502 the issue is with the backend server 
To troubleshoot the issue, we will check first 
1./vars/log/nginx/error.log
2. use the curl command(backend server)
3. check logs in the backend server

```	What is the difference B/W 127.0.0.1 and 0.0.0.0 ? ```
127.0.0.0 is a loop back address  where as only machine talk to itself
0.0.0.0 means all the ipv4 address on the machine, it listens to all the interfaces
 <img width="811" height="97" alt="image" src="https://github.com/user-attachments/assets/03bdfd60-a23c-4c52-96a4-b8042f671131" />


```	What is the difference between a public subnet and a private subnet? ```
Public subnet has a route to the internet through internet gateway.
Example: frontend applications
 Where as are private subnet that do not have access to the internet. Private subnets are more secure compared to public subnet
Example : database

```	You accidentally created a private subnet instead of a public subnet. How you will fix it? ```
The public subnet is configured with an  internet gateway 
For the private subnet that was created,  in the route table and add internet gateway as the destination then the subnet will become public subnet.
<img width="730" height="228" alt="image" src="https://github.com/user-attachments/assets/6cf84ace-e210-4bfb-8f86-d29e7aec2099" />
 
