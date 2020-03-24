# Discovery

To form a highly-available cluster, pgcontd processes must find and talk to each
other.

## Domain Name System

DNS service (SRV) records. Service `pgcontd` over protocol `tcp`.

```
_pgcontd._tcp.{cluster}
( _postgres._tcp.{cluster} )
```

TODO: describe algorithm after lookup.


In Kubernetes, [SRV records][k8s-srv] are created from service definitions.
- "regular" resolves to one service domain name (which resolves to the cluster IP.)
- "headless" resolves to many pod domain names (which each resolve to a pod IP.)

[k8s-srv]: https://kubernetes.io/docs/concepts/services-networking/dns-pod-service/#srv-records

## Kubernetes

Ask the Kubernetes API for pods that match.
