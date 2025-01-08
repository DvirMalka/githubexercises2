
## Lab 3.2: DaemonSets

Goal: create a _DaemonSet_

Duration: 0:15:00

### Setting up the label on the nodes

- Add the `disk=ssd` label on all the nodes:

```shell
kubectl label nodes --all disk=ssd
```

### First DaemonSet

- Create a _DaemonSet_ from the following descriptor (source : `~/workspaces/Lab3/ds-ssd-tp-3.2.yml`) :

```yaml
---
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: ssd-monitor
spec:
  selector:
    matchLabels:
      app: ssd-monitor
  template:
    metadata:
      labels:
        app: ssd-monitor
    spec:
      nodeSelector:
        disk: ssd
      containers:
        - name: main
          image: debian:11-slim
          command:
            - bash
            - -c
            - while true; do echo 'SSD OK'; sleep 5; done
      terminationGracePeriodSeconds: 2
```

- List _DaemonSets_ using `kubectl get ds`
- List _Pods_ associated with the `ssd-monitor` _DaemonSet_
- What do you notice?
- Access the logs of the Pods created by the _DaemonSet_ `ssd-monitor`
- Delete one of the Pod created by the _DaemonSet_ and check that it is recreated

### Changing the label on the nodes

- Replace the `disk=ssd` label on the minikube node with `disk=hdd`
- What do you notice?

<div class="pb"></div>
