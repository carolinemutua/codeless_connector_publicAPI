# sentinel

Personal learning work on Microsoft Sentinel. Everything here is built from public documentation and public data sources. Nothing in this repository contains employer, customer, or tenant-specific information.

## What is in this repository

| Folder | What it holds |
|---|---|
| [codeless-connector/](codeless-connector/) | A self-contained ARM template that stands up a Microsoft Sentinel codeless connector against the public FeodoTracker (abuse.ch) threat feed, plus a detection rule and a workbook. |
| [sentinel-study-guide/](sentinel-study-guide/) | A single-file HTML study guide: a searchable question-and-answer reference for Sentinel concepts, with mind maps and links to Microsoft Learn. |

Each folder has its own README with the detail.

## Scope

This is a study and portfolio repository, not production tooling. The connector template is meant to be deployed into a personal or lab Log Analytics workspace to learn how codeless connectors, data collection rules, and analytics rules fit together.