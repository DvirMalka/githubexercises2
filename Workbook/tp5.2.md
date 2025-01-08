## Lab 5.2: HostPath volume

Goal: create a volume of type `hostPath`

Duration: 0:05:00

### Initialization

- Modify the following descriptor (source: `~/workspaces/Lab5/pod--shared-hostPath-bootstrap--5.2.yml`):
  - Declare a volume `var-log-nginx` with type `hostPath` which mounts the directory `/mnt/sda1/data/var-log-nginx` of the node
  - Mount the volume `var-log-nginx` on the directory `/var/log/nginx` of the container `log2hostfs`

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: shared-hostpath-vol-pod
spec:
  containers:
    - name: log2hostfs
      image: zenika/k8s-training-nginx-log2fs:1.19-alpine-v1
    - name: shell
      image: zenika/k8s-training-tools:v5
      command:
        - bash
        - -c
        - sleep infinity
```

### Check

- From the container `shell` of the pod `shared-hostpath-vol-pod`, run the command `curl localhost` several times
- Get the name of the node where the pod was scheduled
- From the node, check that the file `access.log` in the directory `/mnt/sda1/data/var-log-nginx` has been modified with `minikube ssh --node <node-name> -- cat /mnt/sda1/data/var-log-nginx/access.log`

<div class="pb"></div>
