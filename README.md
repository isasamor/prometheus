PROMETHEUS 
helm init (version 2)
git clone https://github.com/helm/charts.git
cd charts/stable/prometheus
#need to change storage for production
helm install --name=prometheus . --namespace monitoring --set rbac.create=true
export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace monitoring port-forward $POD_NAME 9090


----------------------------------------
GRAFANA 
cd charts/stable/grafana
helm install stable/grafana --set persistence.enabled=true --set persistence.accessModes={ReadWriteOnce} --set persistence.size=8Gi -n grafana --namespace monitoring
helm ls --all
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
export POD_NAME=$(kubectl get pods --namespace monitoring -l "app=grafana,release=grafana" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace monitoring port-forward $POD_NAME 3000
#for production - need to add ingress 
------
http://www.allaboutwindowssl.com/2019/03/setup-prometheus-grafana-monitoring-on-azure-kubernetes-cluster-aks/
