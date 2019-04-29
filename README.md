# rstudio-helm-chart

This resource deploys a Rstudio on the cloud and leveraging cloud scalable nature(using Google Kubernetes Engine) to support multiple users.

## Setup a Kubernetes Cluster.

Step-1: Before setting up a Kubernetes cluster we need to enable the Kubernetes Engine API.

Step-2: You can either using web based terminal provided by google(Google Cloud shell) or run the required command line interfaces on your own computers terminal.

Step-3: Ask Google cloud to create a "Kubernetes cluster" and a "default" node pool to get nodes from. Nodes represent the hardware and node pools will keep track how much of a certain type of hardware is required.
```
gcloud container clusters create \
  --machine-type n1-standard-2 \
  --num-nodes 2 \
  --zone us-east4-b \
  --cluster-version latest \
  <CLUSTERNAME>
```
To check if the cluster is initialized,
```
kubectl get node -o wide
```

Step-4: Give your account permissions to grant access to all the cluster-scoped resources(like nodes) and namespaced resources(like pods). RBAC(Role-based access control) should be used to regulate access to resources to a specific user based on the role.
```
kubectl create clusterrolebinding cluster-admin-binding \
  --clusterrole=cluster-admin \
  --user=<GOOGLE-EMAIL-ACCOUNT>
```
Create a "cluster-admin" role and a cluster role biding to bind the role to the user.

## Setup RStudio
### Setup Helm
Helm is a package manager for Kubernetes, useful for installing, upgrading and managing applications on a Kubernetes cluster.
Helm has two parts, a client(helm) and a server(tiller). Tiller runs as a pod inside of Kubernetes cluster in the kube-system namespace.

Step-1: Install Helm using the below command,
```
curl https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get | bash
```

Step-2: Initialize Helm on the Kubernetes cluster by setting up a "service account" for use by tiller in the namespace "kube-system".
```
kubectl --namespace kube-system create serviceaccount tiller
```

Step-3: Give the service account full permissions to manage the cluster.
```
kubectl create clusterrolebinding tiller --clusterrole cluster-admin --serviceaccount=kube-system:tiller
```

Step-4: Initialize helm and tiller by using the command below,
```
helm init --service-account tiller --wait
```
### Installing RStudio
Step-1: Clone the repo and run the following command to deploy the RStudio server using the following command,

```
helm install /path/to/rstudio-helm-chart-directory
```

# rstudio - Docker

The Dockerfile is on the "docker" branch of this repo.
 
