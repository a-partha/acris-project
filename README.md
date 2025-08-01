
# NYC ACRIS Legal Data Pipeline with AI Integration

## Project Overview
This project demonstrates how to build a modern, local ELT (Extract, Load, Transform) pipeline using public legal-property records from NYC’s ACRIS (Automated City Register Information System) dataset. The goal is to structure raw legal filings data for analytics and power intelligent question-answering using AI models—all without relying on cloud resources.

## Pipeline Stack
- **Data Source:** NYC ACRIS Real Property Legals (official open dataset)
- **API/ELT:** Python for data retrieval and automation
- **Database:** DuckDB (local, lightweight OLAP database)
- **Transformation:** dbt (data build tool) for modular, production-style data modeling
- **Preprocessing & Feature Engineering:** Python, Pandas
- **Vector Search:** Scikit-learn (TF-IDF vectorization, Nearest Neighbors search)
- **AI/LLM Integration:** Google's Gemini-1.5-flash (for Q&A over structured data)

## What the Pipeline Does
1. **Pulls Raw Data**: Retrieves legal property record filings via NYC’s public API. Stores as structured CSV for reproducibility.
2. **Models Data with dbt & DuckDB**: Implements a dbt project that cleans, transforms, and documents the ACRIS dataset inside a local DuckDB instance.
3. **Maps Codes to Plain English**: Uses the official data dictionary to translate coded fields (e.g., borough numbers, document types) into human-readable labels.
4. **Preps for AI Search**: Creates natural language descriptions of each record to make the data searchable by meaning, not just keyword.
5. **Builds Vector Index**: Trains a TF-IDF model to embed those descriptions and enables fast similarity search with NearestNeighbors.
6. **Retrieves and Answers**: Given a user question, finds the most relevant filings and sends their summaries to an LLM to generate answers.

## Workflow
1. **Data Extraction**: Python scripts connect to the ACRIS API and batch-download raw property legal records (fields like `document_id`, `borough`, `property_type`, etc).
2. **Initial Data Storage**: Stores the raw records as CSV for reproducible downstream processing.
3. **Transformation and Modeling**: Uses dbt models to:
   - Clean missing or malformed fields
   - Map coded fields (e.g., borough 1 → Manhattan)
   - Enrich the data using the official data dictionary
   - Output a documented, queryable clean dataset in DuckDB
4. **Feature Engineering**: Converts each legal record into a short, human-readable summary (using mapped codes and descriptive language).
5. **Vectorization**: Uses Scikit-learn’s TF-IDF to embed summaries, creating a vector index for semantic search (finds similar records by meaning).
6. **Retrieval-Augmented Generation**:
   - For any user question, retrieves the most relevant filings using the vector index
   - Passes those records as context to an LLM (Google Gemini) to generate a natural-language answer

## Key Results
- Built a fully local ELT+AI pipeline—no cloud dependencies
- Modeled legal data to be both analyst- and AI-ready (plain English, not just codes)
- Demonstrated how dbt and DuckDB can power rapid prototyping and modular pipelines for legal/real estate data
- Showed how vector search and LLMs enable natural-language Q&A over structured public data

## Skills Demonstrated
- **Pipeline Engineering:** End-to-end data ingestion, modeling, and deployment using modern open-source tools
- **Data Transformation:** dbt workflow, code-to-label mapping, clean data modeling
- **Feature Engineering:** Turning structured records into AI-ready natural language summaries
- **Vector Search/AI:** Building vector indexes for retrieval-augmented LLM question answering
- **Documentation & Reproducibility:** All steps are modular, documented, and reproducible for real-world analytics workflows

## Example Prompt
> “What borough is document 2025061800453002 in?”

The pipeline retrieves the record, decodes the borough, and generates the answer in plain English using the data dictionary mapping and the AI model.

---

**Repo Structure (suggested):**
- `/notebooks` – Pipeline notebooks and experiments
- `/dbt` – dbt project for data modeling
- `/data` – Raw and processed data snapshots
- `/scripts` – Standalone ELT and AI modules
