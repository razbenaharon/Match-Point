# MatchPoint ⚽✈️🏠

### European Football Tourism Recommendation Engine

**Data Collection & Management Lab (00940290) – Final Project**

---

## 💻 Setup & Execution Guide

This guide provides strictly technical instructions on how to configure, build, and run the project from scratch. Follow the steps in the exact order presented.

### 1. Environment Configuration (`.env`)

Before running any notebooks, you must define the following environment variables (or Databricks Secrets) to enable access to proxies and storage:

```ini
# Bright Data Proxy Credentials (for scraping)
BRIGHTDATA_USER=your_username
BRIGHTDATA_PASSWORD=your_password

# Azure Blob Storage SAS Tokens (for data loading)
SAS_SUBMISSIONS="your_sas_token_for_submissions"
SAS_AIRBNB="your_sas_token_for_airbnb_data"
```

### 2. Execution Pipeline

Run the notebooks in the following strict sequential order. Each step generates artifacts required by the next.

#### ▶️ Step 1: Data Scraping

**Notebook:** `scraping.ipynb`

* **Action:** Connects to Wikipedia via Bright Data proxies to scrape match schedules and stadium coordinates.
* **Output:** Ensure the following files are saved successfully to DBFS:
    * `/dbfs/FileStore/Stadium_LLM_Enrichment.csv`
    * `/dbfs/FileStore/Match_Schedule_All_Leagues.csv`

#### ▶️ Step 2: LLM Enrichment

**Notebook:** `llm_enrichment.ipynb`

* **Input:** Reads `/dbfs/FileStore/Stadium_LLM_Enrichment.csv` (generated in Step 1).
* **Action:** Uses LangChain to generate descriptive guides for stadiums.
* **Output:** Ensure the JSON artifact is saved:
    * `/dbfs/FileStore/stadium_guides.json`

#### ▶️ Step 3: Main Application

**Notebook:** `Final Project - MatchPoint.ipynb`

* **Inputs (Automatically loaded):**
    * `Match_Schedule_All_Leagues.csv` (from Step 1)
    * `stadium_guides.json` (from Step 2)
    * Airbnb Dataset (loaded from Azure via `SAS_AIRBNB`)
* **Action:** Runs the logic engine to filter listings and calculate VFM scores.
* **Final Output:** Generates and displays a download link for:
    * `MatchPoint_App.html`
