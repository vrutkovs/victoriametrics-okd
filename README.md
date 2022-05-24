VictoriaMetrics on OKD
====

This repo contains manifests which replace in-cluster Prometheus with VictoriaMetrics instance.

In detail it does the following:
* Installs VictoriaMetrics operator from Community catalog
* Sets up VMCluster with vmselect/vminsert/vmstorage (2 pods for each)
* Installs VMAgent to handle ServiceMonitors
* Unmanages Cluster Monitoring Operator and scales down Prometheus / Thanos Querier
* Runs an nginx proxy to make sure Thanos/Prometheus requests from console are routed to VictoriaMetrics
* Updates prometheus/thanos-querier services to point to VictoriaMetrics / proxy
* Runs VMAlert to make sure recording rules from PrometheusRules are evaluated

HOWTO:
* Install OKD cluster
* Run `oc apply -f` multiple times until all manifests are applied

TODO/FIXME:
* Service spec selectors need to be cleaned up manually
  Prom operator creates services with selectors which we need to replace in order to point to VictoriaMetrics.
  `oc apply` appends labels to selectors instead of replacing them.
  Note: try `oc replace`

* Cluster not upgradable
  Since we need to scale down CMO we override deployment in CVO settings.
  Workaround: remove CVO override before upgrade

* VMAlertmanager missing
