# 1_k8s_configmaps_intro.md

# ✅ Kubernetes ConfigMaps & Secrets – Concept Notes

---

## 1. What Are ConfigMaps?

ConfigMap is a Kubernetes object used to store **non-sensitive** configuration data such as:

- Database name  
- Application name  
- Application version  
- General environment variables  

Key idea:

- ConfigMaps = non-sensitive data (plain text)   
- Secrets = sensitive data (passwords, tokens, API keys)   

---

## 2. ConfigMaps vs Secrets

**ConfigMaps**

- Store non-confidential configuration (DB URL, feature flags, app name).   
- Data stored as plain key–value pairs.   

**Secrets**

- Store confidential data (username, password, credentials, tokens).   
- Values stored as base64-encoded strings under `data`.   

---

## 3. How ConfigMaps Are Used (Env Injection Concept)

Example values in ConfigMap:

- `app_name: myapp`  
- `data: sql`  

Used in Deployment as:

env:

name: APP_NAME
valueFrom:
configMapKeyRef:
name: configmap # ConfigMap name
key: app_name # Key from ConfigMap

text

This makes the value from `configmap.app_name` available in the container as:

- `APP_NAME=myapp` 
