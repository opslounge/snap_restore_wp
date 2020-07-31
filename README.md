### Snap and restore a volume kubernetes
``


## create snapshot class CSI
```
kubectl apply -f https://raw.githubusercontent.com/purestorage/helm-charts/master/pure-csi/snapshotclass.yaml
```
---

Make some changes to your data on sql
---

## create snapshot of volume
```
vim snapshot.yaml
kubectl apply -f snapshot.yaml
```
## apply snapshot
```
kubectl apply -f snapshot.yaml --validate=false
```
## list snapshots 
```
kubectl get volumesnapshots
```
## describe snapshots
```
kubectl describe volumesnapshot/
```
## set volume to retain volume reclaim policy
```
kubectl patch pv pvc-5a4bae38-6029-41bf-8023-96237d7758ee -p '{"spec":{"persistentVolumeReclaimPolicy":"Retain"}}'
```
## scale the pod to 0 replicas
```
kubectl scale --replicas=0 rs/wordpress-mysql-56ff98b6b
```
## delete the PVC
```
kubectl delete pvc mysql-pv-claim
```
## restore the volume
```
kubectl apply -f restore.yaml --validate=false
```
## scale up the replicas 
```
kubectl scale --replicas=1 rs/wordpress-mysql-56ff98b6b
```
ls
kubectl get pods 
