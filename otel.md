# OpenTelemetry for Observability: The Complete Course

Notes on the [course][1] from Udemy.

## General

- Observability is understanding systems behavior through external signals
- Its core pillars are metrics, logs, traces (containing spans)
- Service Level Indicators (SLI) are a carefully selected metrics that directly measure service behavior from the **user's** perspective
- Service Level Objectives (SLO) are specific target values for SLIs. They are promises we make about service quality **internally**
- Service Level Agreements (SLA) are contractual agreements with customers. SLAs should always be *lower* than SLIs
- Error budgets tell us how much we are allowed to fail. They are calculated as 100% - SLO and usually expressed over a 30 day period, e.g. for 99% uptime: 100% - 99% = 1%, which transfers to ~7h downtime per month (30 days * 24 hours * 1%)
- The [four golden figures][2] are: latency, traffic, errors and saturation

## OpenTelemetry (OTel)

### General

- OTel is a standardized, vendor-neutral and well-supported observability framework
- It sits between your app and your preferred observability backend
- The OTel SDK implements the interfaces defined the by the OTel API
- In your app, *initialize the SDK once* and then *use the resources from the API*
- Within the SDK exporters, processors, samplers and resource attributes can be configured

### Instrumentation

- OTel provides packages to enable *automatic* instrumentation for many popular packages
- Use *manual* instrumentation to create telemetry for business logic

### Collector

- Think of the OTel collector as a data pipeline for your observability data
- The OTel collector can be deployed as as *agent* (running an instance alongside every host) or as a gateway (centralized service)
- Use the OTel collectors to:
    - decouple the your app from the observability backend
    - send data to multiple observability backend simultaneously
    - configure sampling or filtering centrally
    - enrich data

### Conventions

- [Conventions][5] provide standardized naming rules for spans, metrics and attributes across programming languages and frameworks
- Within your code use the constants for semantic conventions provided by the OTel package
- When manually instrumenting, follow the naming conventions and always namespace custom attributes with your application name, i.e. *myapp.user.tier*

## Tooling

### Prometheus

- Prometheus is a time-series database to store metrics
- Per default it pulls metrics from services
- It provides its own query language (PromQL) to interact with metric data

### Loki

- Loki is a log aggregation system
- It also provides its own query language (LogQL)

### Tempo

- Tempo is a distributed tracing backend
- It also provides its own query language (TraceQL)

### Grafana

- tbd

### OTel Collector

- tbd

### Logging

- Use [structlog][3] for structured logging in Python


[1]: https://www.udemy.com/course/opentelemetry-observability
[2]: https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals
[2]: https://www.structlog.org/en/stable/index.html
[4]: https://sre.google/sre-book/introduction/
[5]: https://opentelemetry.io/docs/concepts/semantic-conventions/
