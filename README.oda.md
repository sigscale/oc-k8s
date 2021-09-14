# ODA Canvas

## Deploy a SigScale OCS cluster as an ODA Canvas Component
	kubectl apply -f oda-component

## Delete the OCS Component
	kubectl delete -f oda-component
	kubectl delete pvc ocs-data-ocs-{0,1,2}

