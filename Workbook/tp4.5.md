
## Lab 4.5: Headless services

Goal: test the implementation of a _Headless_ service

Duration: 0:05:00

### Restore previous configuration for whoami ReplicaSet

- This exercice is in the _Namespace_ `shopping`
- Restore the previous `whoami` ReplicaSet configuration without the _Readiness_ probe
- Delete _Pods_ to ensure they are created without the probe

### whoami headless

- Create a new `whoami-headless` Service that redirects traffic to all Pods that match `app=whoami,level=expert` on port _80_, but type _Headless_
- Run the command `kubectl exec gateway -- nslookup whoami-headless` and check that the result is the expected one

<div class="pb"></div>
