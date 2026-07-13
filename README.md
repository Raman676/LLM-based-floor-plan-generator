# 🏗️ AI-FloorPlan-Gen: Multi-Agent Architectural Layout Engine

[![Python 3.10+](https://img.shields.io/badge/python-3.10+-3670A0?style=for-the-badge&logo=python&logoColor=ffdd54)](https://www.python.org/)
[![LangGraph](https://img.shields.io/badge/Orchestration-LangGraph-orange?style=for-the-badge)](https://github.com/langchain-ai/langgraph)
[![Pydantic v2](https://img.shields.io/badge/Validation-Pydantic%20v2-red?style=for-the-badge&logo=pydantic&logoColor=white)](https://docs.pydantic.dev/)
[![HuggingFace](https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Models-yellow?style=for-the-badge)](https://huggingface.com/)

An end-to-end, multi-stage **LangGraph** workflow that translates unstructured natural language architectural descriptions into syntactically validated, geometrically constrained floor plan JSON datasets. 

> 🔗 **Interactive Notebook:** Run the complete execution pipeline directly on [Google Colab](https://colab.research.google.com/drive/1dBhAPwqohhNzaXaG3H4EADK80KueqfNL?usp=sharing).

---

## 🌟 Key Capabilities

*   **Deterministic Information Extraction:** Combines `LangChain` with strong `Pydantic v2` schemas to extract room specifications, dimension variations, and spatial constraints from unstructured conversational text.
*   **Algorithmic Spatial Alignment:** Utilizes a logic-driven relative positioning engine to resolve spatial layouts based on dynamic adjacency matrices.
*   **Unit Normalization Layer:** Translates arbitrary physical units (e.g., sq ft, sq meters, feet, inches) down to a globally consistent internal standard of **Square Inches** to eliminate down-stream scaling errors.
*   **Asset Injection Engine:** A rule-based micro-engine that populates functional architectural spaces with appropriate furniture vectors (e.g., specific dimensions for beds, wardrobes, and sinks) based on room classification profiles.

---

## 📈 State Graph Architecture

The workflow utilizes an explicit state-graph loop implemented in **LangGraph**, preventing agent drift and ensuring deterministic validation gates at each transition point:

```text
[User Input] 
     │
     ▼
┌──────────────┐      Validates JSON Schema
│  Room        │ ────► [Pydantic Guardrail]
│  Extraction  │ 
└──────────────┘
     │
     ▼
┌──────────────┐      Identifies Openings & Adjacencies
│  Connection  │
│  Mapping     │
└──────────────┘
     │
     ▼
┌──────────────┐      Calculates Relative [X, Y] Metrics
│  Spatial     │
│  Positioning │
└──────────────┘
     │
     ▼
┌──────────────┐      Populates Internal Assets & Normalizes Scale
│ Furniture &  │
│ Finalization │
└──────────────┘
     │
     ▼
 [Validated Output Schema]
```

---

## 🛠️ Tech Stack & Prerequisites

*   **Orchestration & State Management:** LangGraph
*   **LLM Interface & Tooling:** LangChain / LangChain-HuggingFace
*   **Data Validation & Typing:** Pydantic v2
*   **Base Model Inference:** `openai/gpt-oss-20b` (via Hugging Face Dedicated Inference Endpoints)

---

## 🚀 Getting Started

### 1. Installation
Install the core framework and runtime dependencies:
```bash
pip install langchain langchain-huggingface langgraph pydantic huggingface-hub
```

### 2. Environment Setup
Configure your Hugging Face API access token within your workspace environment variables:
```bash
export HUGGINGFACEHUB_API_TOKEN="your_hf_token_here"
```

### 3. Execution Example
```python
from engine import FloorPlanGraphEngine

# Initialize the workflow graph
app = FloorPlanGraphEngine.compile()

# Invoke with complex, unstructured text specifications
input_prompt = {
    "user_input": "Design a modern 2 BHK apartment. The master bedroom should be 12x14 ft attached to a small balcony. Connect the kitchen directly to the dining room."
}

response = app.invoke(input_prompt)
print(response["final_floor_plan"])
```

---

## 📊 Validated Data Schema

The workflow enforces a strict schema guaranteeing predictable payloads for front-end renderers or CAD export scripts (`ezdxf`):

### Top-Level Entity Structure
| Field | Type | Description |
| :--- | :--- | :--- |
| `rooms` | `Array<RoomObject>` | Collection of all extracted rooms containing IDs, types, assets, and boundaries. |
| `connections` | `Array<LinkObject>` | Directional map establishing doors, windows, and structural adjacencies. |
| `total_area` | `Integer` | Sum total of physical property area explicitly calculated in **square inches**. |

### Structural Object Definitions
```json
{
  "rooms": [
    {
      "id": "room_01",
      "type": "master_bedroom",
      "dimensions": { "width_in": 144, "length_in": 168 },
      "position": { "x": 0, "y": 0 },
      "windows": 2,
      "doors": 1,
      "furniture": [
        { "asset_id": "bed_01", "type": "king_bed", "dimensions": [76, 80] }
      ]
    }
  ],
  "connections": [
    { "from": "room_01", "to": "room_02", "type": "interconnected_door" }
  ],
  "total_area": 24192
}
```
