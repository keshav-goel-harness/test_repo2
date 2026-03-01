### Customer Asks
1. **Granular header support** — Ability to set request/response headers with `add`, `set`, and `remove` (not just key/value pairs).
2. **Patch-only behavior** — If fields aren't defined in the Canary step, don't remove them from the YAML. Only patch what's explicitly configured.



## 1. What is a VirtualService in the Canary Flow?

An Istio **VirtualService** is a custom Kubernetes resource that defines traffic routing rules. In a typical canary setup, the customer has a VirtualService like this:

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: my-app-vs
spec:
  hosts:
    - my-app-svc
  http:
    - match:                       # <--- MATCH RULES (headers, URI, method, etc.)
        - headers:
            x-canary:
              exact: "true"
      name: my-route
      route:                       # <--- ROUTE DESTINATIONS (weight-based traffic split)
        - destination:
            host: my-app-stable
          weight: 100
        - destination:
            host: my-app-canary
          weight: 0
```
