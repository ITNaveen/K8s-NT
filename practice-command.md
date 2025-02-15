1. rolling update to use new image - 
   kubectl set image deployment/deployment_name *=nginx:1.17
   then check rollout status - kubectl rollout status deployment/nginx-deployment
   
   Rollback - kubectl rollout undo deployment/deployment_name

# Replicaset - 
Create a ReplicaSet using httpd image with latest tag (ensure to specify as httpd:latest) and name it httpd-replicaset.
Apply labels: app as httpd_app, type as front-end.
Name the container httpd-container. Ensure the replica count is 4.

apiVersion: apps/v1
kind: ReplicaSet
metadata:               
  name: httpd-replicaset
  labels:                  # labels are rs object
    app: httpd_app
    type: front-end
spec:
  replicas: 4
  selector:
    matchLabels:            # tells rs which pod it should manage
      app: httpd_app
      type: front-end
  template:
    metadata:   
      labels:               # These labels are applied to every new Pod created by the ReplicaSet
        app: httpd_app
        type: front-end
    spec:
      containers:
      - name: httpd-container
        image: httpd:latest

# labels are used in 3 places - 
metadata.labels - "This is who I am" (the controller's own identity)
spec.selector.matchLabels - "These are the pods I'm looking for" (finding pods)
spec.template.metadata.labels - "This is how I'll label new pods I create" (labeling new pods)

 
