# AI / LLM Apps Dashboard

Calls, cost, tokens, errors and latency of LLM applications that send OpenTelemetry traces to OpenObserve. When OpenObserve ingests a span from a GenAI, OpenInference, OpenLLMetry or Vercel AI SDK instrumentation, it writes the model, token, cost and operation into `gen_ai_*` columns. This dashboard reads only those columns from the traces stream `default`, so it works the same for each instrumentation library.

## Dashboard Features

- **Overview tab**: LLM calls, total cost (USD), total tokens, LLM error rate and p95 latency; cost per model, input and output tokens, calls and errors, and p50/p95/p99 latency over time.
- **Models, services, tools tab**: a model leaderboard (calls, error rate, p95, average tokens), cost per model (total and per call, in USD with 6 decimals), cost per service, tool calls, agent runs, and the latest failed spans that carry a gen_ai operation, with their trace id.

Every model panel, including the error rate and the calls and errors chart, counts the same model-call spans: spans with a model name whose operation is `chat`, `text_completion`, `generate_content`, `embeddings`, or empty. Agent and workflow spans repeat the usage of their child spans, so they are left out. These totals match AI Observability > Insights. A failed call that lost its model name is not counted.

## Variables

- **Service** and **Model**: multi-select, all by default. There is no provider filter: many instrumentations do not send a provider name, and the dashboard reads no provider column, so it installs on streams without one.

## How to Import

In OpenObserve, open **Dashboards**, click **Import**, and upload `AI LLM Apps.dashboard.json`. Send your LLM app's traces to the traces stream `default`. If your stream has another name, change `default` in each panel query.
