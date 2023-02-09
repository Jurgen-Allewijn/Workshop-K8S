helm install stable/prometheus --name prometheus --namespace monitoring

helm install stable/grafana --name grafana --namespace monitoring

kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo