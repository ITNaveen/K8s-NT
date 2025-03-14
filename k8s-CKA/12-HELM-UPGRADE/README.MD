Lets see all releases across all NAMESPACES in our cluster -
helm list -A (without A it shows all for current namespace)

we are seeing helm package in default namespace but it should be GRADE-SUBMISSION namespace rather, so -
helm uninstall grade-submission-api
helm package . (to create a fresh chart, .tgz is a chart)
now create package - helm install grade-submission-api -n grade-submission grade-submission-api-1.0.4.tgz

UPGRADE -
now we have new Image for API and we need to upgrade deployment using HELM, in value it V4 (updated image), with this we have a URI that container username, port, password and this makes configmap useless and so we dont need it.
for SECRET we need to make DATA a STRINGDATA, so it accept text as we will be using URI only.
This means my deployment has bit of updates.

we have helm package for API 1.0.4 and now i have new updated chart 1.0.5
helm upgrade grade-submission-api grade-submission-api-1.0.5.tgz -n grade-submission

we can also upgrade by staying in the same DIR and making changes then - 
helm upgrade grade-submission-api .  -n grade-submission (this gives us new revision, lets say - 3)

if we want to go back to previous revision (old deployment)-
helm rollback grade-submission-api 2 -n grade-submission

Now we will delete all from MONGODB as i want to deploy it via HELM. so secret, svc, stateful deleted for mongo.

