IN kubernetes we have2 types of account.
1. user account. (used by humans).
2. service account. (used by machines).

- SA could be an account used by an app to interact with kubernetes cluster. prometheus use SA to pull api metrics for performance monitoring.

- jenkins used SA to deploy app in k8s cluster. Pods do not always need a Service Account. If the Kubernetes API is managing the pod (like scheduling or restarting it), the pod doesn’t need an SA. However, if a pod needs to communicate with the Kubernetes API—like fetching secrets, listing pods, or modifying deployments—it must authenticate using a Service Account token. Kubernetes automatically assigns a default SA to every pod, but for specific API access, we create a custom SA with proper permissions using RBAC.

- "Whoever needs to talk to the Kubernetes API server needs a Service Account (SA)."
Pods automatically get a default SA but need a custom SA for more permissions.
External apps (Jenkins, ArgoCD, Prometheus) also need SA tokens to authenticate.
RBAC controls what an SA can do.

# SA process - 
1. we create SA (lets say for my app - dashboard) - kubectl create sa dashboard-sa .
2. list all SA - kubectl get sa .
3. kubectl describe sa dashboard-sa . we see token because when we create sv we get token as well and this token must be used to authenticate with KUBE-API server. we see "dashboard-sa-token-kudbm" as token.
4. so when sa is created, it create an object then create a secret then store token in there, secret then linked to sa.
5. for example in our case we can access this token as - kubectl describe secret dashboard-sa-token-kudbm .
6. this token now can be used as bearer token to authenticate for kube-api server.
7. you can create SA then assign right permissions (RBAC) then use this token for third party to authenticate with kube-api.

# what about if this app is running on kubernetes cluste - 
IN this case create sa - then RABC then mount this secret in deployment file of that third party app - 
- i create my-app-sa (SA), then RBAC for this and then - mounting.
```yaml
containers:
        - name: my-app
          image: busybox
          command: ["/bin/sh", "-c", "sleep 3600"]
          volumeMounts:
            - name: sa-token   # name of vol mount of my choice, must match with vol down.
              mountPath: "/var/run/secrets/kubernetes.io/serviceaccount" # inside the container where i am mounting token.
              readOnly: true
      volumes:
        - name: sa-token
          secret:
            secretName: my-app-sa-token # so its a secret i got by create SA for this app.
```

# default SA - 
1. each ns has its own default sa. whenver a pod is created , sa and its token is automatically mounted with that pod.
so we can kubectl describe pod pod_name and see volume mount added as it has secret from the default sa in this ns.

2. mount path is - /var/run/secrets/kubernetes.io/serviceaccount (inside pod). if you check this part in pod then you see 3 secretfiles.
- ca.crt.
- namespace.
- token (this one is actual secret).

3. i can use my own sa instead by editing the deployment file - 
```yml
spec:
      serviceAccountName: my-sa  # 🔹 Attach the custom Service Account here (not inside `containers`).
      containers:
        - name: my-app
          image: busybox
          command: ["/bin/sh", "-c", "sleep 3600"]
```
4. i can add "automountServiceAccountToken: false" to avoid anyone to mount sa in deployment (add this pod - spec section).

# changes from 1.22 and 1.24 version on SA - 
Inside the pod at this location - /var/run/secrets/kubernetes.io/serviceaccount , we have ca.crt, namespace, token. when you copy this token and paste in "JWT.IO" then you see there is not expiry of this token.
- In 1.22, Kubernetes introduced short-lived, auto-expiring, and audience-bound tokens. kubectl create token my-sa --duration=10m .
- before 1.24 when sa created it created secret and token. this will automatically mount on any pod that uses that sa.
- with 1.24 when we create sa it doesnt automatically create secret or token. so if i have kubectl create sa dashboard-sa.
then i need to create token - kubectl create token dashboard-sa, now when you take and open this token using "JWT.IO" then we have expiry defined for this token and by default its an hour but we can pass argument to increase the expiry of the token -
(kubectl create token dashboard-sa --duration=2h).

🔹 Key Fixes & Improvements:
✅ Clarified that before 1.24, SA Secrets were auto-created and auto-mounted.
✅ Clearly stated that 1.24 removed auto token creation, requiring manual token requests.
✅ Refined JWT.IO explanation, linking it to token expiration.
✅ Improved readability & technical accuracy.




