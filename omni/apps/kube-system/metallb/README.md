# MetalLB

This directory contains the MetalLB installation using Helm instead of raw YAML manifests.

## What is MetalLB?

MetalLB is a load balancer implementation for bare metal Kubernetes clusters, using standard routing protocols. It allows you to create LoadBalancer services in clusters that don't run on a cloud provider, and thus cannot simply hook into cloud load balancers.

## Installation

MetalLB is installed via ArgoCD using the Helm chart from the official MetalLB repository.

## Configuration

### IP Address Pool

Before using MetalLB, you need to configure an IP address pool. Edit the `ipaddresspool.yaml` file and replace the example IP range with your actual network configuration:

```yaml
spec:
  addresses:
    - 192.168.1.240-192.168.1.250  # Replace with your IP range
```

### Layer 2 Mode (Default)

The default configuration uses Layer 2 mode, which is the simplest to set up. It works by having one node announce that it has the IP address for the service.

### BGP Mode

For BGP mode, you would need to:
1. Enable BGP in the `values.yaml` file
2. Configure BGP peers and communities
3. Update the IPAddressPool to use BGP protocol

## Usage

After installation, you can create LoadBalancer services and MetalLB will automatically assign IP addresses from the configured pool.

Example:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  type: LoadBalancer
  ports:
    - port: 80
      targetPort: 8080
  selector:
    app: my-app
```

## Monitoring

Prometheus monitoring is enabled by default with ServiceMonitor for metrics collection.

## References

- [MetalLB Documentation](https://metallb.universe.tf/)
- [MetalLB Helm Chart](https://github.com/metallb/metallb/tree/main/charts/metallb)
