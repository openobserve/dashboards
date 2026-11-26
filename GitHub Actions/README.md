# GitHub Actions CI/CD Dashboard

An OpenObserve dashboard for GitHub Actions workflow health and CI/CD reliability. It is separate from the `Github/` dashboard, which focuses on general repository and organization metrics.

## Panels

- Workflow run volume over time
- Successful and failed runs over time
- Average and p95 workflow-run duration
- Failed jobs grouped by workflow, job, and step
- P95 runner queue/wait time by workflow
- Flakiest workflows ranked by failure rate (minimum five completed runs)

## Data expected by the dashboard

The exported dashboard queries two OpenObserve **log** streams. Create these streams by forwarding GitHub Actions webhook events and/or polling the GitHub API, then normalize/enrich the records to the field names below. The stream names and fields are assumptions used in the dashboard SQL; edit the queries if your schema differs.

### `github_actions_workflow_run`

One record per completed workflow run, preferably emitted when a `workflow_run` event has `action: completed` (or equivalent from polling).

| Field | Type | Purpose |
| --- | --- | --- |
| `_timestamp` | timestamp | Event/ingestion timestamp used for dashboard time filtering and histograms; set to the run completion time for completed-run analytics. |
| `run_id` | string or number | GitHub workflow run identifier. |
| `workflow_name` | string | Workflow name. |
| `conclusion` | string | Run result such as `success`, `failure`, `timed_out`, `cancelled`, or `action_required`. |
| `run_duration_seconds` | number | Difference between run start and completion time, in seconds. |

### `github_actions_workflow_job`

One record per completed job from `workflow_job` events or API polling.

| Field | Type | Purpose |
| --- | --- | --- |
| `_timestamp` | timestamp | Job event/completion timestamp. |
| `workflow_name` | string | Parent workflow name. |
| `job_name` | string | GitHub Actions job name. |
| `conclusion` | string | Job result; failed jobs use `failure`. |
| `queue_duration_seconds` | number | Difference between job `started_at` and `queued_at`, in seconds. |
| `step_name` | string, optional | Failed step name, if obtained from job logs or other enrichment. |

**Important:** The `workflow_job` webhook payload reports job-level status and timing, but does not provide per-step conclusions. To populate the step portion of the failure breakdown, enrich job records with a `step_name` from an appropriate Actions API/log source. Without that enrichment, the panel groups failures under `step unavailable`.

## Ingestion and setup

1. Configure a GitHub Actions webhook or a scheduled GitHub API poller to send run and job records to your OpenObserve HTTP ingestion endpoint. Use secure secret handling for webhook/API credentials; do not commit secrets to this repository.
2. Normalize each incoming record to the stream names and field names listed above. Calculate duration fields in seconds. For run summaries, emit one completed record per run so retries or multiple webhook actions do not inflate counts.
3. In OpenObserve, confirm the streams are log streams and inspect a few records to verify field names and types.
4. Import `GitHub_Actions.dashboard.json` from **Dashboards → Import**.
5. Choose the desired time range. If your stream/field names differ, update the panel SQL queries to match your data.

The repository contains a dashboard definition, not an ingestion service or GitHub webhook configuration. Data collection and credentials must be configured in your environment.

## Notes

- Workflow volume, success/failure, duration, and flaky-workflow panels query `github_actions_workflow_run`.
- Queue wait and failed-job panels query `github_actions_workflow_job`.
- Cancelled runs are shown separately from failed runs in the trend panel; the flaky-workflow calculation counts `failure`, `timed_out`, and `action_required` as failures.
- Screenshots can be added here after importing the dashboard with representative data.

Related: openobserve/dashboards issue [#55](https://github.com/openobserve/dashboards/issues/55).
