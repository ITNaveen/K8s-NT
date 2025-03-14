on the first container, i mean for portal - i have sidecar container running with this called health check and it will keep hitting the main one on port 5000 to see if the main container is healthy and get 200 code.

for second container API - i have done the same and have health checker for this main container to see if the main container is healthy or not.

the point is to make sure my main container is healthy and i should be able to see that - kubectl logs -f grade-submission-api -c grade-submission-api-health-checker

IN this api is POD and checker is second CONATAINER - This way i dont need to check anymore on UI because i can see my pod is healthy.

The kubectl port-forward command can be used to expose a pod's port (in this case, port 80) to a port on your local machine (in this case, port 8080). This allows you to access the pod from your local machine without exposing it externally through a Kubernetes service. command is - kubectl port-forward mypod 8080:80

now i have 2 microservices working and up but i need to connect them as they are froentend and backend, so 
