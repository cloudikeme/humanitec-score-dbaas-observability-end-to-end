DBaaS using CloudNativePG

# Install CNPG into cluster

kubectl apply --server-side -f \
https://github.com/cloudnative-pg/cloudnative-pg/releases/download/v1.26.0/cnpg-1.26.0.yaml

# confirm its running

kubectl get deployment -n cnpg-system cnpg-controller-manager

# Install postgresql-client(psql) for debugging the connection

to achieve this, first create our cnpg cluster yaml file:

```yaml
cat <<EOF > pg-cluster.yaml
> apiVersion: postgresql.cnpg.io/v1
kind: Cluster
metadata:
  name: ikeme-cluster
spec:
  instances: 1

  storage:
    size: 100M
  bootstrap:
    initdb:
      database: ikeme-cluster
      owner: cloudikeme
      secret:
        name: ikeme-cluster-secret
> EOF
```
Then secret secret.yaml:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: ikeme-cluster-secret
type: kubernetes.io/basic-auth
stringData:
  username: admin # required field for kubernetes.io/basic-auth
  password: t0p-Secret # required field for kubernetes.io/basic-auth
```


# Create resource definition for a postgres cluster using template driver_type  

what i just learned lol!

apiVersion: entity.humanitec.io/v1b1
kind: Definition
metadata:
  id: 5min-idp-
entity:

A resource definition 'entity' will take the following:

name: 
type: postgres
driver_type: humanitec/template
driver_inputs: 
  .values, 
  .values.templates,
  .values.templates.init
  .values.templates.manifests
  .values.templates.outputs
  .values.templates.secrets

criteria: criteria.app_id, criteria.class

after creating our resource definition, we appy it:
cd res-def
 humctl create -f pg-res-def.yaml

create resource class 'dbaas'

humctl api post "/orgs/${HUMANITEC_ORG}/resources/types/postgres/classes" \
  -d '{
  "id": "dbaas",
  "description": "Specifically to be used when using cloudnativePG or dbaaS"
}'

then apply matching criteria

humctl api PUT /orgs/${HUMANITEC_ORG}/resources/defs/ikeme-postgres/criteria --data '[{"env_type": "5min-local","class":"dbaas"}]'


