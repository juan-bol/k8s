cat alias k=kubectl >> /.bashrc && source .bashrc

kubectl create -f pod-definition.yml
kubectl create -f service-defintion.yml
kubectl get pods
kubectl describe pod myapp-pod
kubectl get services
kubectl describe service myapp-service
kubectl delete pod myapp-pod
kubectl create -f replicaset-definition.yml
kubectl get rs
kubectl describe rs myapp-replica
kubectl scale rs myapp-replica --replicas=3
kubectl delete rs myapp-replica
kubectl delete service myapp-service

when editing a replica set the existing pods would remain with previous configuration, pods or rs should be recreated

kubectl get rs myapp-replica -o yaml

To get resources from all namespaces, not only default -> get resource_type -A
To get resources from an specific namespace, no from default -> get resource_type -n namespace_name
To get running pods on a node -> kubectl describe node node_name

kubectl create deployment pingpong --image juanbol/pingpong
kubectl get all
kubectl scale deployment pingpong --replicas=3
kubectl logs deploy/pingpong --tail 3 --follow

If the rs is scaled up or down it will return to its previous config because the deployment manages the rs

