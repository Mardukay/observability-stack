# observability-stack

1. Install cert manager

   - flux manifest monitoring/cert-manager/cert-manager.yaml

2. Create namespace - flux manifest monitoring/kbot-monitoring/ns.yaml

3. Install opentelemetry operator

   - Add [https://github.com/open-telemetry/opentelemetry-operator/releases/latest/download/opentelemetry-operator.yaml]( https://github.com/open-telemetry/opentelemetry-operator/releases/latest/download/opentelemetry-operator.yaml ) to flux

4. Install grafana

   - create persistent volume - flux manifest monitoring/kbot-monitoring/grafana-pvc.yaml
   - deploy grafana - flux manifest monitoring/kbot-monitoring/grafana.yaml

5. Install loki

   - deploy loki - flux manifest monitoring/kbot-monitoring/loki.yaml

6. Install prometheus

   - add rbac for prometheus - flux manifest monitoring/kbot-monitoring/prometheus-rbac.yaml
   - deploy prometheus - flux manifest monitoring/kbot-monitoring/prometheus.yaml

7. Launch grafana dashbord and create datasources loki and prometheus

   - kubectl port-forward pod/{grafana-pod-name} --namespace=kbot 3000:3000
   - in connections/data sources create loki datasource with connection url http://loki:3100
   - in connections/data sources create loki datasource with connection url http://prometheus-service:9090

8. Install otel collector

   - flux manifest monitoring/kbot-monitoring/otel-collector.yaml

9. Install fluent-bit

   - create rbac - flux manifest monitoring/kbot-monitoring/fluent-bit-rbac.yaml
   - create configmap - flux manifest monitoring/kbot-monitoring/fluent-bit-cm.yaml
   - deploy fluentbit  - flux manifest monitoring/kbot-monitoring/fluent-bit.yaml

10. Deploy kbot

    - create secret with telegram token - flux manifest monitoring/kbot-monitoring/kbot-secret.yaml
    - deploy kbot app - flux manifest monitoring/kbot-monitoring/kbot.yaml

Screenshots from grafana dashboard

   1. Grafana datasources![](https://github.com/Mardukay/observability-stack/blob/main/images/grafana-datasporces.png)

   2. Prometheus metric from kbot that show total count of hello message![](https://github.com/Mardukay/observability-stack/blob/main/images/prometheus-hello-count.png)

   3. Prometheus metric from kbot that show total count of hello message after more messages![](https://github.com/Mardukay/observability-stack/blob/main/images/prometheus-hello-count-change.png)

   4. Loki logs from cluster![](https://github.com/Mardukay/observability-stack/blob/main/images/loki-logs.png)

       

       

       

  TODO:

  1. Optimize flux deploy
  2. Unfortunately, Loki now shows logs only from his pods. Find out what the reason is and fix it.
  3. Learn more about how to instrument the application, as the test application currently uses examples from https://github.com/den-vasyliev/kbot/tree/opentelemetry/otel

  

  

