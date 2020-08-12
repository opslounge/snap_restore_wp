### Snap and restore a volume kubernetes
``


## create snapshot class CSI
```
kubectl apply -f https://raw.githubusercontent.com/purestorage/helm-charts/master/pure-csi/snapshotclass.yaml
```
---

Make some changes to your data on sql
---

## create and apply snapshot of volume
```
vim snapshot.yaml
apiVersion: snapshot.storage.k8s.io/v1alpha1
kind: VolumeSnapshot
metadata:
  name: mysql-pv-snapshot
spec:
  snapshotClassName: pure-snapshotclass
  source:
    name: mysql-pv-claim
    kind: PersistentVolumeClaim

```
kubectl apply -f snapshot.yaml
```
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
kubectl get deployments
kubectl scale --current-replicas=1 --replicas=0 deployment/wordpress-mysql

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
kubectl scale --current-replicas=0 --replicas=1 deployment/wordpress-mysql
```
## App is now restored to prior state
```
kubectl get pods 
```
refresh browser to see that site has reverted to previous config


