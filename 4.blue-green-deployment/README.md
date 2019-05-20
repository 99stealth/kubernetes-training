## Blue/Green deployment
1. Persistance storage can help us with a blue/green deployment. In order to create them use next command
```
gcloud compute disks create webcontent-blue webcontent-green --zone europe-west1-d --size 1GB
```
2. Create temporary container with NGINX
```
kubectl create -f ops.yaml
```
3. Check deployment
```
kubectl get deployments
```
⋅⋅⋅And check pod is created
```
kubectl get pods
```
4. Connect to pod you got in output
```
kubectl exec -it ops-xxxxxxxxxx-xxxxx -- bash
```
⋅⋅⋅Or use next command if you are very lazy
```
kubectl exec -it `kubectl get pods | grep ops | awk '{print $1}'` -- bash
```
5. Now you are inside the container. Copy all files from `/usr/share/nginx/html/` to mounted persistance disks.
```
cp /usr/share/nginx/html/* nginx-blue/
cp /usr/share/nginx/html/* nginx-green/
```
⋅⋅⋅Change `index.html` files content using `sed`
```
sed -i -e 's/Welcome to nginx!/Welcome to blue nginx!/g' nginx-blue/index.html
sed -i -e 's/Welcome to nginx!/Welcome to green nginx!/g' nginx-green/index.html
```
⋅⋅⋅Double check files exist and content is correct `Welcome to blue nginx!` is in the `nginx-blue/index.html` and `Welcome to green nginx!` is in the `Welcome to green nginx!`
```
cat /nginx-blue/index.html
cat /nginx-green/index.html
```
6. If everything is ok then close the connection
```
exit
```
⋅⋅⋅And delete the `ops` deployment
```
kubectl delete -f ops.yaml
```
