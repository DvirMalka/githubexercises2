
## Lab 2.1: Pods

Goal: create a Pod and check its behaviour

Duration: 0:30:00

### Getting started with 'kubectl apply'

- Display the help for `kubectl apply` and browse the available options

### Use a YAML descriptor

- Create a `yaml` Pod manifest:
  - name: `yaml-pod`
  - with image `traefik/whoami:v1.10`
  - reference container port `80`
  - Note: use the following command to create a descriptor example in the file `yaml-pod.yml`: `kubectl run POD_NAME --image=IMAGE_NAME --port=PORT_VALUE --dry-run=client -o yaml > yaml-pod.yml`
- Instantiate the Pod using the descriptor file
- Check that the application is working with the following command:

```shell
# We have to enter in a node
minikube ssh -- curl -s <ip-du-pod-yaml-pod>:80
```

### Adding a container in the Pod

- Delete the pod `yaml-pod`
- Edit the descriptor of the Pod `yaml-pod` to add a **second container**
  - named `shell-in-pod`
  - from image `zenika/k8s-training-tools:v5`
  - the main process of the container should be `sleep infinity` (check the documentation "[Define a Command and Arguments for a Container](https://kubernetes.io/docs/tasks/inject-data-application/define-command-argument-container/#running-a-command-in-a-shell)" to find the correct syntax)
  - force Kubernetes to use the last version of the image with `imagePullPolicy`
- Create a Pod from the edited file
- Check that the application is working with the following command:

```shell
# We have to enter in a node
minikube ssh -- curl -s <ip-du-pod-yaml-pod>:80
```

### Test whoami from the sidecar container

- Display help for `kubectl exec`
- Connect in the container `shell-in-pod` of Pod `yaml-pod` to open a bash session with `kubectl exec`
- Check that the `whoami` application is reachable on port `80` of `localhost`

### Edit a Pod

- Display detailed information for the Pod `yaml-pod` with `kubectl describe` and with `kubectl get pod POD_NAME -o yaml`
- Edit the manifest to add a label `from-descriptor: "yaml"` (use the following example, source : `~/workspaces/Lab2/pod--debian-with-label--2.1.yml`) [label usage will be covered in the next section]

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: debian-pod-with-labels
  labels:
    my-label: cool
    my-other-label: super
spec:
  containers:
    - name: shelly
      image: debian:11-slim
      imagePullPolicy: Always
      command:
        - bash
        - -c
        - sleep infinity
  restartPolicy: Always
```

- Apply the modifications on the Pod `yaml-pod` and check that the changes are correctly applied

### Display the definition of a Pod

- Launch the command `kubectl get pod yaml-pod -o yaml`
- Check the sections of the manifest

<div class="pb"></div>
