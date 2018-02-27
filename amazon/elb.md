# Fun with Amazon ELB

When configuring an ELB don't forget to set `CrossZone`.

Symptom strange (but logical) comportment of `HealthyHostCount` :
```
The number of healthy instances registered with your load balancer. A newly registered instance is considered healthy after it passes the first health check. If cross-zone load balancing is enabled, the number of healthy instances for the LoadBalancerName dimension is calculated across all Availability Zones. Otherwise, it is calculated per Availability Zone.
```
