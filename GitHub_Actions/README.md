# GitHub Actions Job Logs Dashboard

Shows job errors and job log spans from GitHub Actions job logs. The data comes from the workflow sender job in the OpenObserve GitHub Actions guide, which posts `{run_id, job_id, _timestamp, log}` rows to the `githubactions` logs stream. Each row is one `##[group]` block of a job's log. The guide's action sends only these log rows. If an org also sends GitHub webhook events to the same stream, the dashboard ignores those rows: every panel reads only rows with a `job_id` or a `log` line.

## Dashboard Features

- **Health**: jobs received, jobs with `##[error]` lines, job error rate, error lines over time, and the top error messages.
- **Speed**: median and p95 log span per job, the jobs with the longest log span (with repository and workflow file read from the `Job defined at:` log line), and the longest runner waits.

## Limits

- The log span is not the job duration: it runs from the job's first log group to the start of its last log group, so time before the first and after the start of the last log group is missing.
- Failed runs reach OpenObserve only when the sender job runs with `if: always()`. Without it the error rate stays near 0%.
- The logs carry no status, conclusion, workflow name or branch columns, so the dashboard cannot show pass or fail per workflow.

## How to Import

1. Dashboards -> Import -> upload `GitHub_Actions_Job_Logs.dashboard.json`.
2. The `githubactions` logs stream must exist and have data.
