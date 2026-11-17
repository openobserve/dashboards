# AWS CloudWatch Monitoring Dashboard

Shows EC2, RDS, Application Load Balancer, Lambda and SQS health from a CloudWatch metric stream. The data comes from the OpenObserve AWS integration: a CloudWatch metric stream in **JSON output format**, delivered by Amazon Data Firehose to the `cloudwatch_metrics` logs stream.

## Dashboard Features

- **Overview**: minutes since the last datapoint arrived (a stopped metric stream shows here first) and datapoints by namespace.
- **EC2**: average CPU per instance, instances failing status checks, network in and out in bytes per minute (summed over the selected instances, averaged over the minutes of each chart interval).
- **RDS**: average CPU, lowest free storage, connections, read and write latency per DB instance.
- **ALB**: requests and 5xx per load balancer, target response time (average and max), target groups with unhealthy or no healthy hosts.
- **Lambda**: invocations, errors and throttles, and duration (average and max) per function.
- **SQS**: age of the oldest message and visible messages per queue.

## Data shape this dashboard reads

The queries target the columns OpenObserve stores today for JSON-format metric-stream records:

| Column | Meaning |
|---|---|
| `namespace`, `metric_name`, `unit` | CloudWatch namespace, metric and unit |
| `account_id`, `region`, `metric_stream_name` | Source account, region and stream |
| `dimensions_<name>` | One column per CloudWatch dimension, name in lower case: `dimensions_instanceid`, `dimensions_dbinstanceidentifier`, `dimensions_loadbalancer`, `dimensions_targetgroup`, `dimensions_functionname`, `dimensions_queuename` |
| `value_sum`, `value_count`, `value_min`, `value_max` | The 1-minute statistic set. There is no average column; averages are `sum(value_sum) / sum(value_count)` |
| `timestamp` | Datapoint time, epoch milliseconds |
| `_timestamp` | Time OpenObserve received the row |

How the queries read it:

- Each panel keeps the rows of one resource type by requiring its dimension column, for example `dimensions_instanceid IS NOT NULL`. Rows for other dimension sets (Auto Scaling group, image, DB cluster) have that column empty and are left out.
- CloudWatch also sends Application Load Balancer metrics per zone and per target group, and Lambda metrics per alias and version. These rows carry the same load balancer or function name. ALB and Lambda totals therefore take, for each resource, metric and datapoint minute, the largest value. That is the resource-level row, so the subsets are not added on top of it.
- Time charts group by `_timestamp` (arrival time). Charts are shifted by the metric stream delivery delay, usually 2 to 3 minutes.
- A dimension column exists only after that service has sent data. Panels for a service you do not stream fail with "unknown field"; the Starter Pack installer leaves them out.

The OpenTelemetry 1.0 output format is stored differently and is not covered by this dashboard.

## How to Import

1. Dashboards -> Import -> upload `AWS_CloudWatch_Overview.dashboard.json`.
2. The `cloudwatch_metrics` logs stream must exist and have data.
3. Use the `Account` and `Region` variables, and the per-service variables (`Instance`, `DB instance`, `Load balancer`, `Function`, `Queue`), to narrow the view. All values are selected by default.
