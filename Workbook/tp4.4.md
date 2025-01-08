
## Lab 4.4: Readiness Probe

Goals :
- set up a _Readiness_ probe
- simulate an error on the Pods

Duration: 0:20:00

### Simulate errors on the Pods of the ReplicaSet whoami

- This exercice is in the _Namespace_ `shopping`
- If needed recreate the gateway Pod used during the Lab4.1
- From the `gateway` Pod, check that the HTTP return code in GET on the URL `http://whoami:8080/health` is 200: `curl --head whoami:8080/health`
- Run the following command to change the HTTP return code to 500 for all Pods behind the `whoami` Service:

```shell
kubectl get endpoints whoami --output jsonpath='{range .subsets[0].addresses[*]}{.ip}{"\n"}{end}' | \
  while read ip; do echo "Send 500 to ${ip}/health"; kubectl exec gateway -- curl -si --data 500 ${ip}/health ; done
```
- From the `gateway` Pod, check that the HTTP return code in GET is now 500

### Addition of a Readiness probe on the Pods of the Whoami ReplicaSet

- Edit the ReplicaSet descriptor `whoami` (source: `~/workspaces/Lab4/rs--whoami--4.1.yml`)
- Add the following Readiness probe to the `whoami` container definition:

```yaml
readinessProbe:
  httpGet:
    path: /health
    port: 80
```

- Apply the changes to the ReplicaSet
- Optional: the _READY_ column of _Pods_ always displays `1/1`, why?
- Delete existing `whoami` _Pods_
- Check that the _READY_ column of the new _Pods_ has the value `1/1` (with `kubectl get po`)
- From the `gateway` Pod, run the command `curl -s --data 500 whoami:8080/health`
- Observe that the value of the _READY_ column changes to `0/1` after about ten seconds for one of the Pods
- Do what is necessary so that all the `whoami` Pods have in their _READY_ column the value `0/1`
- Check with `kubectl` (Optional: and by testing `http://whoami.FIXME.sslip.io/`)
- From the Pod `gateway`, run the command `curl -s --data 200 <ip_d_un_pod_whoami>:80/health`
- Observe the _READY_ column return to `1/1` after about ten seconds of the Pod concerned
- Optional: Note that when you consult `http://whoami.FIXME.sslip.io/` it is always this same Pod that is returned
- Do what is necessary to restart all `whoami` Pods. Their _READY_ column should go back to `1/1`

<div class="pb"></div>
