
### Run a Wordpress Instance in kubernetes


#### prerequisites

you need access to a kubernetes cluster and kubetctl tool installed on your desktop.


#### helm vs. kubectl

If you'd prefer to use Helm instead of kubectl, simply go to the "helm" directory, edit the values.yaml file as needed, and install the building blocks. Done! Otherwise, proceed as described.



#### Create Kubernetes Secrets for MySQL Password

First, create a Kubernetes secret that will store the MySQL database password securely. Replace "RANDOMPASSWORD" with a strong password:

```
kubectl create secret generic mysql-secret --from-literal=password="RANDOMPASSWORD"
```


#### create all the necessary services


You need these components to run a WordPress instance:

MySQL - for database management.


WordPress - the web application itself including php8.1 and apache2 web server


```

kubectl apply -f mysql.yaml

kubectl apply -f wordpress.yaml
```

In the result, you will have a Wordpress instance running on port  80. 

```
NAME                TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
mysql-service       ClusterIP   10.240.24.121   <none>        3306/TCP   12d
wordpress-service   ClusterIP   10.240.18.12    <none>        80/TCP     115m

```

Check the external IP of the ingress-nginx-controller and create a correspondig DNS A entry for your domain

```
kubectl  get -n ingress-nginx svc ingress-nginx-controller
NAME                       TYPE           CLUSTER-IP     EXTERNAL-IP       PORT(S)                      AGE
ingress-nginx-controller   LoadBalancer   10.240.21.69   195.192.156.198   80:32613/TCP,443:31651/TCP   4h27m
```

```
kubectl  get ingress
NAME                CLASS   HOSTS            ADDRESS           PORTS     AGE
wordpress-ingress   nginx   go-bornholm.de   195.192.156.198   80, 443   132m
```

After that, you browse to your wordpress config page and complete the setup.
Do not forget to create a regulary backup of your wordpress data!

