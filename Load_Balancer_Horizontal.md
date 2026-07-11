# Load Balancer and Horizontal Scaling

A load balancer is mainly used with **horizontal scaling**, because there are multiple servers to distribute traffic across.

Let's understand why.

---

# Vertical Scaling (No Load Balancer Needed)

In vertical scaling, you only have **one server**.

```text
            Users
               |
               |
        +--------------+
        |   Server     |
        | 32 CPU       |
        |128 GB RAM    |
        +--------------+
```

There is only **one destination**.

There is nothing to balance.

So, a **load balancer provides little or no benefit** in this scenario.

---

# Horizontal Scaling (Load Balancer Required)

Suppose your application grows.

Instead of one powerful server, you now have **multiple servers**.

```text
                    Users
                       |
                       |
               +----------------+
               | Load Balancer  |
               +----------------+
                /      |      \
               /       |       \
         +---------+ +---------+ +---------+
         |Server A | |Server B | |Server C |
         +---------+ +---------+ +---------+
```

The load balancer distributes incoming requests across the servers.

Example:

- Request 1 → Server A
- Request 2 → Server B
- Request 3 → Server C
- Request 4 → Server A

Without a load balancer:

- Clients must know which server to contact.
- One server may become overloaded while others remain idle.

---

# Why is a Load Balancer Needed?

Imagine you have three servers:

- Server A
- Server B
- Server C

If **10,000 users** all connect to **Server A**:

```text
Server A = 10,000 Requests ❌

Server B = 0

Server C = 0
```

Server A may crash even though the other two servers are idle.

With a load balancer:

```text
Server A = 3,333

Server B = 3,333

Server C = 3,334
```

The workload is distributed evenly across all servers.

---

# Can a Load Balancer Be Used with Vertical Scaling?

**Technically, yes**, but not for balancing traffic across multiple application servers.

Example:

```text
              Users
                 |
          +--------------+
          | Load Balancer|
          +--------------+
                 |
          +--------------+
          | Big Server   |
          +--------------+
```

Here, the load balancer isn't balancing traffic because there is only **one backend server**.

However, organizations may still place a load balancer in front of a single server to:

- Terminate SSL/TLS
- Provide a single public endpoint
- Perform health checks
- Enable an easy transition to multiple servers later
- Support high availability by failing over to a standby server if one exists

---

# Real-World Evolution

Most applications grow in the following stages.

## Step 1: Startup

```text
Users
  |
Server
```

One server is enough.

---

## Step 2: Vertical Scaling

```text
Users
  |
Big Server
```

Increase CPU and RAM as traffic grows.

---

## Step 3: Horizontal Scaling

```text
                  Users
                     |
              Load Balancer
               /    |    \
              /     |     \
          Server  Server  Server
```

At this stage, the **load balancer becomes essential**.

---

# Interview Tip

A common interview question is:

> **When do we introduce a load balancer?**

### Good Answer

> When a single server can no longer handle the traffic or when high availability is required, we horizontally scale the application by adding multiple servers. A load balancer is then introduced to distribute incoming requests among those servers, improving scalability, fault tolerance, and availability.

---

# Summary

| Scenario | Load Balancer Required? |
|-----------|-------------------------|
| Vertical Scaling | ❌ Generally No |
| Horizontal Scaling | ✅ Yes |
| Single Server with SSL Termination or Health Checks | ✅ Optional |

## Key Takeaways

- ✅ **Horizontal scaling → Load balancer is typically required.**
- ❌ **Vertical scaling alone → Load balancer is generally not required.**
- ✅ Even with a single server, a load balancer may be used for:
  - SSL termination
  - Health checks
  - High availability
  - Future scalability