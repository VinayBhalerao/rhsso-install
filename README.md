# Install RH-SSO 7.3 on Openshift 3.11

```
oc login -u developer

oc new-project sso

oc policy add-role-to-user view system:serviceaccount:$(oc project -q):default

oc create -f https://raw.githubusercontent.com/jboss-openshift/application-templates/master/secrets/sso-app-secret.json

```

### Update core set of RH-SSO 7.2 resources for OpenShift in the `openshift` project:

```
oc login -u system:admin

for resource in sso73-image-stream.json \
  sso73-https.json \
  sso73-mysql.json \
  sso73-mysql-persistent.json \
  sso73-postgresql.json \
  sso73-postgresql-persistent.json \
  sso73-x509-https.json \
  sso73-x509-mysql-persistent.json \
  sso73-x509-postgresql-persistent.json
do
  oc replace -n openshift --force -f \
  https://raw.githubusercontent.com/jboss-container-images/redhat-sso-7-openshift-image/sso73-dev/templates/$resource
done

oc -n openshift import-image redhat-sso73-openshift:1.0
```

```
oc login -u developer

oc new-app --template=sso73-mysql-persistent -p SSO_ADMIN_USERNAME=admin -p SSO_ADMIN_PASSWORD=admin -p HTTPS_NAME=jboss -p HTTPS_PASSWORD=mykeystorepass

```

## Routes
`Create secure and non-secure routes`
