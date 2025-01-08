
## Labs 7.1: Deployments

Goal: simulate the rolling update process of an application

Duration: 0:10:00

### Deployment v1

- Check the _Deployment_ descriptor `~/workspaces/Lab7/deploy--zenika-v1--7.1.yml`:

```yaml
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: zenika
  labels:
    app: zenika
spec:
  replicas: 3
  minReadySeconds: 5
  revisionHistoryLimit: 5
  selector:
    matchLabels:
      app: zenika
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  template:
    metadata:
      labels:
        app: zenika
    spec:
      containers:
        - name: app
          image: zenika/k8s-training-deploy:v1
          ports:
            - name: main
              containerPort: 8080
          livenessProbe:
            httpGet:
              path: /.healthcheck
              port: 8080
            periodSeconds: 5
            initialDelaySeconds: 2
          readinessProbe:
            httpGet:
              path: /.readicheck
              port: 8080
            periodSeconds: 5
            initialDelaySeconds: 5
```

- Create the Deployment with `kubectl apply -f ~/workspaces/Lab7/deploy--zenika-v1--7.1.yml --record`
- Check the state of the Deployment and the associated ReplicaSet and Pods

### Service

- Create the associated Service with the following descriptor (source: `~/workspaces/Lab7/svc--zenika.yml`):

```yaml
---
apiVersion: v1
kind: Service
metadata:
  name: zenika-svc
  labels:
    app: zenika
spec:
  selector:
    app: zenika
  ports:
    - name: main
      protocol: TCP
      port: 80
      targetPort: main
```

- If needed recreate the `gateway` _Pod_ used during the Lab4.1: `yq 'del(.metadata.namespace)' ~/workspaces/Lab4/pod--gateway.yml | kubectl apply -f -`
- Connect in the `gateway` Pod and launch the command: `while true; do curl --silent http://zenika-svc; sleep 1; done`
- (leave the command running and open a new terminal tab for the next part)

### Update the Deployment to v2

- Update the Deployment descriptor, and change the image tag value to _v2_
- Apply the change
- Launch `kubectl rollout status deployment zenika` from time to time to check the deployment status
- Check the state of the Deployment and the associated ReplicaSet and Pods
- Check the output of the loop running in the `gateway` Pod that the application is being updated
- Use `kubectl rollout history deploy zenika` to view all recorded revisions for the Deployment

<div class="pb"></div>
