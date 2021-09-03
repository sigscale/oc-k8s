# Manage a SigScale OCS Cluster with `kubectl`

## Create a new OCS Cluster
	$ kubectl apply -f ocs-cluster
	service/diameter-acct created
	service/diameter-auth created
	statefulset.apps/ocs created
	service/ocs-rest created
	service/ocs-snmp created
	service/otp created
	secret/otp-dist created
	service/radius-acct created
	service/radius-auth created

## List the Created Objects
	$ kubectl get all -l app.kubernetes.io/name=ocs
	NAME        READY   STATUS    RESTARTS   AGE
	pod/ocs-0   1/1     Running   0          24m
	pod/ocs-1   1/1     Running   0          23m
	pod/ocs-2   1/1     Running   0          23m
	
	NAME                    TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
	service/diameter-acct   ClusterIP      10.44.6.205    <none>        3868/TCP       24m
	service/diameter-auth   ClusterIP      10.44.14.193   <none>        3869/TCP       24m
	service/ocs-rest        LoadBalancer   10.44.10.56    34.80.248.9   80:31707/TCP   24m
	service/ocs-snmp        ClusterIP      10.44.10.53    <none>        4161/UDP       24m
	service/radius-acct     ClusterIP      10.44.11.241   <none>        1813/UDP       24m
	service/radius-auth     ClusterIP      10.44.9.253    <none>        1812/UDP       24m

## Inspect a Pod Log
	$ kubectl logs --tail=5 ocs-1
	=PROGRESS REPORT==== 1-Sep-2021::08:22:52.783507 ===
	    application: ocs
	    started_at: 'ocs@ocs-1.otp.default.svc.cluster.local'
	
	Eshell V12.0.3  (abort with ^G)

## Attach to a Pod Console
	$ kubectl attach -it ocs-1
	Defaulted container "ocs" out of: ocs, ocs-chown (init), ocs-init (init)
	If you don't see a command prompt, try pressing enter.
	
	(ocs@ocs-1.otp.default.svc.cluster.local)1>
To disconnect type ctrl-P ctrl-Q

## Scale up the OCS Cluster
	$ kubectl scale --replicas=5 statefulset ocs
	statefulset.apps/ocs scaled
	
	graybook:sigscale-ocs-k8s vances$ kubectl get pods -l app.kubernetes.io/name=ocs
	NAME    READY   STATUS    RESTARTS   AGE
	ocs-0   1/1     Running   0          36m
	ocs-1   1/1     Running   0          36m
	ocs-2   1/1     Running   0          35m
	ocs-3   1/1     Running   0          37s
	ocs-4   1/1     Running   0          19s

## Delete the OCS Cluster
	$ kubectl delete -f ocs-cluster
	service "diameter-acct" deleted
	service "diameter-auth" deleted
	statefulset.apps "ocs" deleted
	service "ocs-rest" deleted
	service "ocs-snmp" deleted
	service "otp" deleted
	secret "otp-dist" deleted
	service "radius-acct" deleted
	service "radius-auth" deleted

