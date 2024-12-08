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