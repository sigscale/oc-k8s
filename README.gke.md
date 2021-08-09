# Google Kubernetes Engine (GKE)

## Create a k8s cluster
	$ gcloud container clusters create cluster-1
			--machine-type g1-small \
			--disk-size 10 \
			--num-nodes 1

## List the k8s clusters available on GKE
	$ gcloud container clusters list
	NAME       LOCATION      MASTER_VERSION  MASTER_IP       MACHINE_TYPE  NODE_VERSION    NUM_NODES  STATUS
	cluster-1  asia-east1-b  1.20.8-gke.900  35.194.241.248  g1-small      1.20.8-gke.900  1          RUNNING

## Get authentication credentials and configure kubectl to use the cluster
	$ gcloud container clusters get-credentials cluster-1
	Fetching cluster endpoint and auth data.
	kubeconfig entry generated for cluster-1.

## Create and initialize OCS Deployment
	$ kubectl apply -f ocs-demo.yaml
	deployment.apps/sigscale-ocs created

## Get list of Deployments
	$ kubectl get deployments
	NAME           READY   UP-TO-DATE   AVAILABLE   AGE
	sigscale-ocs   1/1     1            1           12s

## Get list of Pods
	$ kubectl get pods
	NAME                            READY   STATUS    RESTARTS   AGE
	sigscale-ocs-5559f96986-vwt8p   1/1     Running   0          24s


## Add DIAMETER Ro Service
	$ kubectl apply -f diameter-ro.yaml
	service/diameter-ro created

## Add RADIUS Accounting Service
	$ kubectl apply -f radius-acct.yaml
	service/diameter-ro created

## Add RADIUS Authorization Service
	$ kubectl apply -f radius-auth.yaml
	service/radius-acct created

## Add REST Service
	$ kubectl apply -f ocs-rest.yaml
	service/radius-acct created

