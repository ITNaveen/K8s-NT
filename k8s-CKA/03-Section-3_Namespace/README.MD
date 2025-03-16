Namespace is the way to isolate same kind of resources in one logical abstract like one NS for monitoring consisting of prometheus, grafana and another NS for logging like kibana.

Also with NS, we can allow access to someone with least priveledge basis using RBAC and also allocate quota for that specific NS like 1GB memory and 1 CPU core etc.

Here, i have created NS for grade-submission and then created all pods in this NS 
- kubectl apply -f . -n grade-submission
or we can rather define NS in manifest file after the container name in metadata - namespace: grade-submission

