When we define container replicas in DEPLOYMENT file then it tell REPLICASET_CONTROLLER to spin up that many controller with responsibily to maintain them as well.

For deployment, in SPEC we define REPLICAS and labels and then in TEMPLATE pick pod defination from metadata onwards.
with this MATCHLABELS (where we define label) creates link between deployment and pod being created.



