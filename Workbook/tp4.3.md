
## Lab 4.3: Ingress

Goal: install an Ingress Controller and then use it

Duration: 0:10:00

### Ingress Controller

- This exercice is in the _Namespace_ `shopping`
- Activate the `ingress` addon:

```shell
minikube addons enable ingress
```

- Wait for the controller to be ready by running the following command:

```shell
kubectl wait --namespace ingress-nginx\
  --for=condition=ready pod \
  --selector=app.kubernetes.io/component=controller\
  --timeout=90s
```

- Display all objects created in the `ingress-nginx` Namespace with the following command:

```shell
kubectl get pod,svc,deploy,rs,job -n ingress-nginx
```

- Expose the Ingress Controller:

```shell
docker container run --name expose-ingress-controller --detach --network minikube --publish 80:80 alpine/socat tcp-listen:80,fork,reuseaddr tcp-connect:minikube:80
```

### Ingress whoami

> **Note**: For the rest of the lab, replace `FIXME`, with the IP of your minikube machine:
> - ☁️ (cloud): `${PUBLIC_IP}`
> - 💻 (local): `minikube ip`

- Expose the `whoami` Service by an _Ingress_ which will have to respond on the url `whoami.FIXME.sslip.io`
- Complete the following template available in `~/workspaces/Lab4/ingress--whoami.yml` by replacing `XXXX` and `YYYY` with the right values.

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: whoami
  namespace: shopping
spec:
  rules:
    - host: whoami.FIXME.sslip.io
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: XXXX
                port:
                  number: YYYY
```

- Check that the _Ingress_ is correctly configured with `kubectl get ingress`
- Test the url `http://whoami.FIXME.sslip.io/`, check by refreshing the page that you arrive alternately on the different Pods of the Service (see `Hostname`)

- Observe the logs of the Pod `ingress-nginx-controller-...` of the Namespace `ingress-nginx`

<div class="pb"></div>
