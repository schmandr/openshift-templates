# pgAdmin4

## First install and update of app in an Openshift Environment

First create secret pgadmin4-secret for pgadmin4 admin user (username and password in keepass) 

```
apiVersion: v1
kind: Secret
metadata:
  name: pgadmin4-secret
  labels:
    app: pgadmin4
type: Opaque
stringData:
  PGADMIN_DEFAULT_EMAIL: xy
  PGADMIN_DEFAULT_PASSWORD: xy
```

All necassary components of the application are configured in the template pgadmin4.yaml.
Set env parameter to set environment
Set version parameter to set Image version
```
oc process -f pgadmin4.yaml \
  -p version=latest \
  -p scheduled=true \
  -p env="-integration" \ 
  -p CPU_LIMIT="800m" \
  -p MEMORY_LIMIT="800Mi" \
  -p CPU_REQUEST="20m" \
  -p MEMORY_REQUEST="400Mi" \
  | oc apply -f-
oc process -f pgadmin4.yaml \
  -p version=6.8 \
  -p CPU_LIMIT="800m" \
  -p MEMORY_LIMIT="800Mi" \
  -p CPU_REQUEST="20m" \
  -p MEMORY_REQUEST="800Mi" \
  | oc apply -f-
```
