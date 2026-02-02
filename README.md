# SquishLabs Public Notes 🧪

**Welcome to the public brain dump of SquishLabs.**

Because my professional work involves high-compliance environments (US Defense & Tier-1 Banking), I cannot share my production code. However, I believe in **Architecture as Code**. 

This repository documents the reference architectures I research and build in my private lab, focusing on **Edge AI**, **Agentic Workflows**, and **Zero-Trust GenAI Gateways**.

---

## Architecture 1: "The Edge-Native Agent" (LangGraph Implementation)
**Concept:** Running a cyclic reasoning loop on constrained hardware (NVIDIA Jetson) without cloud dependency.
**Why LangGraph?** Standard chains are DAGs (Directed Acyclic Graphs). Real agents need *cycles* to reason, retry, and reflect.

```mermaid
%%{init: { 'themeVariables': { 'noteTextColor': '#000000' } }}%%
sequenceDiagram
    participant Dev as Developer
    participant Gate as API Gateway (Kong/Apigee)
    participant PII as PII Redaction Service
    participant Model as LLM Provider
    participant Smith as LangSmith (Observability)

    Dev->>Gate: POST /v1/chat/completions
    
    rect rgb(255, 220, 220)
    Note over Gate, PII: Security Layer
    Gate->>PII: Scan Payload
    PII-->>Gate: Redacted Prompt (Entities Masked)
    end
    
    Gate->>Model: Forward Safe Prompt
    Model-->>Gate: Generation
    
    rect rgb(220, 255, 220)
    Note over Gate, Smith: Evaluation Layer
    Gate->>PII: Re-hydrate (Optional)
    Gate->>Smith: Log Trace & Latency
    end
    
    Gate-->>Dev: Final Response
