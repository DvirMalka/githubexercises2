
## Lab 1.2: kubectl

Goal: Getting started with the kubectl CLI

Duration: 0:05:00

### Getting started

- Display the list of all available commands:
  - `kubectl`
  - `kubectl get --help`
- Check the completion is working by typing: `kubectl g<tab>`
- Display client (kubectl) and cluster versions: `kubectl version --output yaml`
- Display the common options: `kubectl options`

### Cluster info

- Display the name of the currently used cluster: `kubectl config current-context`
- Display the cluster nodes: `kubectl get nodes`

### Check available groups and resources

- Use `kubectl api-resources` to check resources available on the cluster

<div class="pb"></div>
