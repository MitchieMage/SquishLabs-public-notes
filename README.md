# SquishLabs Public Notes 🧪

**Welcome to the public brain dump of SquishLabs.**

Because my professional work involves high-compliance environments (US Defense & Tier-1 Banking), I cannot share my production code. However, I believe in **Architecture as Code**. 

This repository documents the reference architectures I research and build in my private lab, focusing on **Edge AI**, **Agentic Workflows**, and **Zero-Trust GenAI Gateways**.

---

## Architecture 1: "The Edge-Native Agent" (LangGraph Implementation)
**Concept:** Running a cyclic reasoning loop on constrained hardware (NVIDIA Jetson) without cloud dependency.
**Why LangGraph?** Standard chains are DAGs (Directed Acyclic Graphs). Real agents need *cycles* to reason, retry, and reflect.

```mermaid
graph TD
    User(User Query) --> State[Global State Management]
    State --> Router{Router Node}
    
    %% The Thinking Loop
    Router -- "Needs Context" --> RAG[Local Vector Store]
    Router -- "Needs Compute" --> Tool[Python Tool / Calculator]
    Router -- "Can Answer" --> LLM[Quantized LLM (Llama-3-8B)]
    
    %% The Cycles
    RAG --> Evaluator{Context Quality?}
    Tool --> Evaluator
    
    Evaluator -- "Poor Quality" --> Refine[Query Refinement Node]
    Refine --> Router
    
    Evaluator -- "Good Quality" --> LLM
    
    LLM --> Output(Final Response)
    
    style Router fill:#f9f,stroke:#333,stroke-width:2px
    style LLM fill:#bbf,stroke:#333,stroke-width:2px
    style Evaluator fill:#ff9,stroke:#333,stroke-width:2px
