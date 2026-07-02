# NOCCCD Data Team

Data infrastructure and analytics for **North Orange County Community College District** — managed by [NOCCCD District](https://www.nocccd.edu/) and serving [Cypress College](https://www.cypresscollege.edu/), [Fullerton College](https://www.fullcoll.edu/), and [North Orange Continuing Education (NOCE)](https://noce.edu/).

## Repositories

| Repo | Description | Stack |
|------|-------------|-------|
| [**nocccd-edw**](https://github.com/nocccd-data/nocccd-edw) | Enterprise Data Warehouse built with dbt on Oracle — staging, intermediate, and mart layers (dims + facts) | ![dbt](https://img.shields.io/badge/dbt-FF694B?logo=dbt&logoColor=white) ![Oracle](https://img.shields.io/badge/Oracle-F80000?logo=oracle&logoColor=white) ![SQL](https://img.shields.io/badge/SQL-4479A1?logo=amazondocumentdb&logoColor=white) |
| [**nocccd-scff**](https://github.com/nocccd-data/nocccd-scff) | Student Centered Funding Formula data loading, crosswalks, and analysis for district use | ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![Oracle](https://img.shields.io/badge/Oracle-F80000?logo=oracle&logoColor=white) ![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white) |
| [**nocccd-k16**](https://github.com/nocccd-data/nocccd-k16) | K-16 grant reporting automation — queries Oracle Banner (REPT) and builds a headless Excel pivot dashboard from scratch (no template, no Excel app) | ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![Oracle](https://img.shields.io/badge/Oracle-F80000?logo=oracle&logoColor=white) ![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white) ![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoftexcel&logoColor=white) |
| [**nocccd-nsc**](https://github.com/nocccd-data/nocccd-nsc) | National Student Clearinghouse (NSC) StudentTracker submission builder and return-file loader into the DWH warehouse schema | ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![Oracle](https://img.shields.io/badge/Oracle-F80000?logo=oracle&logoColor=white) ![Pandas](https://img.shields.io/badge/Pandas-150458?logo=pandas&logoColor=white) |
| [**nocccd-sql**](https://github.com/nocccd-data/nocccd-sql) | Ad-hoc SQL queries organized by campus (Cypress, Fullerton, NOCE, District) | ![SQL](https://img.shields.io/badge/SQL-4479A1?logo=amazondocumentdb&logoColor=white) ![Oracle](https://img.shields.io/badge/Oracle-F80000?logo=oracle&logoColor=white) |
| [**nocccd-streamlit**](https://github.com/nocccd-data/nocccd-streamlit) | Ad-hoc reporting dashboards for district use (Oracle / Tableau Cloud) | ![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white) ![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white) ![Tableau](https://img.shields.io/badge/Tableau-E97627?logo=tableau&logoColor=white) |
| [**nocccd-flexit-docker-deploy**](https://github.com/nocccd-data/nocccd-flexit-docker-deploy) | Docker deployment for daily production dbt build refreshes and scheduling, wrapped in the FlexIt web deployment kit | ![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white) ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?logo=postgresql&logoColor=white) ![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white) |

## Architecture

```mermaid
flowchart LR
    Banner[(Banner SIS)] --> DWHDB[(Oracle DWHDB)]
    Banner --> SQL["Ad-hoc SQL Queries"]
    DWHDB --> SQL

    subgraph EDW["Enterprise Data Warehouse"]
        dbt["dbt - EDW_PROD schema"]
    end

    Banner --> dbt
    dbt -.-> SQL
    FlexIt["FlexIt Docker Scheduler"] --> dbt

    DWHDB --> DWH["DWH schema mat. views/queries"]
    DWH --> SCFF["SCFF Analysis"]
    DWH --> Tableau["Tableau Cloud"]
    Tableau --> Streamlit["Streamlit Dashboards"]

    Banner --> K16["K-16 Excel Pivot Dashboard"]
    Banner --> NSC["NSC Submission Builder"]
    NSC --> NSCST["NSC StudentTracker"]
    NSCST --> DWH

    style Banner fill:#F80000,color:#fff
    style DWHDB fill:#F80000,color:#fff
    style dbt fill:#FF694B,color:#fff
    style Tableau fill:#E97627,color:#fff
    style Streamlit fill:#FF4B4B,color:#fff
    style FlexIt fill:#2496ED,color:#fff
    style K16 fill:#217346,color:#fff
    style NSC fill:#1F6FEB,color:#fff
```

## Tech Stack

- **Data Warehouse**: dbt + Oracle — source staging, transformations, dimensional models (dims & facts)
- **Dashboards**: Streamlit with Oracle and Tableau Cloud connectors
- **Analysis**: Python, Pandas, SQL
- **Reporting pipelines**: Python + Oracle — K-16 grant Excel dashboards (excelize, headless pivots) and NSC StudentTracker submission/return processing
- **Scheduling**: FlexIt Docker — daily production dbt refresh orchestration
- **Source System**: Ellucian Banner (Student Information System)