# MatchPoint ⚽✈️🏠
### European Football Tourism Recommendation Engine
**Data Collection & Management Lab (00940290) – Final Project**

MatchPoint is a data-driven decision support tool designed for sports tourists attending international football matches in Europe. By integrating Airbnb accommodation data with match schedules and stadium geospatial information, MatchPoint ranks and recommends properties that optimize for **quality**, **price**, and **proximity** to the match.

---

## 🚀 Project Overview

The core objective of this project is to solve the logistical challenge football fans face: finding high-quality, affordable accommodation near a stadium for a specific match date. The system utilizes a robust ETL pipeline, LLM enrichment, and a custom ranking algorithm (VFM Score) to filter thousands of listings down to the best options.

### Key Features
* **🏟️ Stadium Geolocation Scraping**: Automated extraction of GPS coordinates for over 150 European stadiums using Wikipedia and `BeautifulSoup`.
* **🧠 LLM Data Enrichment**: Uses LangChain to generate rich, context-aware "Stadium Guides" for tourists (e.g., history, atmosphere, tips).
* **⚡ PySpark Data Processing**: Handles large-scale Airbnb datasets, performing spatial filtering and date matching efficiently on Databricks.
* **💎 Value-for-Money (VFM) Engine**: A custom scoring metric that balances property rating, price per guest, and Superhost status.
* **🗺️ Interactive Output**: Generates a user-friendly HTML application (`MatchPoint_App.html`) for exploring matches and recommendations.

---

## 📂 Repository Structure

This repository is organized into three main modules corresponding to the project pipeline:

| File | Description |
| :--- | :--- |
| **scraping.ipynb`** | **Data Extraction**. Scrapes Wikipedia for stadium coordinates using `BeautifulSoup` and `Bright Data` proxies. Cleans match schedule data and performs fuzzy matching to align team/stadium names. Exports `Match_Schedule_All_Leagues.csv`. |
| **`llm_enrichment.ipynb`** | **Data Enrichment**. Uses `LangChain` and Databricks Foundation Models to generate descriptive "Stadium Guides" for the top venues. Exports `stadium_guides.json`. |
| **`Final Project - MatchPoint.ipynb`** | **Main Application Logic**. Loads the Airbnb data and the artifacts from the previous steps. It filters listings by date and location, calculates the VFM score, and generates the final HTML dashboard (`MatchPoint_App.html`). |

---

## 🛠️ Architecture & Methodology

### 1. Data Collection (ETL)
* **Source**: Wikipedia (16 European Domestic Leagues) & Open Data CSVs.
* **Technique**: Web scraping with dynamic column detection to handle varying table formats on Wikipedia pages.
* **Infrastructure**: `Bright Data` proxy network is used to prevent IP bans during scraping.

### 2. Enrichment (LLM)
* **Tool**: Databricks LangChain (`databricks-langchain`).
* **Task**: Generates summaries for stadiums to provide tourists with non-quantitative context (atmosphere, safety, local tips).

### 3. The MatchPoint Algorithm (PySpark)
The system filters Airbnb listings based on a selected match's **Date** and **Stadium Location**. It then applies a **Value-for-Money (VFM)** formula:

$$VFM = (QualityScore \times PriceScore \times SuperhostFactor)$$

* **Quality Score**: Normalized ratings and review counts.
* **Price Score**: Inverse normalization of price per guest (lower price = higher score).
* **Proximity**: Filters strictly for listings within a calculated radius of the stadium (Lat/Lon).

---

## 💻 Setup & Usage

### Prerequisites
* **Databricks Runtime**: 14.3 LTS or higher (supports PySpark & LangChain).
* **Azure Blob Storage**: Data is read from/written to Azure containers (`submissions` and `airbnb`) using SAS tokens.

### Dependencies
The following libraries are required:
```python
beautifulsoup4
pyspark
langchain
langchain-community
databricks-langchain
