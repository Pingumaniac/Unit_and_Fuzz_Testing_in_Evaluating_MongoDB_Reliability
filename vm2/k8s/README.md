# K8s

### Set Up
1. Set up K8 with `vm2` as master and `vm3` and `vm4` as workers

Next, run 2-9 on `vm2`.

2. Install Local Path Provisioner
    ```
    kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.30/deploy/local-path-storage.yaml
    ```
3. Create service to connect Mongo nodes
    ```
    kubectl apply -f mongo-service.yaml
    ```
4. Check service creation with
    ```
    kubectl get service
    ```
5. Create 2 Mongo containers on the 2 worker VMs with Statefulset
    ```
    kubectl apply -f mongo-statefulset.yaml
    ```
6. Check pod creation with
    ```
    kubectl get pods
    ```
7. Enter into MongoDB instance
    ```
    kubectl exec -it mongo-0 -- mongo
    ```
8. Create 2 replica sets
    ```
    rs.initiate({
        "_id" : "rs0",
        "members" : [
            {
                    "_id" : 0,
                    "host" : "mongo-0.mongo.default.svc.cluster.local:27017",
            },
            {
                    "_id" : 1,
                    "host" : "mongo-1.mongo.default.svc.cluster.local:27017",
            }
        ]
    })
    ```
9. Ensure replica sets are initialized by running this command in the mongo shell on all VMs
    ```
    kubectl exec -it mongo-[0|1] -- mongo

    rs.status()
    ```
