cat alias k=kubectl >> /.bashrc && source .bashrc

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

when editing a replica set the existing pods would remain with previous configuration, pods or rs should be recreated

kubectl get rs myapp-replica -o yaml

To get resources from all namespaces, not only default -> get resource_type -A
To get resources from an specific namespace, not from default -> get resource_type -n namespace_name
To get running pods on a node -> kubectl describe node node_name

kubectl create deployment pingpong --image juanbol/pingpong
kubectl get all
kubectl scale deployment pingpong --replicas=3
To get logs from a random pod of a deployment
kubectl logs deploy/pingpong --tail 3 --follow
To get logs from multiple pods use a selector
kubectl logs -l app=pingpong --tail 1 -f
stern --tail 1 --timestamps pingpong

kubectl apply -f deployment.yaml

If the rs is scaled up or down it will return to its previous config because the deployment manages the rs

To create containers for an specific jobs, only restarts on failure
kubectl create cronjob my-job --image=busybox --schedule="*/1 * * * *" -- date
kubectl get po -w