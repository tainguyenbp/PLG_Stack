# Logging in Kubernetes with Loki and the PLG Stack
# Install the PLG Stack with Helm
### The PLG Stack (Promtail, Loki and Grafana)
```
Promtail is an agent that needs to be installed on each node running your applications or services. It detects targets (such as local log files), attaches labels to log streams from the pods, and ships them to Loki.
Loki is the heart of the PLG Stack. It is responsible to store the log data.
Grafana is an open-source visualization platform that processes time-series data from Loki and makes the logs accessible in a web UI.
```
### Create a Kubernetes namespace
```
kubectl create namespace loki-logging
```
### Add Loki Helm Chart repository:
```
helm repo add loki https://grafana.github.io/loki/charts

```
### Run the following command to update the repository
```
helm repo update
```
### Create a Kubernetes namespace
```
helm upgrade --install loki loki/loki-stack -n loki-logging --set grafana.enabled=true

helm upgrade --install loki-logging loki/loki-stack -n loki-logging --set grafana.enabled=true

kubectl get pods -n loki-logging
```
### Get password Grafana:
```
kubectl get secret -n loki-logging

kubectl get secret loki-logging-grafana -n loki-logging

kubectl get secret loki-logging-grafana -n loki-logging -o yaml

kubectl get secret loki-logging-grafana -n loki-logging -o jsonpath="{.data.admin-password}"

kubectl get secret loki-logging-grafana -n loki-logging -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

### Access the Grafana UI with NodePort
```
kubectl get svc -n loki-logging 
kubectl port-forward --namespace loki-logging service/loki-logging-grafana 3000:80
```
### Access the Grafana UI with ClusterIP and Ingress
```
kubectl get svc -n loki-logging 
kubectl edit svc loki-logging-grafana -n loki-logging
Add config the below:
  externalIPs:
  - 192.168.10.5
  - 192.168.10.6
  - 192.168.10.7
```

# Reference: 
```
https://codersociety.com/blog/articles/cloud-native-tools
https://codersociety.com/blog/articles/loki-kubernetes-logging
https://codersociety.com/blog/articles/helm-best-practices
```
