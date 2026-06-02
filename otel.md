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

## Tooling

### Prometheus

- Prometheus is a time-series database to store metrics
- Per default it pulls metrics from services
- It provides its own query language (PromQL) to interact with metric data

### Logging

- Use [structlog][3] for structured logging in Python


[1]: https://www.udemy.com/course/opentelemetry-observability
[2]: https://sre.google/sre-book/monitoring-distributed-systems/#xref_monitoring_golden-signals
[2]: https://www.structlog.org/en/stable/index.html
[4]: https://sre.google/sre-book/introduction/
