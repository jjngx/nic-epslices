# EndpointSlices

## Comaprison with Endpoints

- K8s EndpointSlices reduce size of transferred objects to/from K8s API
- Lower CPU usage due to smaller objects
- The number of update events (for watching clients) triggered by Pod updates (eg. scaling deployment, rolling update) stays the same!

## K8s Docs

**EndpointSlices** [docs](https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/#distribution-of-endpointslices):

With kube-proxy running on each Node and watching EndpointSlices, every change to an EndpointSlice becomes relatively expensive since it will be transmitted to every Node in the cluster.

Rolling updates of Deployments also provide a natural repacking of EndpointSlices with all Pods and their corresponding endpoints getting replaced.

**Duplicate endpoints** [docs](https://kubernetes.io/docs/concepts/services-networking/endpoint-slices/#duplicate-endpoints):

Clients of the EndpointSlice API must iterate through all the existing EndpointSlices associated to a Service and build a complete list of unique network endpoints. It is important to mention that endpoints may be duplicated in different EndpointSlices.

## Approaches to limit number of reloads

### NGINX NIC Community

The community NIC uses `dynamic reloads` with the [balancer_by_lua](https://github.com/openresty/lua-resty-core/blob/master/lib/ngx/balancer.md) module.

[Full description](https://kubernetes.github.io/ingress-nginx/how-it-works/#avoiding-reloads).

### NGINX NIC OpenSource

- We do not use Lua.
- The question is what strategy we will take to address reloads.

### NGINX NIC Plus

- We leverage NGINX API calls for upstream updates without reloading.

### Kong

Code `endpoints` ref: [github](https://github.com/Kong/kubernetes-ingress-controller/blob/main/internal/adminapi/endpoints.go)
