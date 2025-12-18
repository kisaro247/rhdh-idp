# Phase 1: Building Internal Developer Platform (IDP) with Red Hat Developer Hub

## Re-Setup with new Cluster

1. Provision Demo Environment

https://catalog.partner.demo.redhat.com/catalog?item=babylon-catalog-prod/partner.rhdh-use-case.prod&utm_source=webapp&utm_medium=share-link


3. Replace Cluster URLs with new Cluster ID

- Find a cluster URL somewhere in the resources (e.g. rhdh/rhdh/5-app-config-rhdh.yaml)
- Replace the Cluster ID
    - E.g. source "cluster-xg2zk.dynamic" target "cluster-9kpjf.dynamic"

2b. In Github

- Create a classic access token
- Create an OAuth App
    - Name rhdh
    - Homepage: `https://keycloak-rhdh-operator.appls.cluster-<guid>.dynamic.redhatworkshops.io/realms/rhdh/account`
    - Auth Callback: `https://keycloak-rhdh-operator.appls.cluster-xg2zk.dynamic.redhatworkshops.io/realms/rhdh/broker/github/endpoint`
    - Register
    - COpy Client ID
    - Generate client secret
    - Copy client secret
    - Click "Update Application"
- In keycloak/keycloak/6-keycloak-realm.yaml, KeycloakRealmImport resource
    - -Replace clientId/Secret unter identityProviders

- Adapt branch names across all YAML files to match your branch (should be "phase-*" originally)"
- Commit and push everything to github

3. Deploy a generic ArgoCD instance (if not already available)

4. Deploy the ArgoCD apps in folder ArgoCD

5. In ArgoCD

- on "keycloak-deploy"
  - First separately sync the Demo Project, operator subscription and Operator Group 
  - Wait until the Keycloak Operator Installation is done
  - Sync the rest, should sync and progress into "healthy"

5b.

- Edit rdhd/manual/rdhd-secret.yaml (copy from *-example if not existing)
    - Set token as secret data field K8S_SA_TOKEN
    - Set Github classic token as secret data field GITHUB_TOKEN
- Apply everything from rhdh/manual

 
9. Login in Keycloak 

- Find Secret rhdh-demo/demo-keycloak-instance-initial-admin
- Find Route rhdh-demo/demo-keycloak-instance
- Login on 2 with creds from 1

7. In ArgoCD

- on "devhub-deploy"
    - First separately sync the Demo Project, operator subscription and Operator Group
    - Wait until the RHDH Operator Installation is done
    - Sync the rest, should sync, but devhub pods will fail
- Retrieve the token from secret rhdh-k8s-sa-token and base64-decode it


## Additional infos

This is a project template:
https://github.com/kisaro247/RHDH_Golden_Path/blob/main/all-templates.yaml

Import via Self Service -> Existing Repo
 
## Troubleshooting


- Devhub crashes with "MigrationLocked: Plugin 'catalog' startup failed; caused by MigrationLocked: Migration table is already locked"
  - Downscale Devhub and its PSQL
  - Delete PVC and PV
  - Scale up PSQL again until pod ready
  - SCalu up devhub again
