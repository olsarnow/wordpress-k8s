
kubectl create secret generic mysql-secret --from-literal=password="RANDOMPASSWORD"

kubectl create -f mysql-pvc.yaml

kubectl create -f mysql-statefulset.yaml

kubectl create -f mysql-service.yaml

kubectl create -f wordpress-pvc.yaml

kubectl create -f wordpress-deployment.yaml
