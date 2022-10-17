# Challenge 3
Deploy the given architecture to vote namespace.

## Resources
### namepspace
- Create a new namespace: name = 'vote'
kubectl create ns vote

### vote-service
- Create a new service: name = vote-service
- port = '5000'
- targetPort = '80'
- nodePort= '31000'
- service endpoint exposes deployment 'vote-deployment'
[Solution](vote-svc.yaml)

### vote-deployment
- Create a deployment: name = 'vote-deployment'
- image = 'kodekloud/examplevotingapp_vote:before'
- status: 'Running'
[Solution](vote-deployment.yaml)

### redis
- New Service, name = 'redis'
- port: '6379'
- targetPort: '6379'
- type: 'ClusterIP'
- service endpoint exposes deployment 'redis-deployment'
[Solution](redis-service.yaml)

### redis.deployment
- Create new deployment, name: 'redis-deployment'
- image: 'redis:alpine'
- Volume Type: 'EmptyDir'
- Volume Name: 'redis-data'
- mountPath: '/data'
- status: 'Running'
[Solution](redis-deployment.yaml)

### worker
- Create new deployment. name: 'worker'
- image: 'kodekloud/examplevotingapp_worker'
- status: 'Running'
[Solution](worker.yaml)

### db-service
- Create new service: 'db'
- port: '5432'
- targetPort: '5432'
- type: 'ClusterIP'
[Solution](db-service.yaml)

### result-deployment 

Create new deployment, name: 'result-deployment'
image: 'kodekloud/examplevotingapp_result:before'
status: 'Running'

### result-service 

port: '5001'
targetPort: '80'
NodePort: '31001'
zoom_in
zoom_out
refresh
