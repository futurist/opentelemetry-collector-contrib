# Health Check

| Status                   |                       |
| ------------------------ |-----------------------|
| Stability                | [beta]                |
| Distributions            | [contrib]             |

Health Check extension enables an HTTP url that can be probed to check the
status of the OpenTelemetry Collector. This extension can be used as a
liveness and/or readiness probe on Kubernetes.

There is an optional configuration `check_collector_pipeline` which allows
users to enable health check for the collector pipeline. This feature can
monitor the number of times that components failed send data to the destinations.
It only supports monitoring exporter failures and will support receivers and
processors in the future.

The following settings are required:

- `endpoint` (default = 0.0.0.0:13133): Address to publish the health check status. For full list of `HTTPServerSettings` refer [here](https://github.com/open-telemetry/opentelemetry-collector/tree/main/config/confighttp).
- `path` (default = "/"): Specifies the path to be configured for the health check server.
- `response_body` (default = ""): Specifies a static body that overrides the default response returned by the health check service. 
- `check_collector_pipeline:` (optional): Settings of collector pipeline health check
    - `enabled` (default = false): Whether enable collector pipeline check or not
    - `interval` (default = "5m"): Time interval to check the number of failures
    - `exporter_failure_threshold` (default = 5): The failure number threshold to mark
      containers as healthy.

Example:

```yaml
extensions:
  health_check:
  health_check/1:
    endpoint: "localhost:13"
    tls:
      ca_file: "/path/to/ca.crt"
      cert_file: "/path/to/cert.crt"
      key_file: "/path/to/key.key"
    path: "/health/status"
    check_collector_pipeline:
      enabled: true
      interval: "5m"
      exporter_failure_threshold: 5
```

The full list of settings exposed for this exporter is documented [here](./config.go)
with detailed sample configurations [here](./testdata/config.yaml).

[beta]: https://github.com/open-telemetry/opentelemetry-collector#beta
[contrib]: https://github.com/open-telemetry/opentelemetry-collector-releases/tree/main/distributions/otelcol-contrib
