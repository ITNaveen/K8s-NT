# this one is for user to access cluster with administration purpose, we have 2 kinds of users - 
1. humans = admin, developers.
2. bots = apps, servicess, processes. 
they both need access to the clusters.

- kubernetes does not manage users natively, it needs files with users details, LDAP, certificates etc.
- all user access is manage by api server, so all request goes to api server then api server process them.

# How KUBE-API server authenticate them - 
1. username-password in static password file.
2. username-token in static token file.
3. use certificates.
4. use third party authentication.

# static password file - 
1. we can use username-password added in csv file and use this as a source for user info. file has 3 columns username, password, userid.
we then pass the file as an option to api server.
as you can see this is kubeapi options (systemd service configuration for the Kubernetes API server), so just add this and then restart kube-api server and thats it.
```yaml
ExecStart=/usr/local/bin/kube-apiserver \
  --advertise-address=${INTERNAL_IP} \
  --allow-privileged=true \
  --apiserver-count=3 \
  --authorization-mode=Node,RBAC \
  --bind-address=0.0.0.0 \
  --enable-swagger-ui=true \
  --etcd-servers=https://127.0.0.1:2379 \
  --event-ttl=1h \
  --runtime-config=api/all \
  --service-cluster-ip-range=10.32.0.0/24 \
  --service-node-port-range=30000-32767 \
  --v=2 \
  --basic-auth-file=user-details.csv #this one we have added for auth.
```
# kube-apiserver is the Central Command of Kubernetes
✅ Everything in Kubernetes goes through the API server (kube-apiserver).
✅ When you use kubectl, it secretly sends requests to kube-apiserver.
✅ If something is wrong with your cluster, checking kube-apiserver is key!

1. kubectl cluster-info (it will give me ip and port=6443).
2. curl -k -u "user1:password123" https://<master-node-ip>:6443/api/v1/pods .
3. 
curl -k https://<master-node-ip>:6443/healthz .
curl -k https://<master-node-ip>:6443/readyz .
curl -k https://<master-node-ip>:6443/version .

4. basic authentication (user-password in CSV), you need to send login details with every API request. Unlike session-based logins (like web apps), Kubernetes API doesn’t maintain a session for you.

# static token file - 
1. instead of password, we can have static token file. then we can add "-token.auth.file=user.token.detail.csv" .
2. In terms of authentication, use token as "Authorization: Bearer token_here".
3. curl -v -k  https://<master-node-ip>:6443/api/v1/pods --header "Authorization: Bearer gugbkuyuohhuiy8y8hnihhoihyoiih" .









