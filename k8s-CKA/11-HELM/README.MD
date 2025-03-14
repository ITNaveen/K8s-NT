HELM installation - 
brew install helm
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

HELM simplify deployment process - Helm packages all related Kubernetes resources for a single application into one "Chart"

now create values.yaml, where i define values for micrpservice and this what i will ref to depoyment,svc,configmap etc. so far i have made namespace and service name as var and also updated in all files in API.
once that is done then do - helm template . (all should be autofilled with corrected values)

Now once you have defined all the values and ref them to all files then its time to PACKAGE THAT - 
helm package . (should be done from the microservice folder) and it save a tgz file in the same folder.

now helm install package_name_u_want that_tgz_file
if you want to delete a package - 
helm list
helm uninstall package_name_u_want