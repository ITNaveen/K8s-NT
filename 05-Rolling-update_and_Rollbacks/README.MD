ROLLING-UPDATE - 
Here my current deployment on API is running on app_image with STATELESS tag and i want to do update fo image and lets say my new image has a tag of STATEFUL.

this way when i REAPPLY this deployment then it will start new pods first (in this case 2 new) and once they are up and running then it will terminate old one with OLD IMAGE.
NAME                                       READY   STATUS        RESTARTS   AGE
grade-submission-api-68d86c9744-wj689      1/1     Running       0          3s  ---new ones
grade-submission-api-68d86c9744-zvlfp      1/1     Running       0          9s
grade-submission-api-86f6464d8b-sjvjf      1/1     Terminating   0          11m  ----old ones 
grade-submission-api-86f6464d8b-xwk2p      1/1     Terminating   0          11m

ROLLBACK - 
Now if new image or deployment is not working then we can with the help of DEPLOYMENT CONTROLLER, come back to previous stage and so its called ROLLBACK.

kubectl rollout undo deployment/grade-submission-api -n grade-submission 
write down the deployment name that needs to get undone and thats it, app is in previous stage.

Strategy for ROLLING_UPDATE -
we can specify under specs, how this rolling update can work, this defination goes under the SPECS of DEPLOYMENT after selector or replicas.
strategy:
      type: RollingUpdate
      rollingupdate:
        maxUnavailable: 50%
        maxSurge: 1  ---- it says how many pods there can be for this deployment at anytime, this case it will be 3.