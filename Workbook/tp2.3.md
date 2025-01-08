
## Lab 2.3: Annotations and Namespaces

Goals :
- view and add annotations on a Pod
- display Namespaces
- create a Namespace
- create resources in a defined Namespace

Duration: 0:20:00

### Annotations

- Check help for the `kubectl annotate` command
- View existing annotations on the `yaml-pod` Pod
- Add `super.mycompany.com/ci-build-number=722` annotation to `yaml-pod` Pod
- Check that the annotation is present on the Pod

### Namespaces

- Display the list of all Namespaces
- Show all Pods in the `kube-system` Namespace
- Show all Pods from all Namespaces
- List all pods with label `run=yaml-pod` from all namespaces

### Creating a new Namespace

- Create a new namespace `my-sandbox` with a manifest file defining the label `sandbox="true"`

### Using the new Namespace

- Instantiate the `yaml-pod` Pod in the `my-sandbox` Namespace by modifying the Pod descriptor
- List the Pods in the `my-sandbox` Namespace
- Delete the `my-sandbox` Namespace

<div class="pb"></div>
