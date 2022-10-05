# This project is for my k8s learning

### Getting started

`echo alias k=kubectl >> ~/.bashrc && source ~/.bashrc`

```
kubectl api-resources
kubectl api-versions

cd k8s_manifests
kubectl create -f pod-definition.yml
kubectl create -f service-defintion.yml
kubectl get pods
kubectl describe pod myapp-pod
kubectl get services
kubectl describe service myapp-service
kubectl create -f replicaset-definition.yml
kubectl get rs
kubectl describe rs myapp-replica
kubectl scale rs myapp-replica --replicas=3
```

When editing a replica set the existing pods would remain with previous configuration, pods or rs should be recreated

To get yaml file from an existing resource:
`kubectl get rs myapp-replica -o yaml`

To get resources from all namespaces, not only default -> get resource_type -A
To get resources from an specific namespace, not from default -> get resource_type -n namespace_name
To get running pods on a node -> kubectl describe node node_name

Checking logs on a deployment and its scaling:
```
kubectl create deployment pingpong --image juanbol/pingpong
kubectl get all
kubectl scale deployment pingpong --replicas=3
To get logs from a random pod of a deployment
kubectl logs deploy/pingpong --tail 3 --follow
To get logs from multiple pods use a selector
kubectl logs -l app=pingpong --tail 1 -f
stern --tail 1 --timestamps pingpong
```

Creating same deployment with yaml file
`kubectl apply -f deployment.yaml`

If the rs is scaled up or down it will return to its previous config because the deployment manages the rs

To create containers for an specific jobs, that only restarts on failure:
```
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- date
kubectl get po -w
```

Checking how replica sets work as load balancing
```
kubectl create deployment httpenv --image=bretfisher/httpenv --replicas=3
kubectl expose deployment httpenv --port=8888   
kubectl apply -f shpod.yml
kubectl attach -n shpod -ti shpod
IP=$(kubectl get svc httpenv -o go-template --template '{{ .spec.clusterIP }}')
curl -s 10.97.229.34:8888 | jq .HOSTNAME
```

Each service has endpoints (pod host+port), to check them do
```
kubectl describe svc httpenv
kubectl get endpoints httpenv -o yaml
kubectl get po -l app=httpenv -o wide
```

To check how a service can forward to multiple deployments:
```
kubectl create deployment nginx --image nginx
kubectl expose deployment nginx --port 80 --type NodePort
kubectl edit service nginx
#Edit spec selector to add myapp=web
kubectl label pods  -l app=nginx myapp=web
kubectl create deployment httpd --image httpd
kubectl label pods  -l app=httpd myapp=web
```

Rolling updates:
```
kubectl apply -f docker-coins.yml
#Edit image of rs on worker deployment to v0.2, watch pods, rs and deploys
kubectl get po -w -n coins
kubectl get rs -w -n coins
kubectl get deploy -w -n coins
kubectl diff -f docker-coins.yml
kubectl apply -f docker-coins.yml
#Edit image of rs on worker deployment to an unexisting image, keep watching
kubectl rollout status deploy worker -n coins
kubectl describe deploy worker -n coins
#Check Replicas section and Events from below output
kubectl rollout undo deploy worker -n coins
kubectl rollout history deploy worker -n coins
kubectl rollout undo deploy worker -n coins --to-revision=1
```
