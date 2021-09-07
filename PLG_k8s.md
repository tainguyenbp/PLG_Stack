# Logging in Kubernetes with Loki and the PLG Stack
# Install the PLG Stack with Helm
### The PLG Stack (Promtail, Loki and Grafana)
```
Promtail is an agent that needs to be installed on each node running your applications or services. It detects targets (such as local log files), attaches labels to log streams from the pods, and ships them to Loki.
Loki is the heart of the PLG Stack. It is responsible to store the log data.
Grafana is an open-source visualization platform that processes time-series data from Loki and makes the logs accessible in a web UI.
```
# Install the PLG Stack with Helm 1
### The PLG Stack (Promtail, Loki and Grafana)
```
kubectl create namespace loki-logging
```
### Add Loki Helm Chart repository:
```
helm repo add grafana https://grafana.github.io/helm-charts
```
### Run the following command to update the repository
```
helm repo update
```
### Deploy Loki Stack (Loki, Promtail, Grafana, Prometheus)
```
helm upgrade --install loki grafana/loki-stack --set grafana.enabled=true,prometheus.enabled=true,prometheus.alertmanager.persistentVolume.enabled=false,prometheus.server.persistentVolume.enabled=false
```

### Run the following command to update the repository
```
helm uninstall loki --namespace=loki
```


# Install the PLG Stack with Helm 2
### The PLG Stack (Promtail, Loki and Grafana)
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

kubectl get secret loki-grafana -n default -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
```

### Access the Grafana UI with NodePort
```
kubectl get svc -n loki-logging 

kubectl port-forward --namespace loki-logging service/loki-logging-grafana 3000:80

kubectl port-forward --namespace loki-logging service/loki-logging-grafana --address 0.0.0.0 3000:80

kubectl port-forward service/loki-grafana --address 0.0.0.0 3000:80
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
### Remove config helm
```
helm uninstall loki-logging -n loki-logging
```

# Reference: 
```
https://codersociety.com/blog/articles/cloud-native-tools
https://codersociety.com/blog/articles/loki-kubernetes-logging
https://codersociety.com/blog/articles/helm-best-practices
https://aws.amazon.com/cn/blogs/china/from-elk-efk-to-plg-implement-in-eks-a-container-logging-solution-based-on-promtail-loki-grafana/
https://grafana.com/docs/loki/latest/installation/helm/
https://github.com/grafana/loki/tree/main/production/helm
https://artifacthub.io/packages/helm/grafana/loki?modal=template&template=_helpers.tpl
https://grafana.github.io/loki/charts/

```
