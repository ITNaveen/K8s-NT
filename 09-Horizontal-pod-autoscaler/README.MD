WITH HPA, k8s can scale up or down based on CPU utilization. we need to define upper limit and minimum and also target CPU utilization.

First i need to set up METRIX SEVER that will collect CPU utilization and then only HPA can work, here is the command to set it up - 
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml && kubectl patch deployment metrics-server -n kube-system --type='json' -p='[{"op": "add", "path": "/spec/template/spec/containers/0/args/-", "value":"--kubelet-insecure-tls"}]'

we can see CPU and memory utilization - kubectl top pods -n grade-submission

