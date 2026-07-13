# 🏗️ AI Floor Plan Generator
colab link : https://colab.research.google.com/drive/1dBhAPwqohhNzaXaG3H4EADK80KueqfNL?usp=sharing

This project implements a multi-stage **LangGraph** workflow to transform natural language descriptions into structured floor plan JSON data. It leverages **LangChain**, **HuggingFace LLMs**, and **Pydantic** for robust data extraction and spatial positioning.

## 🌟 Features

- **Natural Language Understanding**: Extracts room types, quantities, and dimensions from conversational text.
- **Autonomous Positioning**: Logic-based spatial arrangement of rooms relative to one another.
- **Connection Mapping**: Identifies doors and windows, linking rooms logically.
- **Deterministic Furniture Engine**: Automatically populates rooms with appropriate furniture (e.g., wardrobes in bedrooms, sinks in kitchens).
- **Area Normalization**: Supports multiple units (sq ft, sq meters) and converts them to square inches for internal consistency.

## 🛠️ Tech Stack

- **Orchestration**: LangGraph
- **LLM Framework**: LangChain / LangChain-HuggingFace
- **Data Validation**: Pydantic v2
- **Model**: `openai/gpt-oss-20b` (via HuggingFace Endpoint)

## 📈 Workflow Architecture

The generation process follows a linear directed graph:
1. **Room Extraction**: Parsers the user input into a list of room objects.
2. **Connection Extraction**: Identifies adjacency and openings (doors/windows).
3. **Room Positioning**: Calculates `[x, y]` coordinates based on room dimensions and connectivity.
4. **Furniture & Finalization**: Injects furniture assets and calculates total square footage.


## 🚀 Getting Started

1. **Install Dependencies**:
   ```bash
   pip install langchain langchain-huggingface langgraph pydantic huggingface-hub
   ```
2. **API Key**: Ensure a HuggingFace API token is set in your environment variables as `HUGGINGFACEHUB_API_TOKEN`.
3. **Run the App**: Invoke the `app` object with a `user_input` string.

## 📊 Output Schema

The final output is a validated JSON following the `FloorPlanSchema`:
- `rooms`: Array of objects (type, id, dimensions, position, windows, doors, furniture).
- `connections`: Array of directional links between rooms.
- `total_area`: Integer representing the total area in square inches.
