```mermaid
graph TB
    subgraph "Your Computer"
        A[Developer/You] -->|Natural Language Requests| B[Python Assistant]
        B -->|API Calls| C[Ollama Service]
        C -->|Inference| D[Qwen3-Coder-Next Model<br/>256K Context Window]
        
        B -->|Tool Calls| E[File System Tools]
        B -->|Tool Calls| F[Command Execution]
        B -->|Tool Calls| G[Code Analysis]
        
        E -->|Read/Write| H[Your Codebase]
        F -->|Execute| I[Terminal Commands]
        G -->|Analyze| H
        
        D -->|Responses| B
        B -->|Generated Code<br/>Explanations<br/>Debugging| A
    end
    
    style A fill:#e1f5ff
    style D fill:#ffe1e1
    style B fill:#e1ffe1
    style H fill:#fff9e1
    
    classDef local fill:#f0f0f0,stroke:#333,stroke-width:2px
    class C,D,E,F,G,H,I local
```

# Architecture Diagram: Local Coding Assistant

This diagram shows how the components interact:

1. **You** interact with the Python Assistant using natural language
2. **Python Assistant** manages conversation and calls tools
3. **Ollama Service** hosts and runs the model locally
4. **Qwen3-Coder-Next** performs the actual AI inference
5. **Tools** (file system, commands, analysis) execute actions
6. **Your Codebase** is accessed and modified locally

**Key Point:** Everything runs on your machine - no cloud services involved!

---

## Data Flow Example

```mermaid
sequenceDiagram
    participant U as Developer
    participant A as Assistant
    participant O as Ollama
    participant M as Qwen3 Model
    participant F as File System
    
    U->>A: "Create a FastAPI endpoint"
    A->>O: Send request with tools
    O->>M: Process with context
    M->>O: Generate response + tool calls
    O->>A: Return tool calls needed
    A->>F: write_file(main.py, code)
    F->>A: Success
    A->>U: "Created endpoint at main.py"
    
    Note over U,F: All operations happen locally
```

This shows a typical interaction where:
1. You request code generation
2. Assistant communicates with Ollama
3. Model decides to use tools
4. Files are written locally
5. Results are returned to you

---

## System Resource Usage

```
┌─ Typical Resource Usage ─────────────────────────┐
│                                                   │
│  RAM Usage (q4_K_M quantization):                │
│  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓░░░░░░░░  46 GB / 64 GB       │
│                                                   │
│  CPU Usage (during inference):                   │
│  ▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░░░  30-40%               │
│                                                   │
│  Disk Space:                                      │
│  ▓▓▓▓▓▓▓▓▓▓░░░░░░░░░░░░░░  ~52 GB               │
│                                                   │
│  Network: 0 (fully offline)                       │
│                                                   │
└───────────────────────────────────────────────────┘
```