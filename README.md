# Awesome-Test-Alerting-Notification-Dispatch

# Awesome Test Alerting & Notification Dispatch

> A curated list of top Test Alerting and Notification Dispatch platforms — both SaaS/Hosted commercial solutions and open-source alternatives for routing test failures, build results, and quality gate alerts to the right teams and channels.
> 🌟 Star this repo if you find it useful! 🌟

---

## 📋 Table of Contents

- [SaaS / Hosted Platforms](#-saas--hosted-platforms)
  - [BrowserStack Configurable Webhooks](#browserstack-configurable-webhooks)
  - [PagerDuty](#pagerduty)
  - [Opsgenie](#opsgenie)
  - [Courier](#courier)
- [Open-Source Software](#-open-source-software)
  - [GoAlert](#goalert)
  - [OpsKnight](#opsknight)
  - [Herald](#herald)
  - [ChronoCourier](#chronocourier)
  - [versus-incident](#versus-incident)
  - [NoticeMe](#noticeme)
- [Comparison at a Glance](#-comparison-at-a-glance)
- [Contributing](#-contributing)

---

## 🟦 SaaS / Hosted Platforms

Commercial, cloud-hosted Test Alerting and Notification Dispatch platforms. These are not open-source but are included as reference points for feature comparison.

### BrowserStack Configurable Webhooks
**Type:** Test-aware webhook workflow builder
BrowserStack Test Observability provides **Configurable Webhooks** that act as a workflow builder for test events, not just simple notifications. You can trigger webhooks on Build Finish, Test Failure, Quality Gates, and Alerts, with conditional triggers that fire only when criteria are met . Payloads can use pre-configured templates or custom JSON, with dozens of built-in variables (status, duration, failure logs, flakiness metrics) and response variables for chaining webhooks together (e.g., extracting an incident ID from PagerDuty's response to resolve it later) . This is purpose-built for test automation workflows: creating PagerDuty incidents on critical failures, blocking deployments via Quality Gate, or sending instant Teams alerts for P0 failures . A Coinbase engineer cited the feature for automatically creating and resolving PagerDuty incidents based on automated test results .
**Best for:** Teams using BrowserStack wanting test-aware alerting with intelligent sequencing, dynamic payloads, and direct integration with PagerDuty, Jira, and Teams.

### PagerDuty
**Type:** Incident management and on-call alerting
PagerDuty is the industry standard for incident response and on-call management. For test alerting, it is typically the destination for critical failures that require immediate human attention. It integrates with CI/CD platforms, monitoring tools, and custom webhooks to create incidents, escalate to on-call engineers, and track resolution. Pricing is per-user and scales with feature tiers.
**Best for:** Organizations needing mature on-call scheduling, escalation policies, and incident lifecycle management for critical test and production alerts.

### Opsgenie
**Type:** Incident alerting and on-call management (Atlassian)
Opsgenie provides alerting, on-call scheduling, and incident response similar to PagerDuty, with deep integration into the Atlassian ecosystem (Jira, Bitbucket, Statuspage). It is often used as the alerting destination for CI/CD failures and test quality gates. Pricing is per-user.
**Best for:** Atlassian-centric organizations wanting on-call alerting tightly integrated with Jira and other Atlassian tools.

### Courier
**Type:** Notification infrastructure API
Courier provides a unified API for sending notifications across email, SMS, push, and chat (Slack, Teams). It focuses on notification orchestration and templating rather than incident management. For test alerting, it can serve as the dispatch layer for custom notification workflows that need multi-channel delivery.
**Best for:** Teams wanting a notification API abstraction to dispatch test results across multiple channels without building each integration individually.

---

## 🟩 Open-Source Software

Self-hosted, community-driven Test Alerting and Notification Dispatch tools. The open-source ecosystem splits between **on-call incident management platforms** and **headless notification infrastructure libraries**.

### GoAlert
**Stack:** Go | **License:** Open Source
GoAlert is an open-source on-call scheduling, automated escalation, and notification platform built by Target, explicitly positioned as a **self-hosted alternative to PagerDuty and Opsgenie with no per-user pricing** [citation:6]. It connects to notification channels including Twilio for SMS/voice, Slack via a generated app manifest, and SMTP for email. Monitoring tools like Alertmanager and Grafana can point at GoAlert integration keys to create alerts. Deployed via Docker or Zeabur. **Best for:** Teams wanting a proven, self-hosted PagerDuty alternative for test failure alerting with on-call schedules and escalations, without per-user fees.

### OpsKnight
**Stack:** Next.js / PostgreSQL / Prisma | **License:** Open Source
OpsKnight is a modern, self-hosted incident management and alerting platform with **22+ integrations** for observability and engineering stacks . It runs as a single Next.js application with an internal DB-backed scheduler and Postgres-backed job queue, deploying via Docker Compose, Helm, or Kustomize. Integration credentials are encrypted at rest with an `ENCRYPTION_KEY`. For test alerting, it can receive alerts from CI pipelines and route them to on-call teams. **Best for:** Teams wanting a modern, self-hosted alerting platform with broad integrations and Kubernetes-ready deployment, as a lighter PagerDuty alternative.

### Herald
**Stack:** TypeScript | **License:** Open Source
Herald is a **headless, open-source notification infrastructure library** for TypeScript, positioned as an alternative to SaaS platforms like Novu and Knock [citation:7]. It provides subscribers, workflows, preferences, channels (in-app and email, with SMS/push coming), and an in-app inbox. Critically, it is **bring-your-own-database** (Prisma, Drizzle) and **bring-your-own-workflow-engine** (Inngest, Temporal), with a single config file pattern similar to better-auth. It generates REST endpoints automatically and is framework-agnostic. **Trade-off:** It is a library you embed in your application, not a standalone alerting platform. You build the alerting logic and UI. **Best for:** Developers wanting to embed a notification system into their test tooling or internal platform without SaaS dependencies, with full control over data and workflows.

### ChronoCourier
**Stack:** Python / Flask | **License:** Open Source
ChronoCourier is a priority-based notification service that receives alerts over HTTP, validates and queues them, and delivers to **Telegram and email** [citation:4]. Key features for test alerting include priority queues (alerts get highest initial priority), separate retry queues with configurable delay and retry limits, and independent delivery attempts per destination. It supports API-key authentication, JSON-aware formatting, Telegram MarkdownV2, and email attachments. Deploys via Docker Compose with a development SMTP environment (MailHog + stunnel). **Best for:** Teams wanting a focused, self-hosted notification dispatcher for routing test alerts to Telegram and email with priority and retry handling.

### versus-incident
**Stack:** Go | **License:** Open Source
versus-incident is a self-hosted incident management and alerting platform with **universal alert template support** for Alertmanager (Prometheus), Grafana, Sentry, CloudWatch SNS, and FluentBit [citation:9]. It accepts incidents via `POST /api/incidents` with a JSON payload, making it easy to route test failures from CI pipelines through the same channels and on-call workflows. The agent can learn log patterns and alert on anomalies (training/shadow/detect modes). **Best for:** Teams already using Prometheus/Grafana wanting a self-hosted alerting layer that accepts test failure webhooks alongside infrastructure alerts.

### NoticeMe
**Stack:** Python / SQLite | **License:** Open Source
NoticeMe is a lightweight notification management system with **MCP (Model Context Protocol) server** support for AI agent integration [citation:13]. It supports notification sources (Webhook HTTP API or MQTT subscription) and channels (MQTT publish or API request forwarding), with source-to-channel mapping, real-time WebSocket notifications, and history tracking. Includes a CLI (`nme`) and a dark-theme web UI. **Best for:** Teams wanting a simple, self-hosted notification router with MCP integration for AI-assisted workflows, particularly for IoT or lightweight test alerting.

---

## 📊 Comparison at a Glance

| Software | License | Stack | On-Call/Escalation | Channels | Self-Hosted | Best For |
|----------|---------|-------|-------------------|----------|-------------|----------|
| **BrowserStack Webhooks** | Proprietary | Cloud | (Via PagerDuty) | Any webhook | ❌ | Test-aware alerting + sequencing |
| **PagerDuty** | Proprietary | Cloud | ✅ | SMS, voice, email, chat | ❌ | Industry-standard incident response |
| **Opsgenie** | Proprietary | Cloud | ✅ | SMS, voice, email, chat | ❌ | Atlassian ecosystem alerting |
| **Courier** | Proprietary | Cloud | ❌ | Email, SMS, push, chat | ❌ | Notification API abstraction |
| **GoAlert** | Open Source | Go | ✅ | SMS, voice, email, Slack | ✅ | Self-hosted PagerDuty alternative |
| **OpsKnight** | Open Source | Next.js/Postgres | ✅ | 22+ integrations | ✅ | Modern self-hosted alerting |
| **Herald** | Open Source | TypeScript | ❌ (Library) | In-app, email | ✅ (Embed) | Headless notification infra |
| **ChronoCourier** | Open Source | Python/Flask | ❌ (Priority queues) | Telegram, email | ✅ | Priority-based alert dispatch |
| **versus-incident** | Open Source | Go | ✅ | Slack, Telegram, webhooks | ✅ | Universal alert template support |
| **NoticeMe** | Open Source | Python/SQLite | ❌ | MCP, MQTT, API | ✅ | Lightweight MCP notification router |

---

## 🤝 Contributing

Contributions are welcome! If you know of an open-source Test Alerting or Notification Dispatch tool that should be listed here, please open a pull request with:

- Project name and URL
- Brief description (1–2 sentences)
- Stack and license
- Why it stands out

Please ensure the project is actively maintained (releases within the last 12 months) and genuinely open-source. Note that the open-source ecosystem splits between **on-call incident management** (GoAlert, OpsKnight, versus-incident) and **notification infrastructure libraries** (Herald, ChronoCourier, NoticeMe). GoAlert is the most mature self-hosted PagerDuty alternative, while Herald is notable for developers wanting to embed notification logic directly into their own test tooling without a SaaS dependency.

---

## 📄 License

This list is released under CC0 1.0 Universal (Public Domain). The software projects listed retain their respective licenses.

---

⭐ **If this list helped you discover a useful tool, please star the repo!** ⭐
