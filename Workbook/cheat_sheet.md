# Common commands to interact with Docker/Kubernetes

## Docker

Create a container in interactive mode:
`docker container run -it ubuntu bash`

Create a container in interactive mode while modifying the entrypoint:
`docker container run --entrypoint bash -it ubuntu`

Launch a container in detached mode:
`docker container run -d debian:11-slim sleep infinity`

Show running containers:
`docker container ls`

Open a shell in a container in interactive mode:
`docker container exec -it <CONTAINER_NAME> bash`

Build a Docker image:
`docker image build . -t my_image:my_tag`

## Kubernetes

### Pods

Create a Pod in interactive mode:
`kubectl run -ti --image=ubuntu --restart=Never --rm bash`

List running Pods of the Namespace `staging`:
`kubectl get pods -n staging`

List running Pods with their IP and scheduled node:
`kubectl get pods -o wide`

View a Pod's `YAML` manifest:
`kubectl get pod <POD_NAME> -o yaml`

Open a shell in a Pod/container in interactive mode:
`kubectl exec -it <POD_NAME> -- bash`

If the Pod has multiple containers, use this command:
`kubectl exec -it <POD_NAME> -c <CONTAINER_NAME> --bash`

View Pod logs:
`kubectl logs <POD_NAME>`

View and follow Pod logs:
`kubectl logs <POD_NAME> -f`

View logs for a specific Pod container:
`kubectl logs <POD_NAME> -c <CONTAINER_NAME>`

View complete Pod information (including events):
`kubectl describe pod <POD_NAME>`

### Namespaces

List Namespaces:
`kubectl get ns`

Create a Namespace:
`kubectl create ns <NAMESPACE_NAME>`

### Service

List Services:
`kubectl get services`

List Endpoints:
`kubectl get endpoints`

## VIM

- https://devhints.io/vim
