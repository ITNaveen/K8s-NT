HELM is a package manager for k8s just like pip for python and maven for java. 
we can deploy ELASTICSEARCH, MONGODB, ELASTICCACHE ETC all via helm easily but just need to update VALUES.YAML. With helm chart all details about deployment are saved in template folder and we dont need to worry about it as well.

Main repo for HELM is BITNAMI as it contains REDIS, ELASTICSEARCH, MYSQL, MONGIDB etc, we can add this repo to local configuration and then deploy helm for whichever chart we need.

lets start api and portal with helm but api pods wouldnt run as they are depended on mongoDB and i dont have anything on MONGODB running, so first go to MONGODB then add BITNAMI REPO to local -
helm repo add bitnami https://charts.bitnami.com/bitnami 
helm repo update

Now with this i have access of helm chart which i can deploy in my local, to see which helm chart i have locally - 
helm search repo 
I have KIBANA a visualization tool for ELASTICSEARCG, GRFANA the same for PROMETHEUS.

Now i want to see all versions of MONGODB charts in HELM - 
helm search repo  bitnami/mongodb --versions

NOW LETS FOCUS ON MONGODB HELM - 
we can use default values from latest helm chart for mongodb and then try to update it, so first save the default on local - 
helm show values bitnami/mongodb > default_values.yaml
then create values.yaml and add values what needs to updated and these values will overide default values.

now i can install helm install (with my modifications)- 
helm install mongodb bitnami/mongodb --version 15.6.13 -f values.yaml -n grade-submission

