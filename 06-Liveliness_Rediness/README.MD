SELF_HEALING - when pod is down then with this k8s restart this automatically.
It restarts if fails.

LIVENESS - It should be running and responsive and not going for infinate loop.
It restarts if fails.
its added inside container template under the image - 
livenessProbe:   
  httpGet:
    path: /healthz
      port: 3000
  initialDelaySeconds: 15        #after 15 seconds of container started.
  periodSeconds: 5               #there after every 5 sec.

READINESS - Its able to serve traffic and able to connect to external component.
If this fails then it stop any traffic to reach to the pod.
Just live LIVENESS and goes under it - 
redinessProbe:
  httpGet:
    path: /readyz
    port: 3000

Aspect	         Liveness Probe	                                         Readiness Probe
Purpose	         Checks if the Pod is alive and not stuck.	             Checks if the Pod is ready to serve traffic.
Action            on Failure	Restarts the Pod.	                        Removes the Pod from the Service's endpoints.
Pod State	       Pod is terminated and recreated if failed.	           Pod remains running but marked as "Not Ready".
Use Case	       Detect unresponsive or stuck Pods.                   	Manage traffic routing during         Pod                                                                     initialization or recovery.