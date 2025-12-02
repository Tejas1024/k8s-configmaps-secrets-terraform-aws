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

- ConfigMaps = non-sensitive data (plain text) [web:21][web:22][web:26]  
- Secrets = sensitive data (passwords, tokens, API keys) [web:26][web:33][web:40]  

---

## 2. ConfigMaps vs Secrets

**ConfigMaps**

- Store non-confidential configuration (DB URL, feature flags, app name). [web:21][web:26][web:33]  
- Data stored as plain key–value pairs. [web:21][web:26]  

**Secrets**

- Store confidential data (username, password, credentials, tokens). [web:26][web:33][web:40]  
- Values stored as base64-encoded strings under `data`. [web:18][web:33][web:40]  

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