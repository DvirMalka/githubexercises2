## Lab 4.6: Port forward

Goal: access the whoami _Pod_ using the port-forward

Duration: 0:05:00

- This exercice is in the _Namespace_ `shopping`
- Show online help for `kubectl port-forward`
- Use the command `kubectl run whoami --image=traefik/whoami:v1.10 --port=80` to directly launch a `whoami` Pod
- Use the `kubectl port-forward` command to forward port 80 of the `whoami` Pod to the local port 8888
- Check that the url `http://localhost:8888/api` returns the expected result with the command curl `curl http://localhost:8888/api` or with a browser if you use minikube locally.
- Delete the `whoami` Pod

<div class="pb"></div>
