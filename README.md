# Airbyte Local Installation on K3s

This document outlines the steps to deploy Airbyte locally using K3s and a custom/dummy Helm chart.

## Prerequisites

* [**K3s without Traefik**](https://docs.k3s.io/quick-start): Ensure your local cluster is up and running [without traefik](https://docs.k3s.io/faq?_highlight=disable%3Dtraefik#how-can-i-use-my-own-ingress-instead-of-traefik).
* [**Helm**](https://helm.sh/docs/intro/install/): Required to install the charts.

---

## Installation Steps

### 1. Update Dependencies
Download Chart dependencies before run it.

```bash
helm dependency update cert-manager &&\
helm dependency update ingress-controller &&\
helm dependency update postgres &&\
helm dependency update airbyte
```

### 2. Deploy the Charts
Run your specific Helm install command for the dummy chart.

```bash
helm upgrade --install cert-manager cert-manager -f cert-manager/values.yaml -n cert-manager --create-namespace &&\
kubectl apply -f cert-manager/selfsigned-issuer.yaml &&\
helm upgrade --install ingress-controller ingress-controller -f ingress-controller/values.yaml -n ingress-nginx --create-namespace &&\
helm upgrade --install postgres postgres -f postgres/values.yaml -n database --create-namespace --wait &&\
helm upgrade --install minio minio -f minio/values.yaml -n storage --create-namespace &&\
helm upgrade --install airbyte airbyte -f airbyte/values.yaml -n airbyte --timeout 20m
```

---

## Accessing Airbyte

### 1. Hosts Alias
Create alias in your /etc/hosts file (or similar to windows).
```bash
[K3S_HOST_IP] airbyte.homelab.local
```

### 2. Web Access
Access airbyte from your browser.
```bash
https://airbyte.homelab.local
```
### 3. First Access Credential
On your first access you will be asked to an e-mail, give one and use password defined in values.yaml "airbyte.global.auth.instanceAdmin.password".

---

[Jean Rodrigo](https://www.linkedin.com/in/jeanrodrigop/)
