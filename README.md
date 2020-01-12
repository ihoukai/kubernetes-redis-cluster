# Redis Cluster on Kubernetes

This k8s module is intended to simplify the creation and operation of a Redis Cluster deployment in Kubernetes.

## How it works

These directions assume some familiarity with [Redis Cluster](http://redis.io/topics/cluster-tutorial). 

When you create the resources in Kubernetes, it will create a 6-member (the minimum recommended size) [Stateful Set](https://kubernetes.io/docs/concepts/abstractions/controllers/statefulsets/) cluster where the first,second,third member become master and other members are slaves.

## Testing it out

To launch the cluster, have Kubernetes create all the resources in redis-cluster.yaml:

```
$ kubectl create -f redis-cluster.yaml
configmap/redis-cluster created
statefulset.apps/redis-cluster created
service/redis-cluster created
```

Wait a bit for the service to initialize.

Once all the pods are initialized, you can see that Pod "redis-cluster-0" "redis-cluster-1" "redis-cluster-2" became the cluster master with the other nodes as slaves.

```
$ kubectl exec -it redis-cluster-0 redis-cli cluster nodes
4d96dfa57c283287806aa3dcfe4f0493ac62bcaa 176.16.149.175:6379@16379 master - 0 1578664514520 2 connected 5461-10922
35cda411bef698a5fec4aa15e67f2b8307ea6a76 176.16.54.215:6379@16379 master - 0 1578664516532 3 connected 10923-16383
2abe495e12bdb58fad1e15362712b4b6b8968ddd 176.16.54.217:6379@16379 myself,master - 0 1578664515000 1 connected 0-5460
c7bb3b785998a0c2c856ec56582ff29f62bf1684 176.16.149.170:6379@16379 slave 35cda411bef698a5fec4aa15e67f2b8307ea6a76 0 1578664515024 6 connected
e7ca27c3ccf5b05fa5031570ba9ca490e28ff1ab 176.16.54.218:6379@16379 slave 4d96dfa57c283287806aa3dcfe4f0493ac62bcaa 0 1578664515928 5 connected
b8f739f3217fcab49e3a88b021ee2ad986d3cbc0 176.16.149.156:6379@16379 slave 2abe495e12bdb58fad1e15362712b4b6b8968ddd 0 1578664515528 4 connected
```
