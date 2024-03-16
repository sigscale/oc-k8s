# Manage Google Kubernetes Engine (GKE)

## Create a (tiny) GKE Cluster
	$ gcloud container clusters create cluster-1 \
			--machine-type e2-small \
			--disk-size 10 \
			--num-nodes 3

## Configure `kubectl` to use the GKE Cluster
	$ gcloud container clusters get-credentials cluster-1
	Fetching cluster endpoint and auth data.
	kubeconfig entry generated for cluster-1.

## Create and initialize OCS StatefulSet
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

## Get the log of init container
	kubectl logs ocs-0 -c ocs-init

## Get the log of container
	kubectl logs ocs-0

## Describe the pod
	$ kubectl describe pod ocs-0

## Attach to a GKE pod deployment which runs ocs container
	$ kubectl attach -ti ocs-0 -c ocs -i -t
	(ocs@ocs-0.otp-ocs.default.svc.cluster.local)1>
### There is no `detach` so you'll need to kill the `kubectl attach` process

## Scale up the OCS StatefulSet
	$ kubectl scale --replicas=4 statefulset ocs
	statefulset.apps/ocs scaled

## Delete the OCS StatefulSet
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
	$ gcloud container clusters delete cluster-1

