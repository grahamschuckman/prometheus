# prometheus
Testing out Prometheus for K8s monitoring of Wiz Unified Helm Chart components.

Wanted to test on GKE Autopilot but could not due to built-in security restrictions.
    - Specifically the node exporter requires a host-volume which is not allowed by Autopilot.

Switching to minikube running locally on my Mac for the remainder of this exercise.

Installed prometheus via helm using this command:
    - `helm install prometheus prometheus-community/kube-prometheus-stack`

1. Grep for port 9090 to find the prometheus service we want:
    - `kubectl get svc | grep 9090`
    - Note: It is the `prometheus-kube-prometheus-prometheus` one with the ClusterIP, not the `prometheus-operated` service

2. 