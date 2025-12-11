# 2_k8s_configmaps_yaml_and_commands.md

# ✅ ConfigMaps – YAML, Usage, and Commands

---

## 4. ConfigMap YAML (From Notes)


apiVersion: v1
kind: ConfigMap
metadata:
name: configmap # name of the ConfigMap
data:
app_name: portfolio
app_version: latest
database_url: localhost

 

- `data:` holds non-sensitive key–value pairs in plain text.   

---

## 5. Deployment Using ConfigMap (From Notes)

apiVersion: apps/v1
kind: Deployment
metadata:
name: my-deployment
spec:
replicas: 3
selector:
matchLabels:
app: demo
template:
metadata:
name: my-pod
labels:
app: demo
spec:
containers:
- name: con1
image: nginx
ports:
- containerPort: 80
env:
- name: APP_NAME
valueFrom:
configMapKeyRef:
name: configmap # ConfigMap name
key: app_name # key in ConfigMap

 

Explanation:

- `APP_NAME` gets value from `configmap.data.app_name` (`portfolio`).   

---

## 6. Basic ConfigMap Commands

1️⃣ Create ConfigMap from YAML:

kubectl apply -f configmap.yaml

 

2️⃣ List ConfigMaps:

kubectl get cm

or
kubectl get configmap

 

3️⃣ Describe a ConfigMap:

kubectl describe cm configmap

 

4️⃣ Delete a ConfigMap:

kubectl delete cm configmap

 

---

## 7. Verify ConfigMap Injection Inside Pod

1️⃣ Apply Deployment:

kubectl apply -f deployment.yaml

 

2️⃣ List Pods:

kubectl get pods

 

3️⃣ Check environment variables inside pod:

kubectl exec -it <pod-name> -- env
 
 

You should see:

- `APP_NAME=portfolio`  

This confirms the ConfigMap is successfully consumed as env vars.  
