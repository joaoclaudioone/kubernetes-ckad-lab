# Challenge 02
This 2-Node Kubernetes cluster is broken! Troubleshoot, fix the cluster issues and then deploy the objects according to the given architecture diagram to unlock our Image Gallery!!
Click on each icon to see more details. Once done click on Check button to test your work.

## Resources

### Controlplane

1. Fix kube config
- kubeconfig = /root/.kube/config, User = 'kubernetes-admin' Cluster: Server Port = '6443'
```
Fix the port inside the file  /root/.kube/config
```
2. Change the path of the client certificate inside, you can check this when you run a docker ps -a and the api-server  container is down. Check the logs with docker logs <container id>
- Fix kube-apiserver. Make sure its running and healthy.
```
# find the correct ca file 
ls -l /etc/kubernetes/pki/*.crt
# edit the parameter --client-ca-file 
vi /etc/kubernetes/manifests/kube-apiserver.yaml
```
Wait the container to be up and running. If this take much time restart the kubelet 
```
systemctl restart kubelet
```
3. Change master node version
- Master node: coredns deployment has image: 'k8s.gcr.io/coredns/coredns:v1.8.6'
```
# check for pods with errors
kubectl get pods -n kube-system
# edit the deployment, and change the image of the pod coredns
kubectl edit deployment -n kube-system coredns
```
### Node01
- node01 is ready and can schedule pods?
```
kubectl get nodes
```
We can see that node01 is in state Ready,SchedulingDisabled. This usually means that it is cordoned, so...
```
kubectl uncordon node01
```
https://kubernetes.io/docs/reference/kubectl/overview/

### /web
- Copy all images from the directory '/media' on the controlplane node to '/web' directory on node01

This is a simple scp copy.
```
scp /media/* node01:/web
```

### Persistent Volume
- Create new PersistentVolume = 'data-pv'
- PersistentVolume = data-pv, accessModes = 'ReadWriteMany'
[Solution](pv.yaml) 

### Persisten Volume Claim

- Create new PersistentVolumeClaim = 'data-pvc'
- PersistentVolume = 'data-pvc', accessModes = 'ReadWriteMany'
- PersistentVolume = 'data-pvc', storage request = '1Gi'
- PersistentVolume = 'data-pvc', volumeName = 'data-pv'
[Solution](pv.yaml) 

### gop-file-server
- Create a pod for fileserver, name: 'gop-fileserver'
- pod: gop-fileserver image: 'kodekloud/fileserver'
- pod: gop-fileserver mountPath: '/web'
- pod: gop-fileserver volumeMount name: 'data-store'
- pod: gop-fileserver persistent volume name: data-store
- pod: gop-fileserver persistent volume claim used: 'data-pvc'
[Solution](pod.yaml) 

### go-fs-service
- New Service, name: 'gop-fs-service'
- Service name: gop-fs-service, port: '8080'
- Service name: gop-fs-service, targetPort: '8080'
[Solution](svc.yaml) 