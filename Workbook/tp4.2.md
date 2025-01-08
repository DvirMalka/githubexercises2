
## Lab 4.2: NodePort and LoadBalancer

Goal: Expose a Service to the outside in 2 ways

Duration: 0:15:00

### NodePort

- This exercice is in the _Namespace_ `shopping`
- Change the `whoami` Service to a _NodePort_ Service type without specifying the nodePort value
- Check with `kubectl get svc whoami`
- Test the Service locally on `minikube ip` using the `nodePort`
```shell
curl $(minikube ip):<NODEPORT>
```

> In this lab we need an additional tool to expose services directly on the host VM, this is due to the fact that our nodes are spawned as containers
> To do this we use the tool [socat](http://www.dest-unreach.org/socat/) with a container exposing a host port and redirecting to a minikube node

- Expose the nodePort outside the minikube with:
```shell
WHOAMI_NODE_PORT=$(kubectl get svc -n shopping whoami -ojsonpath="{.spec.ports[0].nodePort}")
docker container run --name expose-port-${WHOAMI_NODE_PORT} --detach --network minikube --publish ${WHOAMI_NODE_PORT}:${WHOAMI_NODE_PORT} alpine/socat tcp-listen:${WHOAMI_NODE_PORT},fork,reuseaddr tcp-connect:minikube:${WHOAMI_NODE_PORT}
```
- Test the Service from outside the minikube using the `nodePort`
- Show detailed information of `whoami` Service

### LoadBalancer

- Retrieve the minikube IP with `minikube ip` (e.g. `192.168.49.2`)
- Configure the minikube addon `metallb` ([doc](https://metallb.universe.tf/)) with the command `minikube addons configure metallb`:
   - StartIP: minikube IP whith last octet set to 10 (e.g. `192.168.49.10`)
   - End IP: minikube IP whith last octet set to 250 (e.g. `192.168.49.250`)
- Activate the minikube addon `metallb` with the command `minikube addons enable metallb`

- Remove `whoami` Service
- Recreate it with type _LoadBalancer_
- Check with `kubectl get svc whoami`
- Test the Service from the kubernetes machine on the announced External IP using the Service `port`
- Show detailed information of `whoami` Service

<div class="pb"></div>
