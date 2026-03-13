# Server / IT / Software / Computer Node Suggestions

This file gathers possible node additions for the **Server, IT, Software, and Computer** range.
The suggestions are aligned to common current computer-science structures seen in ETH Zürich, TUM, and ACM CS2023 guidance.

Format:
- `node-id` — Label → connect to `parent-id`

---

## 1. Broad computer foundations

- `programming-fundamentals` — Programming Fundamentals → connect to `software`
- `software-engineering` — Software Engineering → connect to `software`
- `programming-languages` — Programming Languages → connect to `software`
- `computer-architecture` — Computer Architecture → connect to `it`
- `discrete-mathematics-cs` — Discrete Mathematics for CS → connect to `algorithms`
- `data-structures` — Data Structures → connect to `algorithms`
- `algorithm-design` — Algorithm Design → connect to `algorithms`
- `complexity-theory` — Complexity Theory → connect to `algorithms`

## 2. Software development and application engineering

- `requirements-engineering` — Requirements Engineering → connect to `software-engineering`
- `software-architecture` — Software Architecture → connect to `software-engineering`
- `design-patterns` — Design Patterns → connect to `software-engineering`
- `testing-validation` — Testing & Validation → connect to `software-engineering`
- `devops` — DevOps → connect to `software`
- `ci-cd` — CI/CD Pipelines → connect to `devops`
- `web-development` — Web Development → connect to `software`
- `api-design` — API Design → connect to `web-development`
- `backend-engineering` — Backend Engineering → connect to `software`
- `frontend-engineering` — Frontend Engineering → connect to `software`
- `mobile-development` — Mobile Development → connect to `software`

## 3. Operating systems, systems software, and server internals

- `system-software` — System Software → connect to `os`
- `linux-systems` — Linux Systems → connect to `os`
- `processes-threads` — Processes & Threads → connect to `os`
- `memory-management` — Memory Management → connect to `os`
- `file-systems` — File Systems → connect to `os`
- `virtualization` — Virtualization → connect to `os`
- `virtual-machines` — Virtual Machines → connect to `virtualization`
- `distributed-systems` — Distributed Systems → connect to `os`
- `concurrency` — Concurrency → connect to `os`
- `observability` — Observability → connect to `cloud`
- `site-reliability-engineering` — Site Reliability Engineering → connect to `cloud`

## 4. Networking, internet, and communication

- `network-architecture` — Network Architecture → connect to `networking`
- `internet-protocols` — Internet Protocols → connect to `networking`
- `routing-switching` — Routing & Switching → connect to `networking`
- `network-services` — Network Services → connect to `networking`
- `dns` — DNS → connect to `network-services`
- `dhcp` — DHCP → connect to `network-services`
- `load-balancing` — Load Balancing → connect to `networking`
- `network-virtualization` — Network Virtualization → connect to `networking`
- `network-monitoring` — Network Monitoring → connect to `networking`
- `wireless-networks` — Wireless Networks → connect to `networking`

## 5. Cloud, infrastructure, and platform engineering

- `infrastructure-as-code` — Infrastructure as Code → connect to `cloud`
- `platform-engineering` — Platform Engineering → connect to `cloud`
- `cloud-architecture` — Cloud Architecture → connect to `cloud`
- `kubernetes` — Kubernetes → connect to `containers`
- `container-networking` — Container Networking → connect to `containers`
- `service-mesh` — Service Mesh → connect to `cloud`
- `serverless` — Serverless Computing → connect to `cloud`
- `multi-cloud` — Multi-Cloud Systems → connect to `cloud`
- `edge-computing` — Edge Computing → connect to `cloud`
- `data-center-systems` — Data Center Systems → connect to `cloud`

## 6. Databases and data management

- `databases` — Databases → connect to `data-eng`
- `relational-databases` — Relational Databases → connect to `databases`
- `nosql-databases` — NoSQL Databases → connect to `databases`
- `database-internals` — Database Internals → connect to `databases`
- `transaction-processing` — Transaction Processing → connect to `databases`
- `query-processing` — Query Processing → connect to `databases`
- `data-modeling` — Data Modeling → connect to `data-eng`
- `data-warehousing` — Data Warehousing → connect to `data-eng`
- `stream-processing` — Stream Processing → connect to `data-eng`
- `big-data-platforms` — Big Data Platforms → connect to `data-eng`

## 7. Security and trustworthy systems

- `application-security` — Application Security → connect to `security`
- `network-security` — Network Security → connect to `security`
- `system-security` — System Security → connect to `security`
- `identity-access-management` — Identity & Access Management → connect to `security`
- `cryptography` — Cryptography → connect to `security`
- `secure-software` — Secure Software Engineering → connect to `security`
- `cloud-security` — Cloud Security → connect to `security`
- `zero-trust` — Zero Trust Architecture → connect to `security`
- `security-monitoring` — Security Monitoring → connect to `security`
- `reliable-systems` — Reliable Systems → connect to `security`

## 8. Parallel, performance, and advanced computing systems

- `parallel-computing` — Parallel Computing → connect to `hpc`
- `distributed-computing` — Distributed Computing → connect to `hpc`
- `performance-engineering` — Performance Engineering → connect to `hpc`
- `performance-analysis` — Performance Analysis → connect to `hpc`
- `cluster-computing` — Cluster Computing → connect to `hpc`
- `accelerated-computing` — Accelerated Computing → connect to `gpu-prog`
- `heterogeneous-computing` — Heterogeneous Computing → connect to `hpc`
- `resource-scheduling` — Resource Scheduling → connect to `hpc`

## 9. Computer interaction and visual computing

- `human-computer-interaction` — Human-Computer Interaction → connect to `software`
- `computer-graphics` — Computer Graphics → connect to `software`
- `visual-computing` — Visual Computing → connect to `software`
- `interactive-systems` — Interactive Systems → connect to `software`
- `information-visualization` — Information Visualization → connect to `visual-computing`

## 10. Suggested high-value first additions

These are the most useful additions if you want to expand the graph without adding too many nodes at once.

- `computer-architecture` → `it`
- `programming-languages` → `software`
- `software-engineering` → `software`
- `software-architecture` → `software-engineering`
- `testing-validation` → `software-engineering`
- `distributed-systems` → `os`
- `virtualization` → `os`
- `linux-systems` → `os`
- `network-architecture` → `networking`
- `dns` → `network-services`
- `cloud-architecture` → `cloud`
- `infrastructure-as-code` → `cloud`
- `platform-engineering` → `cloud`
- `databases` → `data-eng`
- `relational-databases` → `databases`
- `database-internals` → `databases`
- `application-security` → `security`
- `system-security` → `security`
- `parallel-computing` → `hpc`
- `performance-engineering` → `hpc`

## 11. Notes for graph integration

- Many of these are best added as **level 3** nodes under your current level 2 IT branches.
- Some are good **level 4** nodes if you want more detail, such as `dns`, `dhcp`, `ci-cd`, `query-processing`, and `container-networking`.
- If you want a cleaner systems subtree, consider introducing a new level 2 node such as `computer-systems` under `it`, then attaching `os`, `distributed-systems`, `virtualization`, and `computer-architecture` under it.
