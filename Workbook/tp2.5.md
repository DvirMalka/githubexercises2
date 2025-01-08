
## Lab 2.5: Init Containers

Goals: add an _Init Containers_

Duration: 0:15:00

### Experiments

- Check __Init Containers__ specs with `kubectl explain`
- Duplicate the `whoami-and-clock` Pod descriptor (source: `~/workspaces/Lab2/pod--whoami-and-clock--2.4.yml`) to create a new `whoami-and-clock-with-init` Pod:
  - Add an __Init Container__:
    - called `timer`
    - which uses the `debian:11-slim` image
    - which loops displaying the date every 1 second for 15 seconds (with the following `bash` command `for i in {1..15}; do date; sleep 1s; done` for example)
- Launch the `whoami-and-clock-with-init` Pod
- Check with `watch kubectl get po whoami-and-clock-with-init` that the Pod takes more than 15 seconds to launch
- Access the logs of the `timer` container and check that they contain the date display as expected

<div class="pb"></div>
