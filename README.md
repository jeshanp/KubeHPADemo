# Kubernetes Horizontal Pod Autoscaler (HPA) Demo

A basic demonstration of Kubernetes Horizontal Pod Autoscaling using Minikube.

## Prerequisites

* [Minikube](https://minikube.sigs.k8s.io/docs/start/)
* [kubectl](https://kubernetes.io/docs/tasks/tools/)

## Setup & Deployment

1. **Start the Minikube cluster** with 2 nodes and Calico CNI:
   ```bash
   minikube start -p hpa-demo --cni=calico --nodes=2
   ```

2. **Enable the Metrics Server** addon (required for HPA to read resource utilization):
   ```bash
   minikube addons enable metrics-server -p hpa-demo
   ```

3. **Deploy the application, service, and HPA**:
   ```bash
   kubectl apply -f deployment.yaml
   kubectl apply -f service.yaml
   kubectl apply -f hpa.yaml
   ```

## Testing Autoscaling

1. **Get the name of a running pod**:
   ```bash
   kubectl get pods
   ```

2. **Generate CPU load** on the pod for 120 seconds (replace `<pod-name>` with your actual pod name):
   ```bash
   kubectl exec -it <pod-name> -- timeout 120 sh -c "while true; do :; done"
   ```

3. **Watch the HPA** scale the pods up and down (run in a separate terminal):
   ```bash
   kubectl get hpa -w
   ```

## Cleanup

To delete the Minikube cluster when you are finished:
```bash
minikube delete -p hpa-demo
```
