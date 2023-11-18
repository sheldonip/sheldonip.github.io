---
title: "SNAT Ports Exhaustion"
date: 2023-11-18T16:07:00Z
draft: false
image: cover.jpg
---

## Introduction

When we are managing micro-services deployed in multi-clouds, with high traffic, we may encounter occasional 5xx or bad gateways error. This cannot be resolved by scaling up. The root cause can be SNAT port exhaustion. We will talk about this exhaustion and how to avoid it from happening.

## What is SNAT?

SNAT is useful for establishing outbound connections. It is not surprising to experience the exhaustion in cloud providers and on-premises. It is worth to check the number of SNAT ports before serving high traffic. You can also learn more about SNAT from [this Microsoft’s document](https://learn.microsoft.com/en-us/azure/load-balancer/load-balancer-outbound-connections).

## What happened?

For micro-services architecture, it is normal for applications to communicate with each other by using API calls. Sometimes an application would initiate numerous calls to the same address and port for your business. This can quickly consume SNAT ports. The application cannot open any new outbound connection until a SNAT port is available. You may find both source and destination applications are ready to serve but the source occasionally complains about connection failure.

Note that if the application opens connections to different address and ports, you may not experience this kind of exhaustion.

In Azure, the pre-allocation number of SNAT ports for a single outbound IP address is as the following:

| Pool size (VM instances) | Default SNAT ports |
| --- | --- |
| 1-50 | 1,024 |
| 51-100 | 512 |
| 101-200 | 256 |
| 201-400 | 128 |
| 401-800 | 64 |
| 801-1,000 | 32 |

Similar number applies to AKS.

We can identify SNAT exhaustion by inspecting the metrics of the load balancer of an AKS cluster.


### Implementation

As a developer, we can help prevent SNAT exhaustion by refining retry logic and using connection pools.

We may use a less aggressive retry algorithm to decrease the chance of opening a large number of outbound connections.

For Java applications, Spring WebFlux provides an event loop for establishing outbound calls. It significantly reduces the number of threads used, and hence, the number of outbound connection is reduced.

## Wrapping up

SNAT exhaustion is very likely happened where there is high traffic, and there is not much we can do to ease the exhaustion in the production environment. It is worth to perform a comprehensive load test for the whole service to identify the bottleneck before something goes wrong.

## References

- https://learn.microsoft.com/en-us/azure/app-service/troubleshoot-intermittent-outbound-connection-errors
- https://medium.com/@OpenJNY/demystifying-outbound-connectivity-and-snat-methods-in-azure-47f880b971c5
- https://www.danielstechblog.io/detecting-snat-port-exhaustion-on-azure-kubernetes-service/
- https://learn.microsoft.com/en-us/answers/questions/1294363/prevent-snat-port-exhaustion-in-aks-standard-load


## Credits

Cover photo by [Red John](https://unsplash.com/@redjohn45?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/cars-on-road-during-night-time-S-NW-qIFGG4?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

  