# rhsso-install

```
oc login -u developer

oc new-project sso

oc policy add-role-to-user view system:serviceaccount:$(oc project -q):default

oc create -f https://raw.githubusercontent.com/jboss-openshift/application-templates/master/secrets/sso-app-secret.json

oc login -u system:admin
```

### Update core set of RH-SSO 7.2 resources for OpenShift in the `openshift` project:

```
for resource in sso72-image-stream.json \
  sso72-https.json \
  sso72-mysql-persistent.json \
  sso72-mysql.json \
  sso72-postgresql-persistent.json \
  sso72-postgresql.json
do
  oc replace -n openshift --force -f \
  https://raw.githubusercontent.com/jboss-openshift/application-templates/ose-v1.4.9/sso/${resource}
done

oc -n openshift import-image redhat-sso72-openshift:1.0
```

```
oc login -u developer

oc new-app --template=sso72-mysql-persistent -p SSO_ADMIN_USERNAME=admin -p SSO_ADMIN_PASSWORD=admin -p HTTPS_NAME=jboss -p HTTPS_PASSWORD=mykeystorepass

```

## Routes
`Create secure and non-secure routes`
