At the moment we are running PORTAL at NODEPORT (so one node specifically) but in PROD we have many NODES that make our ClUSTER and each node with differnt IP and so its hard to keep a track of each IP of each node.
IP address is very dynamic and rely on IP address to access our app is not good.

we now gonna use INGRESS and our cluster will run on multiple node and each node will run INGRESS CONTROLLER.
we will have a DOMAIN then a LB (from aws), now with LB it will load balance across our worker nodes and recipient will be an NGINX INGRESS CONTROLLER (running on node) and it acts as a reverse proxy, this is how it works - 
User Request → Domain → Load Balancer → Node's Nginx Ingress → Service → Pod
Here NGINX INGRESS CONTROLLER is deployed on node as a DAEMONSET.

Example scenario:
User hits api.example.com
DNS resolves to Load Balancer IP
LB forwards to any available node (Node1 or Node2 etc.)

Node's Nginx Ingress Controller:
Reads Ingress rules
Determines target service
Routes to correct pod

now for PORTAL, i have changed NODEPORT to CLUSTERIP and with this it will rely on INGRESS CONTROLLER to receive traffic from outside.
INGRESS is an entry point to our k8s cluster.

Now i need to install NGINX INGRESS CONTROLLER - 
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.8.2/deploy/static/provider/cloud/deploy.yaml

make sure to define ingressClassName when deploy ingress for any service, it should come under service.