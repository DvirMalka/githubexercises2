
## Lab 2.4: logs

Goals: consult the logs of a Pod

Duration: 0:05:00

### Getting Started

- Check the `kubectl logs` online help
- Create a Pod from the following descriptor (source: `~/workspaces/Lab2/pod--whoami-and-clock--2.4.yml`)

```yaml
---
apiVersion: v1
kind: Pod
metadata:
  name: whoami-and-clock
spec:
  nodeSelector:
    kubernetes.io/hostname: minikube-m02
  containers:
    - name: whoami
      image: traefik/whoami:v1.10
      imagePullPolicy: IfNotPresent
      ports:
        - containerPort: 80
      args:
        - --verbose
    - name: clock
      image: debian:11-slim
      command:
        - bash
        - -c
        - while true; do date | tee /dev/stderr; sleep 1; done
```

- Check the logs of the `clock` container of the `whoami-and-clock` Pod
- Try with the `--follow`, `--tail` and `--since` options

<div class="pb"></div>
