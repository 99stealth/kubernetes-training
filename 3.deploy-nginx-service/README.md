## Create Nginx service
Let's deploy simple k8s service with nginx web server
```
kubectl create -f nginx-service.yml
```
Once service is created let's create deployment
```
kubectl create -f nginx.yml
```
Check pods
```
kubectl get pods
```
Or You can see list all pods in all namespaces
```
kubectl get pods --all-namespaces
```
Check service which was created
```
kubectl get svc
```
In the output you should get an `EXTERNAL-IP` of the `webserver` service. Try to connect to it via curl. 

```
curl xxx.xxx.xxx.xxx
```
Or you can use next command if you are very lazy :)
```
curl http://`kubectl get svc | grep webserver | awk '{print $4}'`
```
Congratulations you have created your first k8s service and it is a nice time to delete it :)
First of all you need to delete the `webserver` deployment
```
kubectl delete -f nginx.yml
```
Now it is a time to delete the service
```
kubectl delete -f nginx-service.yml
```
Or you can delete everything together
```
kubectl delete -f nginx.yml,nginx-service.yml
```