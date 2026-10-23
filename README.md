# System Design & Cloud Study Guide

A progressive, visual study guide for designing **secure, resilient, high-performing systems** and for understanding **core cloud services**.

## How this guide teaches

Every anchor concept is walked through the same six zoom levels, from wide to narrow:

| Level | Question it answers | You get |
|---|---|---|
| 1. Big picture | What is it and why does it exist? | Plain-language purpose, the problem it solves |
| 2. Structure | What are the parts and how do they connect? | Component diagram |
| 3. Behaviour | What happens at runtime? | Sequence / state / flow diagram |
| 4. Implementation | How do I actually build it? | Code, config, or a concrete example |
| 5. Trade-offs | What do I give up? When do I not use it? | Comparison table |
| 6. Finer details | Where does it bite in production? | Gotchas, numbers, edge cases |

Smaller concepts appear in a **Concept map** table at the end of each chapter (what / how / trade-off in one line each), so every item from the original checklists is covered.

## Reading order

| # | File | Focus |
|---|---|---|
| 01 | [foundations-big-picture](01-foundations-big-picture.md) | Reference architecture, quality attributes, estimation, trade-off thinking |
| 02 | [security](02-security.md) | AuthN/Z, zero trust, encryption, threat modeling, OWASP |
| 03 | [resilience](03-resilience.md) | Retries, circuit breakers, failover, CAP, backup and DR |
| 04 | [performance-scalability](04-performance-scalability.md) | Caching, databases, async, load balancing, latency math |
| 05 | [distributed-systems-and-apis](05-distributed-systems-and-apis.md) | Observability, API design, consensus, sagas, safe deployment |
| 06 | [cloud-compute-storage](06-cloud-compute-storage.md) | VMs, containers, serverless, object/block/file storage |
| 07 | [cloud-data-messaging](07-cloud-data-messaging.md) | Managed databases, NoSQL, caches, queues, topics, streams |
| 08 | [cloud-networking-security](08-cloud-networking-security.md) | VPC, load balancing, DNS, CDN, IAM, secrets, governance |
| 09 | [cloud-ops-architecture-cost](09-cloud-ops-architecture-cost.md) | IaC, CI/CD, regions/AZs, DR tiers, Well-Architected, FinOps |
| 10 | [worked-example-url-shortener](10-worked-example-url-shortener.md) | Everything applied end to end, v1 to v3 |
| 11 | [cheatsheet-and-glossary](11-cheatsheet-and-glossary.md) | Decision tables, provider mapping, review questions, 4-week plan |

## Suggested path

1. Read **01** for the map of the territory.
2. Read **10** next to see the concepts working together (skim, don't memorize).
3. Go through **02 to 05** (principles), then **06 to 09** (cloud realization).
4. Use **11** for review and self-testing.

## Rendering

Diagrams are [Mermaid](https://mermaid.js.org/) blocks. They render in GitHub, GitLab, VS Code (Markdown Preview Mermaid Support extension), Obsidian, and most modern Markdown viewers. If your viewer shows raw code, paste a block into <https://mermaid.live>.

## Provider naming

Cloud services are given as **AWS / Azure / GCP**. Concepts are provider-neutral; names differ.
