
## Lab 1.1: minikube usage

Goal: Start a `kubernetes` cluster on the VM.

Duration: 0:05:00

### minikube startup

- Launch minikube with the following command:

```shell
minikube start --kubernetes-version v1.30.0 --nodes 3
```

- Use the following command to check that minikube is running.

```shell
minikube status
```

### Addons management

- List enabled/disabled addons
- Enable the `metrics-server` addon if it's not already enabled

<div class="pb"></div>
