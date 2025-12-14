# Executive Daily Briefing - Kestra Workflow

## Overview

**Project Name:** Executive Daily Briefing  

This project is an **AI-driven executive health monitoring system** that consolidates data from multiple business and operational systems — CRM (HubSpot/Salesforce), Engineering (GitHub), Marketing, and Ops (Datadog). The workflow summarizes critical metrics, flags potential issues, and automatically triggers notifications based on severity.

The goal is to **turn raw data into actionable insights**, allowing executives to quickly understand the overall health of their company and prioritize actions without manually checking multiple dashboards.

---

## Features

- Fetches and aggregates metrics from multiple sources in parallel:
  - CRM (deals, pipeline trends)
  - GitHub (issues, repository health)
  - Marketing (ad spend, leads)
  - Datadog (system performance, error rates)
- AI analysis logic:
  - Summarizes overall health
  - Detects red flags based on predefined thresholds
- Dynamic routing based on severity:
  - HIGH: sends Slack alert
  - MEDIUM: logs and updates Notion
  - LOW/DEFAULT: logs daily report
- Scheduled execution every day at 8 AM via Cron trigger

---

## Architecture & Tech Stack

**Architecture Diagram:**

      ┌─────────────┐
      │ CRM System  │
      └─────┬───────┘
            │
      ┌─────▼───────┐
      │ GitHub API  │
      └─────┬───────┘
            │
      ┌─────▼───────┐
      │ Marketing   │
      └─────┬───────┘
            │
      ┌─────▼───────┐
      │ Datadog     │
      └─────┬───────┘
    ┌───────▼───────────┐
    │ Kestra Workflow   │
    │ - Parallel Fetch  │
    │ - AI Analysis     │
    │ - Routing Logic   │
    └─────────┬─────────┘
              │
    ┌─────────▼─────────────┐
    │ Notifications / Logs  │
    │ Slack / Notion / Logs │
    └───────────────────────┘


### Workflow

![flow-graph-1765729817511](https://github.com/user-attachments/assets/5d19adb9-d058-4be3-ac1f-f7340be250fc)

### Result Example
<img width="598" height="142" alt="Screenshot 2025-12-14 at 10 12 05 PM" src="https://github.com/user-attachments/assets/dddaba03-2092-407c-a9e5-f95f28036106" />


**Tech Stack:**

- [Kestra](https://kestra.io/) - Workflow orchestration
- Python - Data transformation & AI analysis
- Gemini API - For agent to analyse automatically
- Slack - Notifications
- Cron - Daily scheduling

---

## How It Works

### 1. Fetch Data in Parallel

The workflow starts by fetching data from multiple sources **simultaneously** using:

- Python scripts (CRM, Marketing, Datadog)
- HTTP request task (GitHub API)

### 2. AI Analysis Logic

A Python script consolidates all metrics and evaluates **severity**:

```text
- LOW: Systems Stable
- MEDIUM: Attention required
- HIGH: Immediate action required
