Now i have 2 microservices working and up but i need to connect them as they are froentend and backend, so I need service discovery - 

Every pod has a virtual IP but its accessible with cluster and to OPEN THE POD FOR OUTSIDE WORLD WE NEED NODEPORT (or LB or Ingress) - 
when we open for NODEPORT then it opens a static port in my machine or node between 30-32k and this way we get entry to our app in k8s.
NODEPORT TAKE EXTERNAL TRAFFIC AND FORWARD TO THE POD BASED ON LABEL AND SELECTOR BUT ITS NOT USED IN PROD.

Now we got entry in out k8s cluster on 32000 port and then we need to redirect this to PORAL-PORT (5001) and this will be done using LEBEL-SELECTOR (it has to be choosen on service and pod), for this KUBEPROXY see the label and check on pod for the same and find out the IP of pod and connect.

so now due to nodeport my pod is exposed on port 32000 but still cant process further than frontend because no connection with backend, so for that connection, i need to create clusterIP SERVICE FOR API POD and it will receive traffic on port 3000 from frontend then it will check label and then based on that it will forward traffic to pod (in our case its api pod).

Now under specs beneath container name i need to define an ENV VAR and with this my pod knows who to connect and it loooks like this -    
env:
      - name: GRADE_SERVICE_HOST
        value: grade-submission-api
This way i am defining backend service (clusterIP for backend) and thats how my frontend is connected to backend.
