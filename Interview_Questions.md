# Scaling & Load Balancer Interview Questions

## Level 1 (Basic Questions)

---

## Q1. What is Scaling?

### Answer

Scaling is the process of increasing or decreasing computing resources so that an application can handle more traffic while maintaining performance.

There are two types:

- **Vertical Scaling (Scale Up)**
- **Horizontal Scaling (Scale Out)**

---

## Q2. Why do We Need Scaling?

### Answer

Without scaling:

- Server becomes overloaded.
- Response time increases.
- Application crashes.
- Users experience downtime.

Scaling improves:

- Performance
- Availability
- Reliability
- User experience

---

## Q3. What is Vertical Scaling?

### Answer

Vertical scaling means increasing the resources of an existing server by adding:

- CPU
- RAM
- Storage
- Network bandwidth

### Example

**Before**

- 2 CPU
- 8 GB RAM

↓

**After**

- 32 CPU
- 128 GB RAM

---

## Q4. What is Horizontal Scaling?

### Answer

Horizontal scaling means adding more servers instead of making one server more powerful.

### Example

```text
          Load Balancer

          /    |    \

      S1      S2     S3
```

---

## Q5. Vertical vs Horizontal Scaling

| Vertical Scaling | Horizontal Scaling |
|------------------|--------------------|
| One server | Multiple servers |
| Increase CPU/RAM | Add servers |
| Easy | Complex |
| Limited | Nearly unlimited |
| Single point of failure | Fault tolerant |

---

# Load Balancer Questions

---

## Q6. What is a Load Balancer?

### Answer

A **Load Balancer** is a component that distributes incoming requests across multiple backend servers to improve:

- Scalability
- Availability
- Fault tolerance
- Performance

---

## Q7. Why do We Need a Load Balancer?

### Answer

Without a load balancer:

```text
10000 Users

↓

Server A

Server A crashes.
```

With a load balancer:

```text
Users

↓

Load Balancer

↓

A  B  C
```

Traffic is distributed evenly.

---

## Q8. When Do We Introduce a Load Balancer?

### Expected Interview Answer

A load balancer is introduced when:

- We horizontally scale our application.
- There are multiple backend servers.
- We need high availability.
- We want fault tolerance.

> **Interview Tip:** This is one of the most common interview questions.

---

## Q9. Can We Use a Load Balancer with Vertical Scaling?

### Answer

Yes.

Although it is not required, companies still use one for:

- SSL termination
- Reverse proxy
- Health checks
- Easy migration to multiple servers
- Centralized routing

---

# Load Balancing Algorithms

---

## Q10. Explain Round Robin.

### Answer

Requests are distributed one after another.

```text
R1 → A

R2 → B

R3 → C

R4 → A
```

**Best when all servers have equal capacity.**

---

## Q11. Explain Weighted Round Robin.

### Answer

Servers have different capacities.

### Example

```text
Server A Weight = 5

Server B Weight = 2
```

Server A receives more traffic.

---

## Q12. Explain Least Connections.

### Answer

Choose the server with the fewest active connections.

### Example

```text
A = 100

B = 50

C = 20

↓

Choose C
```

Suitable for long-lived connections.

---

## Q13. Explain IP Hash.

### Answer

The client's IP determines the server.

The same client always reaches the same server.

Useful for **sticky sessions**.

---

## Q14. Which Algorithm is Best?

There is **no universal best algorithm**.

| Scenario | Best Algorithm |
|----------|----------------|
| Equal servers | Round Robin |
| Unequal servers | Weighted Round Robin |
| Long requests | Least Connections |
| User sessions | IP Hash |
| Distributed cache | Consistent Hashing |

> Interviewers care about **why** you chose an algorithm, not just the name.

---

# Layer Questions

---

## Q15. What is Layer 4 Load Balancing?

### Answer

Works at the **Transport Layer**.

Uses:

- TCP
- UDP
- IP Address
- Port

Cannot inspect HTTP URLs.

---

## Q16. What is Layer 7 Load Balancing?

### Answer

Works at the **Application Layer**.

Can inspect:

- URL
- Cookies
- Headers
- Host name

### Example

```text
/api

↓

API Server

/images

↓

Image Server
```

---

## Q17. Layer 4 vs Layer 7

| Layer 4 | Layer 7 |
|----------|----------|
| TCP/UDP | HTTP/HTTPS |
| Fast | Intelligent |
| IP based | URL based |
| Low CPU | Higher CPU |

---

# Client-side vs Server-side Load Balancing

---

## Q18. Client-side Load Balancing

### Answer

The client chooses the server.

```text
Client

↓

Server A

or

Server B
```

Requires **service discovery**.

---

## Q19. Server-side Load Balancing

### Answer

The client sends the request to the load balancer.

The load balancer selects the backend server.

```text
Client

↓

Load Balancer

↓

Servers
```

---

# Availability Questions

---

## Q20. What is Active-Passive Load Balancing?

```text
Active LB

↓

Servers

Passive LB

(Standby)
```

The passive load balancer starts working only if the active one fails.

---

## Q21. What is Active-Active Load Balancing?

```text
LB1

LB2

↓

Servers
```

Both load balancers serve traffic simultaneously.

---

## Q22. Which is Better?

It depends.

### Active-Passive

**Pros**

- Easy
- Reliable

**Cons**

- Backup remains idle

### Active-Active

**Pros**

- Better utilization
- Higher throughput

**Cons**

- More complex
- Requires synchronization

---

# Health Check Questions

---

## Q23. What are Health Checks?

### Answer

The load balancer periodically checks whether backend servers are healthy.

### Example

```text
Heartbeat

↓

Server A

200 OK

Healthy
```

---

## Q24. What Happens if a Server Becomes Unhealthy?

### Answer

The load balancer removes it from the rotation.

**Before**

```text
A B C
```

**After**

```text
A C
```

Users are no longer sent to Server B.

---

## Q25. How Often Does a Load Balancer Perform Health Checks?

### Answer

Typically every **5–30 seconds**, depending on:

- Configuration
- Acceptable failover time

---

# Advanced Questions

---

## Q26. Is the Load Balancer a Single Point of Failure?

### Answer

Yes.

### Solution

Deploy multiple load balancers.

```text
Users

↓

LB1

LB2

↓

Servers
```

---

## Q27. How Do Multiple Load Balancers Work?

Usually in one of two modes:

- Active-Passive
- Active-Active

They monitor each other using **heartbeat mechanisms**.

---

## Q28. What is Session Affinity (Sticky Sessions)?

### Answer

The same client always reaches the same server.

Useful when:

- User sessions are stored in memory.

**Problem**

- Can cause uneven load distribution.

Modern applications usually store sessions in **Redis** or databases instead.

---

## Q29. What is SSL Termination?

Instead of every server decrypting HTTPS:

```text
HTTPS

↓

Load Balancer

↓

HTTP

↓

Servers
```

### Benefits

- Lower CPU usage
- Centralized certificate management

---

## Q30. What is Connection Draining?

### Answer

During deployments:

```text
Old Server

↓

No New Requests

↓

Complete Existing Requests

↓

Shutdown
```

Prevents interrupting active users.

---

# Scenario-Based Questions

---

## Q31. Your Website Traffic Suddenly Increases 10×. What Would You Do?

### Answer

- Monitor CPU, memory, and request rates.
- Add more application servers (horizontal scaling).
- Put a load balancer in front if not already present.
- Add caching (Redis/CDN).
- Scale the database if it becomes the bottleneck.

---

## Q32. One Server Becomes Slow. How Does the Load Balancer React?

### Answer

Health checks or latency metrics detect the issue.

The load balancer:

- Stops sending new requests to that server.
- Continues routing to healthy servers.
- Adds the server back after it passes health checks.

---

## Q33. Why Not Always Use Round Robin?

### Answer

Round Robin assumes:

- Every server has equal capacity.
- Every request costs roughly the same.

If one server is weaker or already busy, Round Robin may overload it.

Better alternatives:

- Weighted Round Robin
- Least Connections
- Least Response Time

---

## Q34. When Would You Choose Layer 4 Instead of Layer 7?

### Answer

### Choose Layer 4 when:

- Maximum throughput is required.
- TCP/UDP traffic is being balanced.
- No URL-based routing is needed.

### Choose Layer 7 when:

- URL-based routing is required.
- Routing by headers, cookies, or hostnames is needed.

---

## Q35. Draw the Architecture of a Scalable Web Application

```text
                Users
                   |
                   |
                 DNS
                   |
          +----------------+
          | Load Balancer  |
          +----------------+
            /     |      \
           /      |       \
      +------+ +------+ +------+
      | App1 | | App2 | | App3 |
      +------+ +------+ +------+
           \      |      /
            \     |     /
          +---------------+
          | Redis Cache   |
          +---------------+
                  |
          +---------------+
          | Database      |
          +---------------+
```

---

# Interview Tip

A common progression in interviews is:

1. Explain **Vertical vs Horizontal Scaling**.
2. Explain why **Horizontal Scaling requires a Load Balancer**.
3. Discuss **Load Balancing Algorithms** and justify your choice.
4. Compare **Layer 4 vs Layer 7**.
5. Handle failures (**Health Checks, Failover, Active-Active vs Active-Passive**).
6. Extend the discussion to **Caching, Databases, and Multi-Region Deployments**.
```