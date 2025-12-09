# Phase 1: Building Internal Developer Platform (IDP) with Red Hat Developer Hub

## Re-Setup with new Cluster

1. Provision Demo Environment

https://catalog.partner.demo.redhat.com/catalog?item=babylon-catalog-prod/partner.rhdh-use-case.prod&utm_source=webapp&utm_medium=share-link


3. Replace Cluster URLs with new Cluster ID


- Find a cluster URL somewhere in the resources (e.g. rhdh/rhdh/5-app-config-rhdh.yaml)
- Replace the Cluster ID
  - E.g. source "cluster-xg2zk.dynamic" target "cluster-9kpjf.dynamic"

4. Check it in


3. Deploy a generic ArgoCD instance (if not already available)

3b.

Change in 5-app-config-rhdh.yaml: Argo admin password

4. Deploy the ArgoCD apps in folder ArgoCD

5. In ArgoCD

- on "keycloak-deploy"
  - First separately sync the Demo Project, operator subscription and Operator Group 
  - Wait until the Keycloak Operator Installation is done
  - Sync the rest, should sync and progress into "healthy"

6. Login in Keycloak 

- Find Secret demo-project/demo-keycloak-instance-initial-admin
- Find Route demo-project/demo-keycloak-instance
- Login on 2 with creds from 1

7. In ArgoCD

- on "devhub-deploy"
    - First separately sync the Demo Project, operator subscription and Operator Group
    - Wait until the RHDH Operator Installation is done
    - Sync the rest, should sync, but devhub pods will fail
- Retrieve the token from secret rhdh-k8s-sa-token and base64-decode it
- Edit rdhd/mainual/rdhd-secret.yaml (copy from *-example if not existing)
  - Set token as K8S_SA_TOKEN
  - Set Github classic token as GITHUB_TOKEN 
- Apply everything from rhdh/manual
- 
## Troubleshooting


- Devhub crashes with "MigrationLocked: Plugin 'catalog' startup failed; caused by MigrationLocked: Migration table is already locked"
  - Downscale Devhub and its PSQL
  - Delete PVC and PV
  - Scale up PSQL again until pod ready
  - SCalu up devhub again
