# Longhorn Monitoring Setup

This directory contains the monitoring configuration for Longhorn using Prometheus and Grafana.

## Components

### 1. ServiceMonitor
- **File**: `kube-prometheus-stack/longhorn-servicemonitor.yaml`
- **Purpose**: Configures Prometheus to scrape metrics from Longhorn Manager pods
- **Target**: Longhorn Manager pods in the `longhorn-system` namespace
- **Metrics Endpoint**: `/metrics` on port `manager`

### 2. Grafana Dashboard
- **File**: `kube-prometheus-stack/longhorn-dashboard.yaml`
- **Purpose**: Provides a pre-built dashboard for Longhorn metrics visualization
- **Features**:
  - Volume actual size monitoring
  - Volume status tracking
  - Real-time metrics display

### 3. Alert Rules
- **File**: `kube-prometheus-stack/longhorn-alerts.yaml`
- **Purpose**: Defines Prometheus alert rules for Longhorn health monitoring
- **Alerts**:
  - `LonghornVolumeDegraded`: Volume in degraded state for >5 minutes
  - `LonghornVolumeFaulted`: Volume in faulted state
  - `LonghornVolumeDetached`: Volume detached for >10 minutes
  - `LonghornNodeDown`: Longhorn node down for >5 minutes
  - `LonghornVolumeSpaceUsage`: Volume using >80% of allocated space

## Configuration

### Longhorn Application
The Longhorn application has been configured to enable metrics:
```yaml
metrics:
  serviceMonitor:
    enabled: true
```

### Prometheus Configuration
Prometheus is configured to:
- Scrape Longhorn metrics every 30 seconds
- Load custom alert rules
- Store metrics data with 150Gi storage

### Grafana Configuration
Grafana is configured to:
- Auto-discover dashboards from ConfigMaps
- Use anonymous access (no login required)
- Expose service via Omni Workload Proxying on port 50082

## Accessing Metrics

### Prometheus
- Access Prometheus UI through the monitoring stack
- Query Longhorn metrics using PromQL
- View alert rules and targets

### Grafana
- Access Grafana UI through Omni Workload Proxying
- Default dashboard: "Longhorn Dashboard"
- Datasource: `prometheus-longhorn`

## Key Metrics

Longhorn exposes various metrics including:
- `longhorn_volume_actual_size_bytes`: Actual size of volumes
- `longhorn_volume_status`: Volume health status
- `longhorn_node_status`: Node health status
- `longhorn_volume_size_bytes`: Allocated volume size

## Troubleshooting

1. **Metrics not appearing**: Check if ServiceMonitor is properly configured and Longhorn Manager pods are running
2. **Dashboard not loading**: Verify Grafana dashboard ConfigMap is properly labeled
3. **Alerts not firing**: Check Prometheus rule configuration and metric availability

## References

- [Longhorn Monitoring Documentation](https://longhorn.io/docs/1.9.0/monitoring/prometheus-and-grafana-setup/)
- [Longhorn Metrics Reference](https://longhorn.io/docs/1.9.0/monitoring/longhorn-metrics/)
