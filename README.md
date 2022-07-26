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
kubectl delete rs myapp-replica
kubectl delete service myapp-service

when editing a replica set the existing pods would remain with previous configuration, pods or rs should be recreated

kubectl get rs myapp-replica -o yaml