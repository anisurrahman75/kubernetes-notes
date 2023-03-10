## DaemonSet


- It basically ensures that exactly a single copy of pod is running across a set of nodes 
- It's also used for scheduling one pod per subset of nodes
- ensures that all (or some) nodes run a copy of a pod
- As nodes are added to the cluster, pods are added
- As nodes are removed from the cluster, those pods are garbage collected
- Deleting a DaemonSet will clean up the pods it created

## Use Cases:


- Node monitoring daemons. Ex- collectd
- Log collection daemons. Ex- fluentd
- storage daemons. Ex- ceph


- ## Manifest File


- ### First Create Multiple Worker node
- ```yaml
  # three node (two workers) cluster config
  kind: Cluster
  apiVersion: kind.x-k8s.io/v1alpha4
  nodes:
  - role: control-plane
  - role: worker
  - role: worker
  
  ```
- Create Cluster:

    - $ kind create cluster --name k8s-playground --config kind-config.yaml

- Delete Cluster:

    - $ kind delete cluster --name k8s-playground


- ### DamonSet Manifest File:

    - 
      ```yaml
      apiVersion: apps/v1
      kind: DaemonSet
      metadata:
        name: fluentd-ds
      
      spec:
        selector:
          matchLabels:
            name: fluentd
        template:
          metadata:
            labels:
              name: fluentd
          spec:
            containers:
              - name: fluentd
                image: gcr.io/google-containers/fluentd-elasticsearch:1.20
      
      ```

## Create  and Display DaemonSet:


- $ kubectl create -f fluentd-daemonset.yaml
- $ kubectl get pods -o wide
- $ kubectl get ds


## Limiting DaemonSets:


- $ kubectl  get ds nginx-fast-storage  -o yaml > NodeSelectorDaemonSets.yaml
- Set nodeSelector label on pod spec\[ NodeSelectorDaemons.yaml ]
- $ kubectl apply -f NodeSelectorDaemonSets.yaml 
- $ kubectl get pods -o wide


## Delete DaemonSet:
- ## kubectl delete ds <daemonsetName>
