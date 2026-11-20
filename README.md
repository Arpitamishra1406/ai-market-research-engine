# AI Market Research Engine

**An AI-powered market research platform that turns one question into a source-backed strategy report, using LLMs, AI agents and automated research workflows.**

![Python](https://img.shields.io/badge/Python-3.12-3776AB?logo=python&logoColor=white)
![Next.js](https://img.shields.io/badge/Next.js-Frontend-000000?logo=nextdotjs&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-Backend-009688?logo=fastapi&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-Orchestration-1C3C3C)
![Status](https://img.shields.io/badge/Status-Academic%20Project-orange)

[Live Demo](https://mckinsey-ai-market-research-engine-beta.vercel.app/) ·
[Presentation](https://github.com/Arpitamishra1406/ai-market-research-engine/blob/main/docs/AI_Market_Research_Strategy_Engine.pptx)

</div>

> Academic project by a team of 5. Not affiliated with McKinsey & Company.

---

## Overview

Market research is slow. Analysts search many websites, copy facts by hand, and often cannot tell which numbers are reliable. **AI Market Research Engine** automates this work.

A user types a research question, for example *"Analyze the Indian EV market and its competitors"*, and picks a sector, geography and timeframe. A team of six specialised AI agents then plans the research, collects sources from the web, extracts claims, **validates the evidence**, analyses the findings and writes a structured report with citations.

The most important design idea is that **no claim reaches the report unchecked**. Every fact is linked to its source and gets a confidence score first.

## Key Features

- **Natural-language input.** Ask a business question in plain English, with sector, geography, timeframe and report type.
- **Multi-agent workflow.** Six agents, each with one clear job, coordinated by LangGraph.
- **Source-backed evidence.** Every claim keeps its source URL, publisher and date.
- **Evidence validation.** Each claim is checked for source credibility, relevance, support, recency and agreement with other sources, then given a confidence score.
- **Duplicate and conflict detection.** Repeated claims are merged, and conflicting numbers are flagged.
- **Research memory.** Jobs, sources, evidence and reports are stored, so past research can be searched again.
- **Strategy-ready output.** The website describes 12 analyses, including market sizing, competitive landscape, SWOT and Porter's Five Forces, pricing, go-to-market, risk assessment and an executive summary.
- **Downloadable PDF report** with citations and an evidence appendix.

## How It Works

```
User question
     |
     v
 Research Planner (Agent 1)        breaks the question into sub-questions
     |
     v
 Web Research (Agent 2)            searches and collects relevant sources
     |
     v
 Content Extraction (Agent 3)      pulls out claims, figures and quotes
     |
     v
 Evidence Validation (Agent 4)     scores, de-duplicates and flags conflicts
     |
     v
 Analysis & Synthesis (Agent 5)    finds themes, trends, opportunities, risks
     |
     v
 Report Generation (Agent 6)       builds the report with citations and PDF
```

All agents read from and write to a shared database ("research memory").

| Step | Stage | Agent | Output |
|---|---|---|---|
| 1 | User Input | Frontend | Research request |
| 2 | Research Planner | Agent 1 | Research plan and search queries |
| 3 | Web Research & Source Collection | Agent 2 | List of relevant sources |
| 4 | Content Extraction | Agent 3 | Structured evidence |
| 5 | **Evidence Validation** | Agent 4 | Validated evidence with confidence scores |
| 6 | Analysis & Synthesis | Agent 5 | Insights, charts and tables |
| 7 | Report Generation | Agent 6 | Final report and PDF |

## Why This Approach (Advantages)

| Advantage | What it means |
|---|---|
| **Saves time** | Planning, searching, reading and writing are automated, so hours of manual work become one request. |
| **Trustworthy by design** | A dedicated validation step filters weak, outdated, off-topic and duplicate evidence before analysis. |
| **Transparent** | Every claim has a source, a status and a note explaining the decision, so users can verify it. |
| **Modular** | Each agent has one job and a clear JSON input and output, so a part can be improved or replaced without touching the rest. |
| **Reusable knowledge** | Research memory lets the system build on earlier work instead of starting from zero. |
| **Beginner-friendly** | The user needs only a question. No research or data skills are required. |

## Tech Stack

| Layer | Tools |
|---|---|
| Frontend | Next.js, Tailwind CSS |
| Backend | Python, FastAPI |
| LLM | Gemini API |
| Web search | Tavily |
| Page extraction | Trafilatura, Playwright |
| Database | PostgreSQL (Supabase) |
| Vector search | pgvector |
| Analytics | Pandas, Plotly |
| PDF generation | ReportLab |
| Orchestration | LangGraph, Pydantic, Celery |
| Hosting | Vercel (frontend), Render (backend) |

## Team

The project is split into six areas, shared by five team members.

| Area | Main responsibilities | Team members |
|---|---|---|
| Frontend & UI | Interface, research input form, progress tracking, evidence review, report viewer | Subodh Kumar |
| Backend & API | FastAPI backend, API endpoints, job management, workflow integration, error handling | Nihal Tripathi |
| AI Planner & Orchestration | Research planner, LangGraph workflow, agent coordination, retries and state management | Subodh Kumar, Nihal Tripathi, Akash Singh, Arpita Pandey, Amit Karmakar |
| Web Research & Extraction | Web search, content extraction, structured evidence, source metadata | Akash Singh, Nihal Tripathi |
| Database & Validation | Database schema, evidence validation, conflict detection, research memory | Arpita Pandey |
| Analytics, Reporting & QA | Data analysis, charts, report generation, testing and QA, deployment docs, demo video | Amit Karmakar |

## Repository Structure

```
README.md                 Project overview (this file)
docs/                     Presentation and documents
frontend/                 Website (Next.js)
backend/                  API (FastAPI)
agents/
  validation_agent/       Evidence Validation Agent (Agent 4)
```

Each folder has its own README with setup steps. Folder names can be adjusted by the team.

## Getting Started

**Requirements:** Python 3.12 or newer. Node.js is needed for the frontend.

```bash
git clone https://github.com/Arpitamishra1406/ai-market-research-engine.git
cd ai-market-research-engine
```

Frontend and backend setup steps are in `frontend/README.md` and `backend/README.md`. Keep API keys in environment variables and never commit them.

**Try the Evidence Validation Agent:**

```bash
cd agents/validation_agent
pip install -r requirements.txt
python run_validation.py          # validates the sample evidence and prints a table
python -m pytest tests -q         # runs the automated tests
```

The first run downloads a small embedding model (about 90 MB).

## Evidence Validation Agent (Agent 4)

Takes structured evidence from Agent 3 and gives every claim a status and a confidence score before analysis begins.

**Five checks and their weights:** source credibility (30%), evidence support (25%), relevance (20%), recency (15%) and corroboration by other sources (10%).

| Status | Meaning |
|---|---|
| `validated` | Trusted, relevant and well supported |
| `flagged` | Usable with care (weak source or a conflict) |
| `rejected` | Off-topic, unsupported or very low confidence |
| `duplicate` | Same claim as another source, merged |

**Use it in code:**

```python
from agent import validate_evidence
results = validate_evidence(evidence_list, context="user research question")
```

**Use it as a LangGraph node:**

```python
from validation_node import validation_node
graph.add_node("validate", validation_node)
```

Full input and output fields are in [`agents/validation_agent/README.md`](agents/validation_agent/README.md).

## Project Status

| Part | Status |
|---|---|
| Frontend | Live demo deployed. Backend integration pending |
| Validation Agent | Complete, with 16 automated tests on mock data |
| Full pipeline | End-to-end integration with real agent output is still to be done |

## Future Scope

- **End-to-end integration** of all agents with the live website, with real-time progress for each step.
- **Smarter validation.** Replace the word-and-number support check with an LLM-based check, and tune thresholds on real data.
- **Richer data sources.** Add trusted APIs and official publications (for example government and industry data) beyond web search.
- **Human-in-the-loop review.** Let users accept, reject or comment on individual pieces of evidence before the report is written.
- **More export formats.** Word and PowerPoint reports, and interactive dashboards with charts.
- **User accounts and history.** Saved projects, shared workspaces and comparison between research runs.
- **Multilingual support**, including Hindi and other Indian languages.
- **Evaluation and monitoring.** Benchmarks for answer quality, cost and speed, and alerts when sources change.
- **Scheduled research.** Automatic refresh of a report when new data appears.

## Notes

- This is an academic project. Reports are generated for learning and should not be treated as professional advice.
- Do not commit API keys or passwords. Keep them in environment variables.
