
### Run a Wordpress Instance in kubernetes


#### prerequisites

you need access to a kubernetes cluster and kubetctl tool installed on your desktop.



#### create all the necessary services


```
kubectl create secret generic mysql-secret --from-literal=password="RANDOMPASSWORD"

kubectl apply -f mysql.yaml

kubectl apply -f redis.yaml

kubectl apply -f wordpress.yaml
```

In the result, you will have a Wordpress instance that uses an external IP and port 80. 
I recommend to use a service like Cloudflare. Create a DNS entry there for the external IP and redirect the traffic via HTTPS

```
NAME                                        READY   STATUS    RESTARTS   AGE
pod/mysql-0                                 1/1     Running   0          7d
pod/redis-deployment-dd5d9fc87-9psdw        1/1     Running   0          52m
pod/wordpress-deployment-7b94b69fd5-xfbl8   1/1     Running   0          56m

NAME                        TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)         AGE
service/kubernetes          NodePort       10.240.16.1     <none>           443:31837/TCP   9d
service/mysql-service       ClusterIP      10.240.24.200   <none>           3306/TCP        7d4h
service/redis-service       ClusterIP      10.240.23.87    <none>           6379/TCP        3d1h
service/wordpress-service   LoadBalancer   10.240.24.71    155.251.119.43   80:30007/TCP    7h14m

NAME                                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/redis-deployment       3/3     3            3           3d1h
deployment.apps/wordpress-deployment   1/1     1            1           56m

NAME                                              DESIRED   CURRENT   READY   AGE
replicaset.apps/redis-deployment-dd5d9fc87        3         3         3       3d1h
replicaset.apps/wordpress-deployment-7b94b69fd5   1         1         1       56m

NAME                     READY   AGE
statefulset.apps/mysql   1/1     7d4h
```

this is the wordpress service with the external IP you need to use for the DNS entry:

```
$kubectl get service wordpress-service 
NAME                TYPE           CLUSTER-IP     EXTERNAL-IP      PORT(S)        AGE
service/wordpress-service   LoadBalancer   10.240.24.71    155.251.119.43   80:30007/TCP    7h14m
```

After that, you browse to your wordpress config page and complete the setup.
Do not forget to create a regulary backup of your wordpress data!

