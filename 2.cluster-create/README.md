## Create Kubernetes cluster with GKE
Before you create a new cluster you can choose the Kubernetes master version. In order to determine which versions are currently available in your zone run next command:
```
gcloud container get-server-config --zone europe-west1-d
```
In the output you can find `defaultClusterVersion`, `defaultImageType`. If you are ok with these defaults you can create a cluster using next command:

```
gcloud container clusters create "k8s-cluster" --zone "europe-west1-d" \
   --machine-type "custom-1-1024" --disk-size "100" --network "default" \
   --no-enable-cloud-logging --no-enable-cloud-monitoring \
   --enable-autoscaling --min-nodes="2" --max-nodes="10" \
   --enable-legacy-authorization
```
If you want to change these default master and nodes Kubernetes versions you can set `--cluster-version` choosen from `validMasterVersions`. 
Also, you can deploy Kubernetes Nodes using other images available in `validImageTypes`. If you want to set it specify valid one with `--image-type` option.
Use next example if you want to set `--cluster-version` and `--image-type`:
```
gcloud container clusters create "k8s-cluster" --zone "europe-west1-d" \
   --machine-type "custom-1-1024" --disk-size "100" --network "default" \
   --no-enable-cloud-logging --no-enable-cloud-monitoring \
   --enable-autoscaling --min-nodes="2" --max-nodes="10" \
   --enable-legacy-authorization \
   --cluster-version "COS_CONTAINERD" --cluster-version 1.13.5-gke.10
```