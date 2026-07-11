# 1. What is Scaling?

### Definition

Scaling is the process of increasing or decreasing the resources of a
system so that it can handle more users, more requests, and more data
while maintaining performance.

Imagine your website receives:

Day 1 → 100 users Day 100 → 10 million users

Your current server cannot handle all those users.

You have two choices:

Make the server bigger (Vertical Scaling) Add more servers (Horizontal
Scaling) Diagram Users \| \| +--------------+ \| Web Server \|
+--------------+

          Handles 100 users

More users arrive

Users = 1 Million

Need Scaling \| +------------+ \| \| Vertical Horizontal Scaling Scaling
\## 2. Vertical Scaling (Scale Up) \### Definition

Vertical Scaling means increasing the capacity of an existing server by
adding more:

CPU RAM SSD GPU Network bandwidth

Instead of adding new machines, we make the current machine stronger.

Before Scaling +----------------+ \| CPU : 2 Core \| \| RAM : 4 GB \| \|
SSD :100 GB \| +----------------+

Handles

500 Requests/sec After Scaling +----------------+ \| CPU : 32 Core \| \|
RAM : 128 GB \| \| SSD : 4 TB \| +----------------+

Now handles

20,000 Requests/sec Diagram Before

Users \| \| +---------+ \| Server \| \|2 CPU \| \|4 GB RAM \|
+---------+

        Scale Up

Users \| \| +-------------+ \| Server \| \|32 CPU \| \|128 GB RAM \|
+-------------+ Advantages

✅ Easy

✅ No application changes

✅ No Load Balancer needed

Disadvantages

❌ Hardware limit

❌ Expensive

❌ Single point of failure

If this server crashes,

Entire application crashes.

Real Example

A startup has

2 CPU 8 GB RAM

As users increase

Upgrade to

64 CPU 512 GB RAM

Still only one server.

## 3. Horizontal Scaling (Scale Out)

### Definition

Instead of making one server bigger,

We add more servers.

All servers perform the same job.

Diagram

Without Horizontal Scaling

Users

\| \|

---

Server A

---

After Horizontal Scaling

                    Users
                      |
               +--------------+
               | Load Balancer|
               +--------------+
                /      |      \
               /       |       \
      +---------+ +---------+ +---------+
      |Server A | |Server B | |Server C |
      +---------+ +---------+ +---------+

Now

Server A Server B Server C

share the traffic.

Example

Each server handles

1000 Requests/sec

Three servers

1000 +1000 +1000 -------- 3000 Requests/sec

Need more?

Add Server D

4000 Requests/sec Advantages

✔ High availability

✔ Fault tolerance

✔ Easy scaling

✔ Cheaper than huge machines

Disadvantages

Needs

Load Balancer Distributed Cache Distributed Database

Application becomes more complex.

Real Example

Netflix

Instead of

1 Huge Server

They have

Thousands of servers

distributed worldwide.

Vertical vs Horizontal Scaling Vertical Scaling Horizontal Scaling
Increase server size Increase server count One server Multiple servers
Limited Almost unlimited Simple Complex Single point failure Fault
tolerant Expensive Cost effective \## 4. What is Load Balancing? \###
Definition

A Load Balancer is a component that distributes incoming client requests
across multiple servers so that no single server becomes overloaded.

Without Load Balancer

10000 Users

      |
      |

---

Server A

---

Server crashes

With Load Balancer

                 Users
                    |
                    |
           +----------------+
           | Load Balancer  |
           +----------------+
            /      |      \
           /       |       \
     +--------+ +--------+ +--------+
     |Server A| |Server B| |Server C|
     +--------+ +--------+ +--------+

Traffic gets divided.

Responsibilities of Load Balancer Route requests Health checking SSL
termination Session persistence (sticky sessions) Failover Traffic
distribution Rate limiting (some implementations) DDoS protection (often
integrated with edge services) Example

1000 Requests

Without LB

1000

↓

Server A

With LB

1000

↓

Load Balancer

↓

Server A = 333

Server B = 333

Server C = 334 \## 5. Client-Side Load Balancing \### Definition

The client decides which server to send the request to.

There is no centralized load balancer in the request path.

Diagram

             Client

       Chooses Server

        /      |      \
       /       |       \

Server A Server B Server C

Client keeps the server list.

Example

Server A Server B Server C

Client randomly selects one.

Used In Microservices gRPC Service Mesh Mobile SDKs

Examples include client libraries that integrate with service discovery.

Advantages

✔ No Load Balancer bottleneck

✔ Faster

✔ Lower latency

Disadvantages

❌ Client becomes complex

❌ Needs service discovery

## 6. Server-Side Load Balancing

### Definition

Clients always send requests to a load balancer.

The load balancer chooses the backend server.

Diagram

            Client
               |
               |
        +--------------+
        | LoadBalancer |
        +--------------+
          /    |    \
         /     |     \

ServerA ServerB ServerC

Advantages

✔ Client is simple

✔ Centralized routing

✔ Easy monitoring

Disadvantages

The Load Balancer itself can become a bottleneck if not deployed
redundantly.

Client-side vs Server-side Client Side Server Side Client selects server
LB selects server Faster Easier Complex client Simple client Needs
discovery Doesn't \## 7. Load Balancing Algorithms \## 1. Round Robin

### Definition

Requests are distributed one after another.

Diagram

Request1 → Server A

Request2 → Server B

Request3 → Server C

Request4 → Server A

Request5 → Server B

Good for

Servers with equal capacity.

## 2. Weighted Round Robin

Different servers have different capacities.

Example

Server A Weight = 5

Server B Weight = 3

Server C Weight = 2

Traffic

AAAAA BBB CC \## 3. Least Connections

Choose the server with the fewest active connections.

Example

Server A = 50

Server B = 20

Server C = 5

Choose

Server C \## 4. Weighted Least Connections

Considers both

Server capacity Current connections

Better for heterogeneous environments.

## 5. Least Response Time

Chooses the server responding fastest.

Example

A = 20 ms

B = 50 ms

C = 5 ms

Choose

Server C \## 6. IP Hash

Hash

Client IP

Same client always reaches same server.

Useful for session persistence.

## 7. Consistent Hashing

Commonly used in distributed caches and sharded systems.

Hash Ring

A------B------C

Adding a new server only remaps a small subset of keys, minimizing
disruption.

## 8. Random

Randomly selects a server.

Simple but less predictable.

## 8. OSI Layers

The OSI (Open Systems Interconnection) model has seven layers:

Layer Name Example 7 Application HTTP, HTTPS, gRPC 6 Presentation
SSL/TLS encryption, data formatting 5 Session Session management 4
Transport TCP, UDP 3 Network IP 2 Data Link Ethernet 1 Physical Cable,
Fiber

Load balancers commonly operate at Layer 4 or Layer 7.

## 9. Layer 4 Load Balancer

Works on

Transport Layer

TCP

UDP

It looks at

Source IP Destination IP Port Protocol

It does not inspect the application data (such as the HTTP URL).

Diagram

Client \| TCP Packet \| Layer 4 LB \| Chooses Server

Fast because it forwards packets based on network information.

Example

TCP Port 443

↓

Server A

Advantages

✔ Very fast

✔ High throughput

✔ Low latency

Disadvantages

Cannot route based on:

URL path HTTP headers Cookies \## 10. Layer 7 Load Balancer

Works on

HTTP

HTTPS

gRPC

Reads the application request.

Example request

GET /images/logo.png

Host: example.com

It can inspect:

URL Cookies Headers Host name HTTP method

Diagram

Client \| HTTP Request \| Layer 7 LB \| Reads URL

/images → Image Server

/api → API Server

/videos → Video Server

Advantages

✔ Smart routing

✔ SSL termination

✔ Authentication integration

✔ Content-based routing

✔ Request rewriting

Disadvantages

Slightly slower than Layer 4 because it parses application-layer data.

Layer 4 vs Layer 7 Layer 4 Layer 7 TCP/UDP HTTP/HTTPS/gRPC Fast More
intelligent IP/Port based Content based No URL inspection Can inspect
URL, headers, cookies Lower CPU usage Higher CPU usage \## 11.
Active-Passive Load Balancer

Two load balancers exist.

Only one handles traffic.

The other waits as a standby.

Diagram

               Clients
                  |
          +----------------+
          | Active LB      |
          +----------------+
                  |
             Backend Servers


          +----------------+
          | Passive LB     |
          | Standby        |
          +----------------+

If the active load balancer fails, the passive one takes over
automatically (failover).

Advantages Simple Reliable Easier to manage Disadvantages Standby
resources are idle during normal operation. \## 12. Active-Active Load
Balancer

Both load balancers serve traffic simultaneously.

Diagram

                Clients
                  |
         -------------------
         |                 |

+----------------+ +----------------+ \| Active LB 1 \| \| Active LB 2
\| +----------------+ +----------------+   /   / Backend Servers

If one fails:

All traffic

↓

Remaining LB Advantages Better utilization Higher throughput Greater
scalability High availability Disadvantages More complex synchronization
and configuration Requires careful health checks and state management
Complete High-Level System Design Flow Internet Users \| \| DNS
Resolution \| \| +----------------------+ \| Active Load Balancer \|
+----------------------+ /\
/\
+-----------+ +-----------+ \| Web/App 1 \| \| Web/App 2 \|
+-----------+ +-----------+   /   / +-------------------------+ \|
Distributed Cache \| \| (e.g., Redis Cluster) \|
+-------------------------+ \| \| +-------------------------+ \|
Database (Primary/Replica\| \| or Distributed Cluster) \|
+-------------------------+

As traffic grows:

Scale vertically by increasing server resources if feasible. Scale
horizontally by adding more application servers. Place a load balancer
in front of the servers to distribute requests. Use an appropriate
load-balancing algorithm (Round Robin, Least Connections, etc.). Choose
Layer 4 load balancing for maximum throughput or Layer 7 when
content-aware routing is needed. Deploy load balancers in Active-Passive
mode for simple failover or Active-Active mode for both high
availability and increased capacity.

This progression forms the foundation of the architecture used by
large-scale systems such as those operated by companies like Netflix,
Google, Amazon, and Meta.
