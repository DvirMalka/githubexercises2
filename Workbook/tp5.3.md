## Lab 5.3: PVC and PV

Goal: Use PV and PVC

Duration: 0:10:00

### Pre-requisites

- Enable the `csi-hostpath-driver` addon:

```shell
minikube addons enable csi-hostpath-driver
```

- Set the corresponding StorageClass as the default one:

```shell
kubectl annotate storageclass standard storageclass.kubernetes.io/is-default-class-
kubectl annotate storageclass csi-hostpath-sc storageclass.kubernetes.io/is-default-class=true
```

### Pre-provision of a PV

- Create a PersistentVolume from the following descriptor (source: `~/workspaces/Lab5/pv--my-hostpath-pv--5.3.yml`):

```yaml
---
apiVersion: v1
kind: PersistentVolume
metadata:
  name: my-hostpath-pv
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteOnce
    - ReadOnlyMany
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /data/pv/pv003
```

- Check that the _PersistentVolume_ has been created and that it is currently not bound to any _PersistentVolumeClaim_

### Creating a PersistentVolumeClaim

- Create a _PersistentVolumeClaim_ from the following descriptor (source: `~/workspaces/Lab5/pvc--firstclaim--5.3.yml`):

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-first-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
  storageClassName: ""
```

- Check that the _PersistentVolumeClaim_ has been created and that it is bound to the _PersistentVolume_ created previously
- Check the status of the _PersistentVolume_ created previously
- What is the capacity associated with the PVC created? Why?

### Using the PVC

- Adapt the following descriptor (source: `~/workspaces/Lab5/pod--use-pvc-bootstrap--5.3.yml`):

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-pv
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

- Make the container `log2hostfs` mount a volume called `var-log-nginx` on the directory `/var/log/nginx`
- Make volume `var-log-nginx` a volume provisioned from a _PersistentVolumeClaim_

### Check

- Connect into the container `shell`
- Run the command `curl localhost` several times
- Check where was the file `access.log` created ?
  - Check on which node the Pod was created
  - Check the `hostPath` of the created persistent volume
  - Connect to the node `minikube ssh --node <node>` and check in the path found above

### Creating a second PVC

- Create a second PVC from the following descriptor (source: `~/workspaces/Lab5/pvc--second-claim--5.3.yml`):

```yaml
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: my-second-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 500Mi
  storageClassName: ""
```

- What is the state of the PVC?

### Optional: Fix the situation

- Do what is necessary so that the second PVC can be bound to a PV:
  - either by manually creating another PV
  - or by recreating the PVC by modifying the `storageClassName` (either completely remove the field, the default StorageClass will be used, or use the name of the existing StorageClass `kubectl get storageclass`)

<div class="pb"></div>
