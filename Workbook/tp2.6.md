
## Lab 2.6: Life cycle

Goals: understand the automatic restart of containers

Duration: 0:20:00

### Experiments

- Run `watch kubectl get po` command in another terminal
- Identify the container corresponding to the `clock` container of the `whoami-and-clock` Pod:
```shell
minikube ssh --node minikube-m02 -- crictl ps --label io.kubernetes.pod.name=whoami-and-clock --label io.kubernetes.container.name=clock
```
- Terminate the previously identified container:
```shell
minikube ssh --node minikube-m02 -- crictl stop <container_id>
```
- Check that Kubernetes restarts the container in the `whoami-and-clock` Pod and that the __restart__ count has evolved

### Restart Policy

- Create a Pod from the following descriptor (source: `~/workspaces/Lab2/pod--restart-policy-rp-check--2.6.yml`):

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: rp-check
  labels:
    app: rpchk
spec:
  restartPolicy: Always
  containers:
    - name: rp-check
      image: debian:11-slim
      command:
        - bash
        - -c
        - sleep 15s
```

- Check the running Pods with `watch kubectl get po`
- What do you notice?
- Delete the created Pod
- Change the value of `restartPolicy` to `OnFailure`
- Recreate the Pod
- Watch the running Pods with `watch kubectl get po`
- What do you notice?
- Show Pods list.
- Delete the `rp-check` Pod
- Update the command in the descriptor, replace `sleep 15s` with `sleep 15s; false` (Note: the `false` command returns a `status code` which is 1)
- Recreate the `rp-check` Pod
- Watch the running Pods with `watch kubectl get po`
- What do you notice?

<div class="pb"></div>
