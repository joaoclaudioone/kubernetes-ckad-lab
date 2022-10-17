# Challenge 01
Build this environment

![Alt text](./challenge.png?raw=true "Challegen Diagram")

## Specifications

### User

- Build user information for martin in the default kubeconfig file: User = martin , client-key = /root/martin.key and client-certificate = /root/martin.crt
- Create a new context called 'developer' in the default kubeconfig file with 'user = martin' and 'cluster = kubernetes'


<details>
<summary><b>Solution</b></summary>

```
kubectl config set-credentials martin \
  --client-certificate=/root/martin.crt \
  --client-key=/root/martin.key

kubectl config set-context developer \
  --cluster=kubernetes --user=martin
```

</details>


### Developer-role
- 'developer-role', should have all(*) permissions for services in development namespace
- 'developer-role', should have all permissions(*) for persistentvolumeclaims in development namespace
- 'developer-role', should have all(*) permissions for pods in development namespace

[Solution](role.yaml)

### Developer-rolebinding
- create rolebinding = developer-rolebinding, role= 'developer-role', namespace = development
- rolebinding = developer-rolebinding associated with user = 'martin'

[Solution](rolebinding.yaml)

### Persistent Volume 
This is already created in the challenge

[Persistent Volume][pv.yaml]

### Persist Volume Claim
- Storage Request: 1Gi
- Access modes: ReadWriteMany
- pvc name = jekyll-site, namespace = development
- 'jekyll-site' PVC should be bound to the PersistentVolume called 'jekyll-site'.

[Solution](pvc.yaml)

### Pod

- pod: 'jekyll' has an initContainer, name: 'copy-jekyll-site', image: 'kodekloud/jekyll'
- initContainer: 'copy-jekyll-site', command: [ "jekyll", "new", "/site" ] (command to run: jekyll new /site)
- pod: 'jekyll', initContainer: 'copy-jekyll-site', mountPath = '/site'
- pod: 'jekyll', initContainer: 'copy-jekyll-site', volume name = 'site'
- pod: 'jekyll', container: 'jekyll', volume name = 'site'
- pod: 'jekyll', container: 'jekyll', mountPath = '/site'
- pod: 'jekyll', container: 'jekyll', image = 'kodekloud/jekyll-serve'
- pod: 'jekyll', uses volume called 'site' with pvc = 'jekyll-site'
- pod: 'jekyll' uses label 'run=jekyll'

[Solution](pod.yaml)

### Service
- Service 'jekyll' uses targetPort: '4000', namespace: 'development'
- Service 'jekyll' uses Port: '8080', namespace: 'development'
- Service 'jekyll' uses NodePort: '30097', namespace: 'development'

[Solution](service.yaml)

### kube-config
- set context 'developer' with user = 'martin' and cluster = 'kubernetes' as the current context.

<details>
<summary><b>Solution</b></summary>

```
kubectl config set-context developer
```

</details>