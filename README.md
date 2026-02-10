# Azure Multi-Modal Compliance Orchestration Engine (LangGraph + LangSmith)

🚀 **From hours of manual video audits → seconds of automated intelligence**

An enterprise-grade AI system that audits video content for regulatory and brand compliance using a multi-modal pipeline (video + audio + text). The platform ingests a YouTube video, extracts transcript and OCR with Azure Video Indexer, retrieves relevant rules via Azure AI Search, and uses Azure OpenAI with LangGraph to produce a structured compliance report. LangSmith provides LLM tracing, while Azure Application Insights delivers production observability.

---

## 📌 Overview

The **Azure Multi-Modal Compliance Orchestration Engine** automates video compliance checks end-to-end:

- 🎥 **Azure Video Indexer** for speech-to-text and OCR extraction
- 🧠 **LangGraph** for stateful workflow orchestration
- 🔍 **Azure AI Search (RAG)** for evidence-driven retrieval
- 🤖 **Azure OpenAI** for reasoning and decision-making
- 🧪 **LangSmith** for tracing, debugging, and prompt/response analysis
- 📊 **Azure Application Insights** for production monitoring and reliability

Instead of human auditors reviewing long footage, the system **ingests, extracts, retrieves, reasons, and generates structured compliance reports automatically**.

---

## 🎯 Problem Statement

Organizations in finance, healthcare, media, and security must continuously monitor video content to ensure compliance with regulations (misleading claims, restricted visuals, sensitive disclosures). Manual reviews are slow, costly, and error-prone. This project solves that by:

✅ Automating video compliance checks  
✅ Providing evidence-backed findings  
✅ Enabling explainability with LLM tracing  
✅ Producing structured JSON audit reports  

---

## 🏗️ System Architecture

### High-Level Workflow

1. **Trigger (API / CLI)**
   - User submits a YouTube URL.
   - Audit job is created.

2. **Azure Video Indexer**
   - Extracts transcript, OCR text, and timestamps.

3. **LangGraph Orchestration**
   - Coordinates indexing, retrieval, reasoning, and report generation.

4. **Azure AI Search (RAG)**
   - Retrieves applicable compliance rules and policies.

5. **Azure OpenAI + LangSmith**
   - LLM evaluates evidence and returns structured JSON output.
   - LangSmith traces every step for debugging and auditability.

6. **Azure Application Insights**
   - Captures latency, failures, dependencies, and health metrics.

7. **Final Output**
   - Compliance report with violations, evidence, and status.

---

## 🧩 Key Technical Components

| Layer | Technology | Purpose |
| --- | --- | --- |
| Video Processing | Azure Video Indexer | Transcript + OCR + timestamps |
| Orchestration | LangGraph | Stateful AI workflow |
| Retrieval | Azure AI Search | Compliance rule lookup |
| LLM Reasoning | Azure OpenAI | Audit decisions + report |
| Tracing | LangSmith | LLM observability |
| Monitoring | Application Insights | Metrics + reliability |

---

## 📁 Repository Structure

```
.
├── backend/
│   ├── src/
│   │   ├── api/          # FastAPI server + telemetry
│   │   ├── graph/        # LangGraph nodes + workflow
│   │   └── services/     # Azure Video Indexer integration
│   └── tests/
├── main.py               # CLI simulation runner
├── pyproject.toml        # Dependencies + Python version
└── README.md
```

---

## ⚙️ Prerequisites

- Python **3.13+**
- Azure resources:
  - Video Indexer
  - Azure OpenAI (chat + embeddings deployments)
  - Azure AI Search
  - Application Insights (for telemetry)
- (Optional) LangSmith account for tracing

---

## 🚀 Setup

1. **Create and activate a virtual environment**.
2. **Install dependencies**:

```bash
uv sync
```

3. **Configure environment variables** (via `.env`):

```bash
# Azure OpenAI
AZURE_OPENAI_ENDPOINT=
AZURE_OPENAI_API_KEY=
AZURE_OPENAI_API_VERSION=
AZURE_OPENAI_CHAT_DEPLOYMENT=
AZURE_OPENAI_EMBEDDINGS_DEPLOYMENT=

# Azure AI Search
AZURE_SEARCH_ENDPOINT=
AZURE_SEARCH_API_KEY=
AZURE_SEARCH_INDEX_NAME=

# Azure Video Indexer
AZURE_VI_ACCOUNT_ID=
AZURE_VI_LOCATION=
AZURE_SUBSCRIPTION_ID=
AZURE_RESOURCE_GROUP=
AZURE_VI_NAME=project-brand-guardian-001

# Azure Application Insights
APPLICATIONINSIGHTS_CONNECTION_STRING=

# LangSmith (optional)
LANGCHAIN_TRACING_V2=true
LANGCHAIN_API_KEY=
LANGCHAIN_PROJECT=
```

---

## ▶️ Run the API

```bash
uv run uvicorn backend.src.api.server:app --reload
```

- Swagger UI: `http://localhost:8000/docs`
- Health check: `http://localhost:8000/health`

### Example API request

```bash
curl -X POST http://localhost:8000/audit \
  -H "Content-Type: application/json" \
  -d '{"video_url": "https://youtu.be/dT7S75eYhcQ"}'
```

---

## ▶️ Run the CLI Simulation

```bash
uv run python main.py
```

The CLI executes the same LangGraph workflow and prints a compliance report to stdout.

---

## 🧠 Example Output (JSON Report)

```json
{
  "video_id": "vid_12345",
  "audit_id": "audit_98765",
  "processed_at": "2026-02-05T10:00:00Z",
  "summary": "Two potential compliance violations detected.",
  "violations": [
    {
      "rule_id": "FIN-03",
      "severity": "HIGH",
      "timestamp_start": "00:02:15",
      "timestamp_end": "00:02:35",
      "evidence": {
        "transcript": "Guaranteed returns with no risk...",
        "ocr": "Earn 20% monthly"
      },
      "confidence": 0.87,
      "recommended_action": "Flag for legal review"
    }
  ],
  "uncertainties": [
    "Low confidence OCR detected between 00:10:00 and 00:10:20"
  ]
}
```

---

## 🔍 LangSmith Tracing & Observability

- **LangSmith** provides trace visibility into every LangGraph node, input/output, and LLM call.
- **Application Insights** captures dependency maps, performance, failure rates, and system health for production workloads.

---

## 🛠️ Troubleshooting

- **Missing transcript**: Ensure Azure Video Indexer finished processing and the video is accessible.  
- **Authentication errors**: Verify Azure credentials and deployment names.  
- **Search results empty**: Confirm AI Search index contains compliance rules.  
- **Tracing not visible**: Ensure `LANGCHAIN_TRACING_V2=true` and your LangSmith API key are set.  

---

## 🛣️ Future Enhancements

- Human-in-the-loop review dashboard
- Speaker recognition
- Live stream compliance audits
- Image + document compliance support
- Real-time alerts

---

## License

This repository is licensed under the terms in `MIT LICENSE`.
