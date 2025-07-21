# prometheus
Testing out Prometheus for K8s monitoring of Wiz Unified Helm Chart components.

YouTube: https://www.youtube.com/watch?v=6xmWr7p5TE0

Wanted to test on GKE Autopilot but could not due to built-in security restrictions.
    - Specifically the node exporter requires a host-volume which is not allowed by Autopilot.

Switching to minikube running locally on my Mac for the remainder of this exercise.

Installed prometheus via helm using this command:
    - `helm install prometheus prometheus-community/kube-prometheus-stack`

1. Grep for port 9090 to find the prometheus service we want:
    - `kubectl get svc | grep 9090`
    - Note: It is the `prometheus-kube-prometheus-prometheus` one with the ClusterIP, not the `prometheus-operated` service

2. Redirect the output of `kubectl get svc prometheus-kube-prometheus-prometheus -o yaml > prometheus-service-aks.yaml` to get the manifest.
    - Update the type under spec to be `LoadBalancer` instead of `ClusterIP`
        - Refer to the `prometheus-service-aks.yaml` file as an example.
    - Note that this will prompt Azure to create a new Load Balancer called `kubernetes` with a Frontend IP Configuration rule for prometheus.
        - The LoadBalancer will also have a new `ExternalIP` associated with it.
        - The rules will make sure traffic for that Load Balancer IP get routed to 9090 and 8080.
        - Since it is just a LoadBalancer it will not have SSL. Ingress can be used to get SSL.

3. 