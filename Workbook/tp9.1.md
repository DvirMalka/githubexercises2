
## Lab 9.1: Helm

Goal: Deploy and update an application with Helm

Duration: 0:10:00

### Cleanup

- Clean up the cluster with the following commands (ignore displayed errors) :

```shell
kubectl delete deploy,sts,ds,rs,svc --all
kubectl delete pods --all
kubectl delete pvc,pv --all
for node in $(kubectl get nodes -o name | cut -d'/' -f2); do
  minikube ssh --node ${node} -- docker image prune --all --force
done
```

### Deploy the application

- Check that `Helm` is correctly installed:

```shell
helm version
```

- Create and use `helm-dockercoins` Namespace:

```shell
kubectl create namespace helm-dockercoins
kubectl config set-context --current --namespace=helm-dockercoins  # or kubens helm-dockercoins
```

- Get in the `~/workspaces/Lab9/dockercoins` directory
- Check the contents of the supplied `values.yaml` file
- Inspect the contents of the files in the `templates` folder
- Get dependencies:

```shell
helm dependency update
```

- Deploy a release:

```shell
public_ip=${PUBLIC_IP}  # or $(minikube ip)
echo "public_ip: ${public_ip}"
helm install dockercoins ./ \
  --set ingress.webui.host=dockercoins.${public_ip}.sslip.io
```

- Check the output produced by the install command
- Monitor the creation of Pods
- Check the app in your browser at `http://dockercoins.<PUBLIC_IP>.sslip.io`
- List releases with `helm list`

### Update the deployment

- Increase the number of worker replicas:

```shell
helm upgrade dockercoins ./ \
  --reuse-values \
  --set replicaCount.worker=2
```

- Update the worker:

```shell
helm upgrade dockercoins ./ \
  --reuse-values \
  --set image.worker.version=1.1
```

- Monitor the update with `watch kubectl get pods,rs,ds,deploy,svc,ing -o wide`

### Delete the application

- Delete the release:

```shell
helm delete dockercoins
```

- Exit and delete the namespace:

```shell
kubectl config set-context --current --namespace=default
kubectl delete namespace helm-dockercoins
```

<div class="pb"></div>
