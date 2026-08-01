---
name: Generate an ecommerce analytics report with Polar
description: Use the Polar Headless MCP to discover a workspace's metrics and dimensions, then generate a custom ecommerce analytics report with a shareable deep link.
api: mcp/polar-analytics-mcp.yml
operations: [get_context, list_dimensions, generate_report, generate_report_link, rate_report]
---

# Generate an ecommerce analytics report with Polar

Operating instructions for an agent using the hosted **Polar Headless MCP**
(`https://api.polaranalytics.com/mcp`). All tool calls use
`Authorization: Bearer <API_KEY>` (generate the key on the Polar MCP page at
https://app.polaranalytics.com/mcp). See `conventions/polar-analytics-conventions.yml`
and `authentication/polar-analytics-authentication.yml`.

## Steps

1. **Prime the context.** Call `get_context` first to retrieve the conversation ID,
   workspace details, and the metrics and dimensions available in this workspace.
   Never assume a metric name — only use what `get_context` returns.
2. **Resolve dimensions.** Call `list_dimensions` for the target metric(s) to confirm
   which filters/breakdowns are compatible before building the report.
3. **Generate the report.** Call `generate_report` with the chosen metrics, dimensions,
   date range, and filters. The response contains a rows list, a totals row, and a deep link.
4. **Share it.** Call `generate_report_link` to produce a shareable Polar deep link,
   choosing a visualization mode (`table`, `bar`, or `pie`).
5. **Capture feedback (optional).** Call `rate_report` (1-10) to record report quality feedback.

## Rules

- Authenticate every request with the Bearer API key; for hosted connectors, OAuth 2.0
  (authorization code / client credentials, PKCE S256, dynamic client registration) is available.
- No idempotency-key contract is documented — do not retry `generate_report` blindly on timeout;
  re-issue only after confirming no report was produced.
- Prefer dashboards (`list_dashboards` / `get_dashboard_details`) and saved views
  (`get_view_details`) when the user references existing Polar assets rather than an ad-hoc report.
