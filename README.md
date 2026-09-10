# Kong
Using Kong for routing, authentication, rate limiting, load balancing


Absolutely. Since you’re already seeing **Kong/Konga in your organization**, learning the basics from a DevOps/API-support perspective is useful.

### 1. First: What is Kong?

Kong Inc. **Kong** is an **API Gateway**.

Think of it like a **security + traffic controller in front of your microservices**:

```text
                    Clients
                       |
                       v
                 +-----------+
                 |    KONG   |
                 | API Gateway|
                 +-----------+
                  /    |    \
                 /     |     \
                v      v      v
             User   Payment   Order
             API      API      API
```

Instead of clients directly calling every backend service, requests can go through Kong.

### 2. What does Kong actually do?

Common things Kong handles:

* **Routing** → sends `/users` to User Service
* **Authentication** → verifies API credentials/token
* **Authorization**
* **Rate limiting** → e.g. maximum 100 requests/minute
* **Logging**
* **Monitoring**
* **Load balancing**
* **SSL/TLS termination**
* **Request/response transformation**
* **Plugins** → add functionality without changing application code

For example:

```text
Client
  |
  | GET /payment/123
  v
Kong
  |
  | Authentication ✓
  | Rate Limit ✓
  | Routing
  v
Payment Service
```

---

## 3. Then what is Konga?

This is where people commonly get confused.

**Konga is/was a web-based GUI for managing Kong.**

Instead of doing everything through Kong's APIs or command line, a GUI can make it easier to view/manage things such as:

```text
Kong
 ├── Services
 ├── Routes
 ├── Consumers
 ├── Plugins
 └── Upstreams / Targets
```

So a simple way to remember:

> **Kong = API Gateway**
> **Konga = GUI used to manage/visualize Kong**

One important point: **Konga and Kong are not the same product**, and Konga's compatibility/status depends on the Kong version and deployment your organization uses. If your company has a customized/internal Konga setup, its exact role may differ.

---

# 4. Important Kong terms you should learn

These are the terms I'd recommend you learn first for your DevOps role.

### Service

Represents your backend application/API.

```text
Kong → Service → Backend application
```

Example:

```text
User Service
URL: http://user-service:8080
```

### Route

Defines **how Kong matches an incoming request**.

Example:

```text
/api/users
```

could route to:

```text
User Service
```

So:

```text
Client
  |
  | /api/users
  v
Kong
  |
  v
User Service
```

### Consumer

Represents the **client/application/user** consuming an API.

For example:

```text
Consumer: Mobile-App
Consumer: Web-App
Consumer: Payment-App
```

### Plugin

This is extremely important.

Plugins add functionality to Kong.

Examples:

```text
Rate Limiting
Authentication
JWT
Key Authentication
CORS
Logging
Request Transformer
```

For example:

```text
Client
   |
   v
Kong
   |
   +--> Authentication Plugin
   |
   +--> Rate Limit Plugin
   |
   +--> Logging Plugin
   |
   v
Backend
```

---

# 5. Upstream and Target

You'll also encounter these.

Suppose you have:

```text
Payment Service
```

running on three instances:

```text
10.10.1.10:8080
10.10.1.11:8080
10.10.1.12:8080
```

Kong can use an **Upstream** and multiple **Targets**.

```text
                 Kong
                   |
                Upstream
                   |
          +--------+--------+
          |        |        |
       Target    Target    Target
       .10        .11       .12
```

Kong can distribute traffic between them.

---

# 6. A real-world DevOps example

Imagine your organization has:

```text
                    Customer
                       |
                       v
                    Kong
                       |
          +------------+------------+
          |            |            |
          v            v            v
      Login API    Payment API   Order API
          |            |            |
          v            v            v
       Backend      Backend       Backend
```

A customer calls:

```text
POST /api/payment
```

Kong may perform:

```text
1. Receive request
       ↓
2. Check authentication
       ↓
3. Check rate limit
       ↓
4. Apply required plugin
       ↓
5. Find matching Route
       ↓
6. Find Service
       ↓
7. Forward request
       ↓
8. Receive response
       ↓
9. Return response to client
```

This is why Kong becomes very important in a **microservices environment**.

---

# 7. Where Konga fits

You can visualize it like this:

```text
                 DevOps / Developer
                         |
                         v
                    +---------+
                    |  Konga  |
                    |   GUI   |
                    +---------+
                         |
                         v
                    +---------+
                    |  Kong   |
                    | Gateway |
                    +---------+
                         |
          +--------------+--------------+
          |              |              |
          v              v              v
       Service A      Service B      Service C
```

Konga provides a convenient interface to manage/inspect Kong rather than working entirely with APIs/CLI.

---

## 8. What you should learn for your job

Since you're working around **deployments, OCP/Kubernetes, monitoring and production support**, I would learn Kong in this order:

**Level 1 — Must know**

1. What is API Gateway?
2. What is Kong?
3. Kong vs Konga
4. Service
5. Route
6. Consumer
7. Plugin
8. Upstream
9. Target

**Level 2 — DevOps**

10. Kong on Kubernetes/OCP
11. Kong configuration
12. Kong Admin API
13. Authentication
14. Rate limiting
15. Logging
16. Health checks
17. Troubleshooting 4xx/5xx errors

**Level 3 — Production**

18. Kong + Kubernetes Ingress
19. Kong + DNS
20. Kong + TLS certificates
21. Kong + monitoring
22. Kong logs
23. Kong performance troubleshooting
24. Kong deployment/upgrade

### One interview definition to remember

>>> KONG:=
Kong is an API Gateway that sits between clients and backend services and provides capabilities such as routing, authentication, rate limiting, load balancing, security, and traffic management.**

KONGA:-

>>> Konga is a web-based GUI used to manage and monitor Kong, depending on the Kong/Konga version and setup.**



