In order to have data stored persistently, we need to provision a storage for DB pods and this we can do with PVC - In Persistent volume claim we define how much storage we need and what kind as well.
Once that is done then K8s control plane will try to find a PERSISTENT VOLUME that satisfied the requirenment mentioned in the PVC.
Once the right PV is found then it get mounted with local dir of that DB pod where data is getting saved.

when you create a StatefulSet with a storage configuration, it will create a separate PersistentVolumeClaim (PVC) for each replica.

once the pvc and deployment is up along with service for DB, i need to connect api with mongo db and for this i need to define ENV in api deployement inside container template - 
          env:
            - name: MONGODB_HOST
              value: mongodb
            - name: MONGODB_PORT
              value: '27017'

By default MongoDB config doesnt require auth but we can set this up and we need to this in the actual statefulset.yaml as env var (under the image) - 
env:
          - name: MONGO_INITDB_ROOT_USERNAME
            value: "admin"
          - name: MONGO_INITDB_ROOT_PASSWORD
            value: "password123"
and for API to connect to this MONGODB, we need to add username and passord as env var in API deployment as well (in specs under image)
        env:
          - name: MONGO_INITDB_ROOT_USERNAME
            value: "admin"
          - name: MONGO_INITDB_ROOT_PASSWORD
            value: "password123"
      
if pv or pvc creating issue while deleting then do this - 
kubectl scale statefulset mongodb --replicas=0 -n grade-submission
