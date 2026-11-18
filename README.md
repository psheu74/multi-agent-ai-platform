# Multi-Agent AI Platform — Local LLMs + RAG on Self-Hosted GPU Infrastructure

A self-hosted, **multi-agent AI system** that coordinates specialized agents built on **local large language models** to research, decide, and act autonomously. A **frontier model generates training data and fine-tunes the local models**, **Retrieval-Augmented Generation (RAG)** supplies context from a vector store, and a rigorous, evidence-based evaluation loop validates every change before it ships. Designed, built, and operated end to end on my own GPU infrastructure.

> **Portfolio project** demonstrating applied-AI engineering — LLM orchestration, fine-tuning, RAG, MLOps, and the infrastructure beneath them. The decision domain happens to be algorithmic trading; **this repo focuses on the engineering, not trading signals.**

---

## Architecture

- **7 specialized agents** (research, orchestration, execution, supervisor, and others) — each a **FastAPI** service with its own least-privilege database login.
- **Local LLM layer:** **Ollama** serving models from **8B to 72B** (Llama, Qwen, Phi) across **4× NVIDIA Tesla V100 32 GB** GPUs.
- **Frontier-in-the-loop:** the **Claude API** generates and labels training data and refines the local models; frontier and local models run together in a **voting** process with a **supervisor override**.
- **RAG:** a **ChromaDB** vector store supplies relevant history and context to agents before each decision.
- **Data:** dual **SQL Server 2022** (operational + always-on replica) with nightly mirroring; a **13-year market-data lab** for backtesting.
- **Orchestration:** 110+ scheduled jobs, monitoring, alerting, and per-host version control.

## Infrastructure (designed, built, and operated solo)

- **HPE ProLiant DL380 Gen10** — 2× Xeon Gold 6230R (52 cores), 384 GB RAM, **1.5 TB Intel Optane persistent memory**, 8× enterprise SSD RAID 10, 2× Tesla V100.
- **Secondary host** — 2× Xeon E5-2699 v4, 2× Tesla V100 + RTX 5060 Ti.
- **VMware ESXi 8**, Ubuntu VMs, **UniFi 10G** networking, **Tailscale** remote access, **HPE iLO/Redfish** automated power lifecycle.

## Engineering highlights

- **Evidence over intuition** — every model/rule change is validated against **13 years of data** (train/test split, baseline-adjusted lift, live-signal replay) before deployment.
- **Governance & security** — per-service least-privilege DB logins, secrets in protected `.env` files, credential rotation, dashboard authentication, and shell-injection / path-traversal hardening.
- **Reliability** — zero-data-loss rebuild of **1.5 TB of Optane persistent memory** (checksum-verified), recovering **317 GiB** of leaked capacity; clean VM boot when storage is missing; daylight-saving-safe scheduling across 110+ jobs; leak and heartbeat monitoring.
- **Knowledge capture** — a 2,600-entry lessons-learned database informs future changes.

## Tech stack

`Python` · `FastAPI` · `Node.js` · `Ollama` · `Claude API` · `ChromaDB` · `SQL Server 2022 / T-SQL` · `VMware ESXi` · `Linux` · `GPU passthrough` · `UniFi` · `Tailscale` · `HPE iLO / Redfish`

## What this demonstrates

End-to-end ownership of a production AI system: LLM deployment and **fine-tuning**, **multi-agent orchestration**, **RAG**, **MLOps** (scheduling, monitoring, evaluation, governance), and the **GPU and virtualization infrastructure** underneath — the full stack, built and run by one engineer.

---

> **Before pushing:** secrets, API keys, and credentials are excluded; all configuration is templated (`.env.example`). Never commit real keys or account identifiers.
