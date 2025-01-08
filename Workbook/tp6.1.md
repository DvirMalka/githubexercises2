
## Lab 6.1: ConfigMaps

Goal: create ConfigMaps and use it from a Pod

Duration: 0:15:00

### Literal values

- Create the _ConfigMap_ `special-config` with the following key/values
  - `special.k=cereales`
  - `special.ite=kubernetes`
- Check the ConfigMap content

### Use as environment variable

- Create a Pod from the following descriptor (source: `~/workspaces/Lab6/pod--cm-one-venv--6.1.yml`)

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-cm-one-venv
spec:
  containers:
    - name: witness
      image: debian:11-slim
      command:
        - bash
        - -c
        - env ; sleep infinity
      env:
        - name: SPECIAL_LEVEL_KEY
          valueFrom:
            configMapKeyRef:
              name: special-config
              key: special.k
```

- Check in the Pod's logs the value of the SPECIAL_LEVEL_KEY environment variable

### Import all the ConfigMap keys

- Create the _ConfigMap_ `special-config-for-venv` with the following key/values
  - SPECIAL_K=cereales
  - SPECIAL_ITE=kubernetes
- Create a Pod from the following descriptor (source: `~/workspaces/Lab6/pod--cm-all-vars--6.1.yml`):

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: pod-with-cm-all-vars
spec:
  containers:
    - name: neo
      image: debian:11-slim
      command:
        - bash
        - -c
        - env ; sleep infinity
      envFrom:
        - configMapRef:
            name: special-config-for-venv
```

- Check in the Pod's logs the values of `SPECIAL_*` environment variables

### Import from files

- Create the _ConfigMap_ `game-config` from the content of the directory `~/workspaces/Lab6/configs`
- Check the content of the ConfigMap

### Use ConfigMap as a volume

- Create a Pod from the following descriptor (source: `~/workspaces/Lab6/pod--cm-as-vol--6.1.yml`):

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: cm-as-vol
spec:
  volumes:
    - name: config-volume
      configMap:
        name: game-config
  containers:
    - name: droopy
      image: debian:11-slim
      command:
        - bash
        - -c
        - ls /etc/config/ ; sleep infinity
      volumeMounts:
        - name: config-volume
          mountPath: /etc/config
```

- Connect inside the Pod and check the content of the files in the `/etc/config/` directory


<div class="pb"></div>
