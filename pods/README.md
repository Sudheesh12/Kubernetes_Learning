# Pods:

Pods can be created in Kubernetes using both imperative and declarative way just like in docker,

**Imperative is by running the command:**

```bash
kubectl run nginx
```

**Declarative way is using JSON or YAML file and declaring the state of the pod in it**

## Imperative way of creating a pod:

The cmd to creating a pod is shown below:

```bash
#kubectl run pod-name --image=image-name/version

kubctl run nginx-pod --image=nginx/latest

```
![imperative](./images/image.png)


- Here the pod is first getting created and then in runnig state.
- the **Ready** `1/1` is the number of container in the pod. here it is showing there is one container in the pod.

## Declarative way of creating the pods:

We can use both `json` and `yaml` to write a **Kubernetes** configuration.

here we will be using the `json` format;

``` json
apiVersion: v1
kind : Pod
metadata:
  name: nginx-pod02
  labels:
    env: dev
    cloud: azure
    type: frontend
spec:
  containers:
   - name: nginx-cont-01
     image: nginx:latest
     ports:
      - containerPort: 80
```

- command to create the pod using a config file:

```bash
Kubectl create -f aaaa.yaml
```

- if you have made some mistakes which creating the pod or there is some mistakes in the config file, use the command below:
- - it will show you your pod config details and the information or step taken during the creation of the pod.

```bash
kubectl describe pod pod_name
```

- to live edit the configuration of the pod use the command below:

```bash
kubectl edit pod pod_name
```

- to interact with the pod we can us the below command:

```bash
kubectl exec -it pod_name -- /bin/bash     # for bash
kubectl exec -it pod_name -- sh            # for normal shell
```

 

## Trick to create your on `yaml` or `json` using imperative commands:

it is not always required to write your own `yaml` or `json` file. we can use the imperative command and dry run the creating of pod and ask the kubectl to give the output in `yaml` or `json`. With this output details we can create a file.

### Commands:

```bash
 kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > test_pod_yaml.yaml
 kubectl run nginx --image=nginx:latest --dry-run=client -o json > test_pod_json.json
```

### Outputs:

```bash
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx:latest
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o json
{
    "kind": "Pod",
    "apiVersion": "v1",
    "metadata": {
        "name": "nginx",
        "labels": {
            "run": "nginx"
        }
    },
    "spec": {
        "containers": [
            {
                "name": "nginx",
                "image": "nginx:latest",
                "resources": {}
            }
        ],
        "restartPolicy": "Always",
        "dnsPolicy": "ClusterFirst"
    },
    "status": {}
}
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > test_pod_yaml.yaml
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o json > test_pod_json.json

```
