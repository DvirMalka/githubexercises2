
## Lab 1.3: kubectl run

Goal:
- create a Pod without a descriptor
- troubleshoot a Pod's start

Duration: 0:15:00

### Getting started

- Display the help for the `kubectl run` command:
```shell
kubectl run --help
```

### Start Pod

- Launch the following command that will:
  - start a `whoami` Pod
  - from the `traefik/whoami:v1.10` image
  - and reference the `80` port
```shell
kubectl run whoami --image=traefik/whoami:v1.10 --port=80
```

- Check the Pod start with `watch kubectl get po` (__ctrl+c__ to exit the __watch__ loop)
- What can you see?
- Display more details on the Pod with `kubectl describe` and get its IP address which you will need later
- Check that the application is working with the following command:
```shell
# We have to enter in a node of the cluster
minikube ssh -- curl <ip-of-the-pod>:80/api
```

### Troubleshoot a Pod startup

- Launch the following command that will:
  - start a __Pod__ (we will cover the Pod's detail later)
  - with the name `faulty-whoami`
  - from the image `traefik/whoami:nil`
  - and reference the `80` port
```shell
kubectl run faulty-whoami --image=traefik/whoami:nil --port=80
```

- Check the Pod's startup with `watch kubectl get po` (__ctrl+c__ to exit the __watch__ loop)
- Check that the Pod fails to start
- Display more info on the Pod and check the `Events` with `kubectl describe` to determine the error cause
- Delete this __Pod__ with `kubectl delete pod faulty-whoami`

### Start a Pod shell

- Launch the following command that will:
  - start a `training-shell` Pod
  - from the image `zenika/k8s-training-tools:v5`
  - whose main process will be `sleep infinity`
```shell
kubectl run training-shell --image=zenika/k8s-training-tools:v5 --command -- sleep infinity
```

- Check the Pod's startup
- List all created Pods
- Launch `kubectl exec training-shell -- curl -s <ip-of-the-pod>:80/api`
- Display the help for `kubectl exec`

### k9s

[`K9s`](https://k9scli.io/) is a TUI ("Text User Interface") used to browse Kubernetes resources without `kubectl` commands.

- Launch `k9s` to find the created __Pod__ and browse the interface. The keyboard shortcuts are displayed on top of the screen. Use `CTRL+C` to exit.

### Optional: Dashboard usage

The __Kubernetes__ dashboard is used to browse __Pods__ and other resources from a web UI.

To launch it:
- Check that the minikube addon `dashboard` is enabled, enable it if needed:
```shell
minikube addons enable dashboard
```
- ☁️ (cloud) :
  - Expose the dashboard with:
  ```shell
  kubectl -n kubernetes-dashboard patch service kubernetes-dashboard --type json --patch \
    '[
      {"op": "replace", "path": "/spec/type", "value": "NodePort"},
      {"op": "add", "path": "/spec/ports/0/nodePort", "value": 30998}
    ]'
  docker container run --name expose-dashboard --detach --network minikube --publish 9998:9998 alpine/socat tcp-listen:9998,fork,reuseaddr tcp-connect:minikube:30998
  ```
  - Open the dashboard using the Strigo button or in a new tab in your browser with `http://${PUBLIC_DNS}:9998`

- 💻 (local) :
  - Expose the dashboard with
  ```shell
  minikube dashboard &
  ```
  - Open the provided URL with your browser if it's not done automatically

- Browse the interface to find the __Pod__ which was just created.

- ☁️ (cloud) :
  - To avoid security issues, close public access to the Dashboard:
  ```shell
  docker container rm --force expose-dashboard
  ```


<div class="pb"></div>