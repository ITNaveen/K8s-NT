Operators extend Kubernetes API with custom resources and intelligence about your specific application, while core controllers just maintain state of built-in Kubernetes resources.
Operators are built using special Kubernetes libraries like controller-runtime that let you write code to automate how your application runs. Think of it like writing down all your manual operational steps into code that Kubernetes can automatically execute.

Start fresh remove mongodb chart - helm uninstall mongodb -n grade-submission
lets list and then remove bitnami repo - 
helm list repo
helm repo remove bitnami

now lets add new mongo repo - helm repo add mongo https://mongodb.github.io/helm-charts

GENERALLY - i can add repo - helm repo add my-own https://charts.bitnami.com/bitnami then look for chart - helm search repo my-own/kibana

NOW I will install helm chart - helm install community-operator mongo/community-operator  --version 0.10.0 -n mongodb
with this pod, deployment, statefulset is up and running, if i see logs now - 
kubectl logs mongodb-kubernetes-operator-74966668c-fjddg  -n mongodb
Running ./manager
2024-12-08T10:47:07.107Z        INFO    manager/main.go:74      Watching namespace: mongodb
2024-12-08T10:47:07.107Z        INFO    manager/main.go:91      Registering Components.
2024-12-08T10:47:07.107Z        INFO    manager/main.go:104     Starting the Cmd.
it says its watching, why because operator watches for custom resources, operator will contuniely watch CRD to match desired state.

For better understanding, we need to read docs like what CRD is provided by which operator and how does it work etc.

i downloaded mongodb-community crd yaml file from github then paste it in mongodb and then i defined another file called mongodb secret.
echo -n "password123" | base64
cGFzc3dvcmQxMjM=