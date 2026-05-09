# IntelliSpec AI: Semantic UML Abstraction Engine

IntelliSpec AI is a state-of-the-art (SOTA) requirement engineering platform that ingests raw Software Requirements Specification (SRS) documents and intelligently synthesizes them into visual, interactive, and standard-compliant software architecture diagrams. 

Moving beyond legacy "one-requirement-to-one-shape" mapping, IntelliSpec AI utilizes a 3-phase LLM-driven semantic pipeline to understand the *architectural intent* behind requirements, generating professional-grade Use Case diagrams, Activity diagrams, and Data Flow Diagrams (DFDs).

## 🚀 Key Features

* **3-Phase Semantic Intelligence Pipeline:** Uses NVIDIA NIM (`meta/llama-3.1-8b-instruct`) to execute a strict, schema-enforced pipeline: `Classification` ➜ `Goal Abstraction` ➜ `Relationship Generation`.
* **Multi-Diagram Support:** Dynamically generates entities and syntax mapping for:
  * **Use Case Diagrams** (Actors, Use Cases, System Boundaries, `<<include>>` / `<<extend>>` dependencies).
  * **Activity Diagrams** (Start Nodes, End States, Action States, Decisions).
  * **Data Flow Diagrams (DFDs)** (External Entities, Processes, Data Stores).
* **Interactive JointJS Canvas:** A highly responsive, interactive drawing canvas that enforces UML/DFD notation standards natively.
* **Semantic Traceability:** Hover over any rendered node on the canvas to inspect the exact source requirement that generated it.
* **Auto-Layout & Smart Routing:** Features a "Place All" auto-alignment algorithm and natively renders standard UML dashed-line styling for semantic dependencies.
* **Aggressive Caching:** Sub-second UI responsiveness achieved through local semantic state caching.

## 🧠 Architecture Under the Hood

The platform is split into a robust intelligence backend and an interactive frontend:

1. **The Intelligence Engine (`backend/pipeline/uml_semantic_engine.py`)**
   - Parses raw Markdown SRS documents.
   - Interfaces with NVIDIA NIM APIs to synthesize raw functional requirements into professional-grade UML schemas.
   - Standardizes the output JSON to include explicit entities and semantic relationships, eliminating parsing failures through strict schema constraints.
   - Caches abstractions locally (`stage7_uml_semantic/`) to guarantee rapid retrieval.

2. **The JointJS Rendering Pipeline (`src/components/DiagramCanvas.jsx`)**
   - Receives the semantic JSON and translates it into joint shapes (`customShapes.js`).
   - Acts as the single source of truth for rendering, ensuring standard compliance (e.g., open rectangles for DFD Data Stores, solid filled circles for Activity Start Nodes).
   - Manages dark/light mode themes, grid snapping, interactive tools, and drag-and-drop state.

## 🛠️ Tech Stack

* **Frontend:** React, Vite, JointJS, Lucide-React
* **Backend:** Python, FastAPI
* **AI / LLM Engine:** NVIDIA NIM (`meta/llama-3.1-8b-instruct`)

## ⚙️ Local Setup & Installation

### Prerequisites
* Node.js (v18+)
* Python 3.10+
* NVIDIA NIM API Key (`NVIDIA_API_KEY`)

### 1. Backend Configuration
Navigate to the backend directory and set up the Python environment:
```bash
cd backend
python -m venv uml_venv
# Activate venv:
# Windows: .\uml_venv\Scripts\activate
# Mac/Linux: source uml_venv/bin/activate

pip install -r requirements.txt
```

Set your NVIDIA API key in a `.env` file in the root or backend folder:
```env
NVIDIA_API_KEY=your_api_key_here
```

Start the FastAPI server:
```bash
uvicorn api:app --host 0.0.0.0 --port 8000 --reload
```

### 2. Frontend Configuration
Open a new terminal window, navigate to the project root, and install the React dependencies:
```bash
npm install
```

Start the Vite development server:
```bash
npm run dev
```

The application will be available at `http://localhost:5173/`.

## 📁 Project Structure

```text
IntelliSpec-AI/
├── backend/
│   ├── api.py                      # FastAPI gateway
│   ├── pipeline/
│   │   └── uml_semantic_engine.py  # 3-Phase LLM semantic abstraction logic
│   └── data/
│       └── raw_SRS_processed/      # Cached semantic JSONs and cleaned Markdown
├── src/
│   ├── components/
│   │   ├── Dashboard.jsx           # Main orchestration, state, and UI layout
│   │   ├── DiagramCanvas.jsx       # JointJS core canvas engine
│   │   └── NodePanel.jsx           # AI Workspace sidebar and node categorization
│   └── joint-logic/
│       └── customShapes.js         # Single source of truth for standard UML/DFD shapes
└── package.json
```
