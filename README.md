# DWL03 Lakeside Beatmap

Beatmap is a data-driven analytics project designed to identify underserved music genres in the UK event industry. The solution is implemented as a data lake and data warehouse architecture on Amazon Web Services (AWS), with ingestion, transformation, and analytical logic provided in this repository. Curated datasets from the gold layer are consumed by a Tableau dashboard for business intelligence and exploratory analysis.

The final Tableau dashboard based on the gold-layer views is available here:
https://public.tableau.com/app/profile/andreas.goerre/viz/Beatmap_dynamic_v2/FinalDashboard?publish=yes

---

## 1. Project Overview
**Course:** Data Warehouse and Data Lake (DWL03)  
**Institution:** Lucerne University of Applied Sciences and Arts (HSLU)  
**Semester:** HS2025  
**Authors:**  
- Andreas Goerre
- Lukas Kramer
- Sheena Walker

---

## 2. Execution
The project is executed using AWS-managed services.

- Scripts in the `bronze` layer are designed to run as AWS Lambda functions and ingest raw data from external APIs into Amazon S3.
- Scripts in the `silver` and `gold` layers are executed as SQL statements in AWS Athena to clean, transform, and curate the data.
- The final analytical views in the gold layer are consumed by Tableau via an Athena connection.

- Note: AWS Glue jobs are used for orchestration and execution but do not contain custom transformation logic. As no project-specific code is implemented within Glue itself, these jobs are not included in this repository.

---

## 3. Repository Structure
Short explanation of the folder structure and the purpose of the scripts:
```text
.
├── bronze/                                         # Raw data ingestion layer (data lake – bronze)
│   ├── googlesearch-api/                           # Scripts to fetch Google Search demand data via API and store raw results
│   ├── last-fm-api/                                # Scripts to fetch music listening data from the Last.fm API and upload to S3
│   └── ticketmaster-api-fetch/                     # Scripts to fetch raw event data from the Ticketmaster API
│
├── silver/                                         # Cleaned and standardized datasets (data lake – silver)
│   ├── google_search_clean_v1                      # Cleans and normalizes Google Search API results
│   ├── spotify_clean_v1                            # Cleans and prepares Spotify-derived data for analytical use
│   └── ticketmaster_dedup                          # Deduplicates and standardizes Ticketmaster event data
│
├── gold/                                           # Curated analytical layer / data warehouse (gold)
│   ├── dim_genre_map                               # Dimension table mapping genres across different platforms
│   ├── spotify_genre_attention_share               # Pre-aggregated Spotify genre attention metrics            
│   ├── v_google_demand_event                       # View exposing Google demand signals at event level
│   ├── v_google_event_visibility_bi                # BI-ready view for Google event visibility metrics                  
│   ├── v_supply_city_genre_share                   # View calculating event supply share per city and genre
│   ├── v_supply_city_mapped_genre_share            # View aligning city-level supply with mapped genre taxonomy                
│   ├── v_supply_demand_spotify_bi                  # BI-ready view combining Ticketmaster events and Spotify demand metrics
│   ├── v_supply_demand_spotify_join                # Join view for detailed Ticketmaster events vs. Spotify demand analysis
│   └── v_ticketmaster_event_with_google_demand     # Final view combining Ticketmaster events with Google demand data 
|
└── README.md                                       # Repository documentation and execution instructions

