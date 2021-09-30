# Install Beeinstana operator

## 1. Create Namespace:

Create the namespace for the beeinstana operator to run in
```
kubectl create namespace beeinstana-operator-system
```
## 2. Create secrets

Create the Secrets needed to pull images from the Instana container registry, use the `Agent_Key` as a password.
```
kubectl create secret docker-registry instana-registry --docker-server=https://containers.instana.io --docker-username=_ --docker-password=<Agent_Key>
```

## 3. Edit the storageclass

The Beeinstana operator uses `PersistentVolumes` for data storage. Depending on your kubernetes variant (kubernetes, GKE, EKS, AKS) you might need to change the `provisioner` for volumes in `storageclass.yaml`.


## 4. Edit the Beeinstana custom ressource

Beeinstana consumes metrics from Kafka using the `ingestor` component. For this, you will need to add a list of Kafka nodes to the `ingestor` spec in `beeinstana.cr.yaml`.

Depending on the size of your deployment and amount of data expected, you will need to change the CPU, Memory and Storage limits set in `beeinstana.cr.yaml` to suit your needs. 

You can also configure the amounts of `shards` and `mirrors` for the `aggregator` component. 

PLEASE NOTE: the `aggregator` can only be scaled up. Removing `mirrors` or `shards` is currently not supported

## 5. Apply to the cluster

Apply using the `kustomize` parameter.

```
kubectl apply -k .
````