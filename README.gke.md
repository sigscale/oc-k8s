# Manage Google Kubernetes Engine (GKE)

## Create a (tiny) GKE Cluster
	$ gcloud container clusters create cluster-1 \
			--project sigscale-ocs
			--zone asia-east1-b \
			--machine-type e2-small \
			--disk-size 10 \
			--num-nodes 1

## List the k8s clusters available on GKE
	$ gcloud container clusters list
	NAME: cluster-1
	LOCATION: asia-east1-b
	MASTER_VERSION: 1.21.5-gke.1302
	MASTER_IP: 35.229.186.29
	MACHINE_TYPE: e2-small
	NODE_VERSION: 1.21.5-gke.1302
	NUM_NODES: 3
	STATUS: RUNNING

## Configure `kubectl` to use the Cluster
	$ gcloud container clusters get-credentials cluster-1
	Fetching cluster endpoint and auth data.
	kubeconfig entry generated for cluster-1.

## Create and initialize OCS Deployment
	$ kubectl apply -f ocs-cluster/
	service/diameter-acct created
	service/diameter-auth created
	statefulset.apps/ocs created
	service/ocs-rest created
	service/ocs-snmp created
	service/otp-ocs created
	secret/otp-dist created
	service/radius-acct created
	service/radius-auth created

## Get list of Pods
	$ kubectl get pods
	NAME    READY   STATUS    RESTARTS   AGE
	ocs-0   1/1     Running   0          64m
	ocs-1   1/1     Running   1          63m
	ocs-2   1/1     Running   0          62m

## Attach to a GKE pod deployment which runs im container
	$ kubectl attach -ti ocs-0 -c ocs -i -t
	If you don't see a command prompt, try pressing enter.

## Scale up the OCS Cluster
	$ kubectl scale --replicas=4 statefulset ocs
	statefulset.apps/ocs scaled

## Delete the OCS Cluster
	$ kubectl delete -f ocs-cluster/
	service "diameter-acct" deleted
	service "diameter-auth" deleted
	statefulset.apps "ocs" deleted
	service "ocs-rest" deleted
	service "ocs-snmp" deleted
	service "otp-ocs" deleted
	secret "otp-dist" deleted
	service "radius-acct" deleted
	service "radius-auth" deleted

## Get all the PersistentVolumes
	$ kubectl get pvc

## Delete PVC
	$ kubectl delete pvc ocs-data-ocs-{0,1,2}
	$ kubectl delete pvc ocs-log-ocs-{0,1,2}

## Delete the GKE Cluster
	$ gcloud container clusters delete cluster-1 --zone asia-east1-b --project sigscale-ocs

