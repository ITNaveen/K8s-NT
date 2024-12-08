we need to seperate application relation info from env related details, its bad because for example i have ENV VAR in statefulset and i may need to update username or password and for this i dont have reconfigure statefulset again and again.

another point is i have added ENV VAR in API deployment and at the moment they are 3 but i might need to add more ENV VAR and so this will make deployment even longer and somewhere hard to manage.

We can store non senstive data in CONFIGMAP and BASE64 encoded sensitive data in SECRETS.
After that we can REFERENCE configmap and secrets to our deployment.

what i did i created a seperate folder for API and then added ENV VAR details there as key:value pair. I have then REF this configMap in deployment - 
          envFrom:
            - configMapRef:
                name: gs-api-configmap

I do the same for SECRETS, now when adding value in SECRETS in DATA, k8s assumes its BASE64 encoded and so we have to do the same.
if it has to be plain data then use STRINGDATA.
for BASE64, do - 
echo -n "admin" | base64

once its done then it should look like this - 
          envFrom:
            - configMapRef:
                name: gs-api-configmap
            - secretRef:
                name: GS-api-secrets

Same for mongoDB as well.