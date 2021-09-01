# Manage Google Kubernetes Engine (GKE)

## Create a (tiny) GKE Cluster
	$ gcloud container clusters create cluster-1
			--machine-type g1-small \
			--disk-size 10 \
			--num-nodes 1

## Configure `kubectl` to use the Cluster
	$ gcloud container clusters get-credentials cluster-1
	Fetching cluster endpoint and auth data.
	kubeconfig entry generated for cluster-1.

## Delete teh GKE Cluster
	$ gcloud container clusters delete cluster-1

