# Phase 1: Building Internal Developer Platform (IDP) with Red Hat Developer Hub

## Re-Setup with new Cluster

1. Provision Demo Environment

https://catalog.partner.demo.redhat.com/catalog?item=babylon-catalog-prod/partner.rhdh-use-case.prod&utm_source=webapp&utm_medium=share-link


3. Replace Cluster URLs with new Cluster ID


- Find a cluster URL somewhere in the resources (e.g. rhdh/rhdh/5-app-config-rhdh.yaml)
- Replace the Cluster ID
  - E.g. source "cluster-xg2zk.dynamic" target "cluster-9kpjf.dynamic"

4. Check it in


3. Deploy a generic ArgoCD instance 