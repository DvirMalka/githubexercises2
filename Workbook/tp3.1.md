
## Lab 3.1: ReplicaSets

Goals :
- create a _ReplicaSet_
- scale up the _Pod_ replica count
- migrate _Pods_ from a _ReplicaSet_ to another

Duration: 0:25:00

### Clean up

- Delete all Pods from the __default__ Namespace with `kubectl delete pod --all`
- Optional: view the resources successive states with `watch kubectl get pod --show-labels`

### First ReplicaSet

- Create _ReplicaSet_ named `nginx` from the following descriptor (source : `~/workspaces/Lab3/rs--nginx-simple--3.1.yml`) :

```yaml
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
      level: novice
  template:
    metadata:
      name: nginx
      labels:
        app: nginx
        level: novice
    spec:
      containers:
        - name: nginx
          image: nginx:alpine
          ports:
            - containerPort: 80
```

- Monitor the progress of the creation of the _RS_ and associated Pods with the command `watch kubectl get pod,rs --show-labels`
- Delete one of the created Pods and check that the _RS_ is doing its job

### ReplicaSet and Pod selector

- Create a _ReplicaSet_ from the following descriptor (source : `~/workspaces/Lab3/rs--nginx-cannot-create--3.1.yml`) :

```yaml
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: frontend
      level: intermediate
  template:
    metadata:
      name: frontend
      labels:
        app: frontend
        level: novice
    spec:
      containers:
        - name: nginx-fe
          image: nginx:alpine
          ports:
            - containerPort: 80
```

- What is wrong ?
- Fix the _selector_ in the descriptor and create the _RS_
- Check that the _Replicaset_ has been created successfully

### Modifying a ReplicaSet

- In the _RS_ `frontend` template, modify the _nginx_ image used to `nginx:stable-alpine`
- Apply the new configuration of the _RS_
- Check that no Pod are being recreated
- With `kubectl label` modify the labels on the `frontend-xxxxx` Pods by overriding the value for the _level_ key to `advanced`
- What is going on ? Why ?
- Check that the docker image used by the new Pods created is `nginx:stable-alpine`

### Scaling a ReplicaSet

- Change the number of replicas of the _RS_ `frontend` to _3_ using `kubectl scale`
- In the _RS_ `frontend` template, change the number of replicas to _4_ and apply the new configuration
- Use `kubectl describe pod frontend-xxxx` on one of the Pods associated with the frontend ReplicaSet and note the value of the `Controlled By` field
- Use `kubectl get pod frontend-xxxx -o yaml` on one of the Pods associated with the ReplicaSet frontend and note the value of the `.metadata.ownerReferences` field

### Deleting a ReplicaSet

- Delete the _RS_ `nginx` with its Pods
- Delete the _RS_ `frontend` _*without deleting*_ its Pods
- Use `kubectl describe pod frontend-xxxx` on one of the Pods associated with the frontend ReplicaSet and note that the `Controlled By` field is no longer present
- Use `kubectl get pod frontend-xxxx -o yaml` on one of the Pods associated with the frontend ReplicaSet and note that the `.metadata.ownerReferences` field is no longer present

### Migrating Pods from one ReplicaSet to another (Optional)

We now have several orphaned `frontend-*` Pods, i.e. without _ReplicaSet_. The goal is to attach them to a new _ReplicaSet_ named `frontend-rebirth`.

- Create a _ReplicaSet_ (called `frontend-rebirth`) to manage the Pods previously handled by the _RS_ `frontend` (`app=frontend,level=novice`)
- Check _ReplicaSet_ status with `kubectl get rs frontend-rebirth`
- Use `kubectl describe pod frontend-xxxx` on one of the Pods previously associated with the `frontend` ReplicaSet and note the value of the `Controlled By` field
- Use `kubectl get pod frontend-xxxx -o yaml` on one of the Pods previously associated with the `frontend` ReplicaSet and note the value of the `.metadata.ownerReferences` field

<div class="pb"></div>
