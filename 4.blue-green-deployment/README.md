## Blue/Green deployment
Persistance storage can help us with a blue/green deployment. In order to create them use next command
```
gcloud compute disks create webcontent-blue webcontent-green --zone europe-west1-d --size 1GB
```
Create temporary container with NGINX
```
kubectl create -f ops.yaml
```
Check deployment
```
kubectl get deployments
```
Check pod is created
```
kubectl get pods
```
Connect to pod you got in output
```
kubectl exec -it ops-xxxxxxxxxx-xxxxx -- bash
```
Or use next command if you are very lazy
```
kubectl exec -it `kubectl get pods | grep ops | awk '{print $1}'` -- bash
```
Now you are inside the container. Copy all files from `/usr/share/nginx/html/` to mounted persistance disks.
```
cp /usr/share/nginx/html/* nginx-blue/
cp /usr/share/nginx/html/* nginx-green/
```
Change `index.html` files content using `sed`
```
sed -i -e 's/Welcome to nginx!/Welcome to blue nginx!/g' nginx-blue/index.html
sed -i -e 's/Welcome to nginx!/Welcome to green nginx!/g' nginx-green/index.html
```
Double check files exist and content is correct `Welcome to blue nginx!` is in the `nginx-blue/index.html` and `Welcome to green nginx!` is in the `Welcome to green nginx!`
```
cat /nginx-blue/index.html
cat /nginx-green/index.html
```
If everything is ok then close the connection
```
exit
```
And delete the `ops` deployment
```
kubectl delete -f ops.yaml
```
