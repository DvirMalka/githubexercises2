
## Lab 2.2: Labels

Goals:
- display and add labels on a Pod or a Node
- select Pods with labels
- use labels to control a Pod's schedule

Duration: 0:20:00

### Getting started

- Check help for `kubectl label`
- Check Pod labels `yaml-pod`
- Add `release=stable` and `stack=market` labels to the `yaml-pod` Pod
- Change the label `release=stable` to `release=unstable` for the Pod `yaml-pod`
- Display labels of the Pod `yaml-pod` and check that the expected labels are present
- View full Pod description `yaml-pod` and observe labels

### Show Pod Labels

- Display all Pod labels
- Display `run`, `release` and `stack` labels in dedicated columns
- Select only Pods that have the `stack` label set
- Optional: only display Pods that do not have the `release` label set
- Optional: display Pods for which the `run` label is `whoami` or `yaml-pod`
- Optional: display Pods for which the `run` label is `whoami` or `yaml-pod` and for which `stack` is not specified

### Using a nodeSelector

- List the current labels of the cluster nodes
- Create a Pod descriptor:
  - name: `light-sleeper`
  - 1 container:
    - Using the `debian:11-slim` image
    - Main process: `bash -c 'sleep 3d'`
  - Labels:
    - `from-descriptor=yaml`
  - Constrain the placement of the Pod on a node with the label `cluster-distribution=minikube`
- Create the Pod
- List all Pods in another session with `watch kubectl get pods`
- What is the status of the `light-sleeper` Pod? Why?
- Identify your nodes names with `kubectl get nodes`
- Add the label `cluster-distribution=minikube` on one of the nodes
- Check that the `light-sleeper` Pod starts

<div class="pb"></div>
