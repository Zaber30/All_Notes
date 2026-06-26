**Scaling is the process of increasing or decreasing the resources of a system to handle changes in workload (number of users, requests, or data).**

In simple words, scaling means **adjusting a system's capacity when demand changes.**

# Types of Scaling

There are mainly two types:

1. **Vertical Scaling (Scale Up)**
2. **Horizontal Scaling (Scale Out)**

---

# 1. Vertical Scaling (Scale Up)

## Definition

**Vertical scaling means increasing the power of an existing server by adding more resources such as CPU, RAM, or storage.**

You make one machine stronger.

# 2. Horizontal Scaling (Scale Out)

## Definition

**Horizontal scaling means adding more servers to handle increased workload.**

Instead of making one server stronger, you add more servers.
## Example

Before:

```
User Requests      |   Server 1
```

After scaling:

```
          Users             |       Load Balancer             | --------------------- |        |          |Server1 Server2 Server3
```

Requests are shared between multiple servers.