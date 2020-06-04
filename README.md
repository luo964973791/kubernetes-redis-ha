### 一、部署的版本是helm3,下载redis到本地

```javascript
helm pull stable/redis-ha
```

### 二、解压redis

```javascript
tar zxvf redis-ha-4.4.4.tgz
cd redis-ha
vi values.yaml
#取消注释，并添加storageClass,这里使用rook-ceph搭建的，所以填rook-ceph
storageClass: "rook-ceph"
```

### 三、部署三个pvc

```javascript
kubectl create namespace redis

vi redis-pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-redis-redis-ha-server-1
  namespace: redis
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 6Gi
  storageClassName: rook-cephfs
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-redis-redis-ha-server-2
  namespace: redis
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 6Gi
  storageClassName: rook-cephfs
---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data-redis-redis-ha-server-1
  namespace: redis
spec:
  accessModes:
  - ReadWriteMany
  resources:
    requests:
      storage: 6Gi
  storageClassName: rook-cephfs
```

### 四、启动redis集群

```javascript
helm install redis -f values.yaml stable/redis-ha -n redis
```



###  
