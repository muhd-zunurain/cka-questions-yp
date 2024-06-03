## Question
Taint the worker node to be Unschedulable. Once done, create a pod called dev-redis, image redis:alpine to ensure workloads are not scheduled to this worker node. Finally, create a new pod called prod-redis and image redis:alpine with toleration to be scheduled on node01.

## Steps
### 1. Taint the node to be unscheduleable
`kubectl taint nodes <nodename> key1=val1:NoSchedule`\
Note: The key and value pair can be any name, as long as you have the `NoSchedule` effect.

### 2. Add `toleration` in pod manifest so that it will be scheduled.

## Example
1. Taint the node with `kubectl taint nodes node01 node-hostname=node01:NoSchedule`. Applying this will deny any pods to be scheduled to this node

2. Add `tolerations` section to your pods spec, so that it will still be scheduled. Refer to q53.yaml for more info.
```
spec:
  tolerations:
  - key: "node-hostname"
    operator: "Equals"
    value: "node01"
```
3. You can verify that the pod with `tolerations` is running successfully.
```
controlplane $ kubectl get pod
NAME         READY   STATUS    RESTARTS   AGE
dev-redis    0/1     Pending   0          6m14s
prod-redis   1/1     Running   0          5s
```

