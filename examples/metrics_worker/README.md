# SQS Worker Metrics with Prometheus and Grafana

This example demonstrates how to monitor SQS worker metrics using Prometheus.

## Architecture

- **Worker**: SQS worker that processes messages and exposes metrics on port 8000
- **LocalStack**: Local AWS SQS service for testing
- **Prometheus**: Metrics collection and storage (accessible on port 9090)

## Metrics Exposed

The SQS worker exposes the following metrics:

1. **`sqs_worker_exceptions_total`** (Counter): Total number of exceptions encountered
1. **`sqs_worker_messages_total`** (Counter): Total number of processed messages
2. **`sqs_worker_work_latency_seconds`** (Histogram): Latency of message processing

## Usage

### 1. Start the Stack

```bash
docker-compose up -d
```

This will start:
- LocalStack (SQS service)
- SQS Worker with metrics enabled
- Prometheus

### 2. View Metrics

#### Prometheus (Raw Metrics)
- URL: http://localhost:9090
- You can query metrics directly using PromQL

Example queries:
- `sqs_worker_exceptions_total` - Total exceptions
- `sqs_worker_messages_total` - Total exceptions
- `rate(sqs_worker_work_latency_seconds_count[5m])` - Message processing rate
- `histogram_quantile(0.95, rate(sqs_worker_work_latency_seconds_bucket[5m]))` - 95th percentile latency

### 3. Clean Up

```bash
docker-compose down -v
```

## Configuration Files

- `prometheus.yml`: Prometheus configuration to scrape worker metrics

## Customization

To add more metrics, modify the worker code to expose additional Prometheus metrics using the `metrics_instrumentator` object.
