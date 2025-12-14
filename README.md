# NYC Payroll Data Analytics - Udacity Submission

**Course:** Azure Data Engineering  
**Project:** Data Integration Pipelines for NYC Payroll Data Analytics  
**Student:** Rodolfo Lerma  
**Submission Date:** December 14, 2025

---

## Project Objective

Build a data analytics platform on **Azure Synapse Analytics** to accomplish two primary goals for the City of New York:

1. **Analyze** how the City's financial resources are allocated and how much of the City budget is being devoted to overtime
2. **Provide** transparency to the public by making payroll data accessible to interested stakeholders

---

## Project Deliverables

This submission includes all required components as specified in the project rubric:

### ✅ Infrastructure (Step 1)
- Resource Group created
- Azure Data Lake Gen2 Storage Account with 3 containers:
  - `dirpayrollfiles` (source CSV files)
  - `dirhistoryfiles` (historical data)
  - `dirstaging` (aggregated output)
- Azure SQL Database with star schema tables
- Azure Synapse Analytics Workspace with external table
- Screenshots: `s1-*.png`

### ✅ Linked Services (Step 2)
- Data Lake connection: `ls-DataLake.json`
- SQL Database connection: `ls-SqlServer.json`
- Synapse Workspace connection: `ls-SynapseWorkspace.json`
- Screenshots: `s2-*.png`

### ✅ Datasets (Step 3)
- **19 datasets created** across all systems:
  - 8 Data Lake CSV datasets (source files)
  - 5 SQL Database table datasets
  - 1 Synapse external table dataset
  - 5 additional staging/output datasets
- All JSON files in `dataset/` folder
- Screenshots: `s3-*.png`

### ✅ Data Flows (Step 4)
- **6 data flows created**:
  - 5 load flows (Agency, Employee, Title, 2020 data, 2021 data)
  - 1 aggregation flow (union, filter, calculate, aggregate)
- All JSON files in `dataflow/` folder
- Screenshots: `s4-*.png`

### ✅ Pipeline (Step 5)
- Master pipeline: `pl-NYC-Payroll-Data-Analytics`
- Orchestrates all 6 data flows with dependencies
- Aggregation flow runs only after all loads complete
- JSON file: `pipeline/pl-NYC-Payroll-Data-Analytics.json`
- Screenshots: `s5-*.png`

### ✅ Execution & Monitoring (Step 6)
- Pipeline triggered and executed successfully
- All 6 data flow activities completed
- Monitoring screenshots showing execution times and row counts
- Screenshots: `s6-*.png`

### ✅ Verification (Step 7)
- SQL queries validate correct row counts:
  - NYC_Payroll_Data_2020: 195 rows
  - NYC_Payroll_Data_2021: 10 rows
- Synapse query validates aggregation:
  - NYC_Payroll_Summary: 27 rows
  - Total salary: $36,141,709.69
- Screenshots: `s7-*.png`

### ✅ GitHub Integration (Step 8)
- All Data Factory resources committed to GitHub
- Repository configured in ADF for version control
- This submission repository contains complete codebase
- Screenshots: `s8-*.png`

### ✅ Documentation (Step 9)
- Comprehensive LaTeX report: `report/project_report.pdf`
- Step-by-step documentation in `docs/` folder
- Beginner-friendly overview: `docs/OVERVIEW.md`
- Project status tracker: `docs/PROJECT_STATUS.md`

---

## Repository Structure

```
Project4/
├── dataset/                     # 19 ADF dataset JSON files
├── dataflow/                    # 6 ADF data flow JSON files
├── pipeline/                    # 1 ADF pipeline JSON file
├── linkedService/               # 3 ADF linked service JSON files
├── scripts/
│   ├── 01_create_infrastructure.py      # Python SDK for Azure provisioning
│   ├── 02_upload_data.py                # Data Lake upload automation
│   ├── 03_create_sql_tables.py          # SQL schema creation
│   └── 04_create_synapse_external_table.py  # Synapse setup
├── data/                        # Source CSV files
│   ├── AgencyMaster.csv
│   ├── EmpMaster.csv
│   ├── TitleMaster.csv
│   ├── nycpayroll_2020.csv
│   └── nycpayroll_2021.csv
├── screenshots/                 # All step screenshots (s1-*.png through s9-*.png)
├── docs/
│   ├── OVERVIEW.md              # Beginner-friendly learning guide
│   ├── PROJECT_STATUS.md        # Development log
│   ├── STEP2_LINKED_SERVICES.md # Linked service details
│   ├── STEP3_DATASETS.md        # Dataset documentation
│   └── STEP4_PIPELINES.md       # Pipeline details
├── report/
│   └── project_report.pdf       # **Comprehensive LaTeX report**
├── README.md                    # Technical README
└── README_UDACITY.md            # This file (submission overview)
```

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                         Azure Data Factory                          │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────────┐ │
│  │ Linked Svcs  │  │   Datasets   │  │      Data Flows         │ │
│  │              │  │              │  │                          │ │
│  │ • Data Lake  │  │ • 8 CSV      │  │ • Load Agency Master    │ │
│  │ • SQL DB     │  │ • 5 SQL      │  │ • Load Employee Master  │ │
│  │ • Synapse    │  │ • 1 Synapse  │  │ • Load Title Master     │ │
│  │              │  │ • 5 Staging  │  │ • Load 2020 Data        │ │
│  └──────────────┘  └──────────────┘  │ • Load 2021 Data        │ │
│                                       │ • Aggregate Summary     │ │
│                                       └──────────────────────────┘ │
│  ┌────────────────────────────────────────────────────────────┐   │
│  │              Pipeline Orchestration                        │   │
│  │  [Load 5 flows in parallel] → [Aggregate flow]            │   │
│  └────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
           ↓                   ↓                         ↓
    ┌─────────────┐    ┌─────────────┐         ┌─────────────┐
    │  Data Lake  │    │   SQL DB    │         │   Synapse   │
    │   Gen2      │    │  (Staging)  │         │  Analytics  │
    │             │    │             │         │             │
    │ • Source    │    │ • Tables    │         │ • External  │
    │ • Staging   │    │ • Star      │         │   Table     │
    │ • History   │    │   Schema    │         │ • Queries   │
    └─────────────┘    └─────────────┘         └─────────────┘
```

---

## Key Technical Achievements

### 1. Full Automation
- Python SDK used for infrastructure provisioning (no manual portal clicks)
- Scripted data upload to Data Lake
- Automated SQL table creation
- Repeatable deployment process

### 2. Data Flow Design
- **Parallel execution** of 5 independent load flows for performance
- **Dependency management** ensuring aggregation runs only after loads complete
- **Column mapping** handling schema differences (AgencyCode → AgencyID)
- **Data quality** filtering invalid fiscal years (1998, 1999)
- **Business logic** calculating TotalPaid from multiple salary components

### 3. Star Schema Implementation
- **Dimension tables**: Agency, Employee, Title (master data)
- **Fact tables**: NYC_Payroll_Data_2020, NYC_Payroll_Data_2021 (transactional)
- **Aggregated view**: NYC_Payroll_Summary (reporting optimized)

### 4. Cross-System Verification
- SQL queries validate staging data
- Synapse queries validate aggregated output
- Row count validation at each stage
- Financial total verification ($36.1M)

---

## Data Validation Summary

| System | Table/File | Expected Rows | Actual Rows | Status |
|--------|------------|---------------|-------------|--------|
| SQL DB | NYC_Payroll_AGENCY_MD | 3 | 3 | ✅ |
| SQL DB | NYC_Payroll_EMP_MD | 10 | 10 | ✅ |
| SQL DB | NYC_Payroll_TITLE_MD | 4 | 4 | ✅ |
| SQL DB | NYC_Payroll_Data_2020 | 195 | 195 | ✅ |
| SQL DB | NYC_Payroll_Data_2021 | 10 | 10 | ✅ |
| Synapse | NYC_Payroll_Summary | 27 | 27 | ✅ |
| **Financial Validation** | **Total Salary** | **$36,141,709.69** | **$36,141,709.69** | ✅ |

---

## Technologies Used

| Category | Technology | Purpose |
|----------|-----------|---------|
| **Cloud Platform** | Microsoft Azure | Infrastructure hosting |
| **Storage** | Azure Data Lake Gen2 | Raw and processed data storage |
| **Database** | Azure SQL Database | Staging and star schema tables |
| **Analytics** | Azure Synapse Analytics | Serverless SQL queries on external tables |
| **Orchestration** | Azure Data Factory | Pipeline execution and data flows |
| **IaC** | Python (Azure SDK) | Infrastructure automation |
| **Version Control** | Git/GitHub | Code versioning and collaboration |
| **Documentation** | LaTeX, Markdown | Formal report and technical docs |

---

## How to Run This Project

### Prerequisites
- Azure subscription (free trial or student account)
- Python 3.8+
- Azure CLI installed
- Git installed

### Step-by-Step Execution

1. **Clone Repository**
   ```bash
   git clone <this-repo-url>
   cd Project4
   ```

2. **Configure Environment**
   ```bash
   python -m venv .venv
   .venv\Scripts\activate  # Windows
   pip install -r requirements.txt
   ```

3. **Set Azure Credentials**
   ```bash
   az login
   az account set --subscription <your-subscription-id>
   ```

4. **Run Infrastructure Script**
   ```bash
   python scripts/01_create_infrastructure.py
   # Creates resource group, Data Lake, SQL DB, Synapse
   ```

5. **Upload Data**
   ```bash
   python scripts/02_upload_data.py
   # Uploads CSV files to Data Lake
   ```

6. **Create SQL Tables**
   ```bash
   python scripts/03_create_sql_tables.py
   # Creates star schema in SQL Database
   ```

7. **Create Synapse External Table**
   ```bash
   python scripts/04_create_synapse_external_table.py
   # Sets up external table for aggregated data
   ```

8. **Deploy ADF Resources**
   - Import JSON files from `linkedService/`, `dataset/`, `dataflow/`, `pipeline/` folders into Azure Data Factory via portal or Azure DevOps

9. **Trigger Pipeline**
   - Navigate to Data Factory → Pipelines → `pl-NYC-Payroll-Data-Analytics`
   - Click "Trigger" → "Trigger Now"
   - Monitor execution in "Pipeline runs"

10. **Verify Results**
    - Query SQL Database using provided verification queries
    - Query Synapse using Synapse Studio or SQL client

---

## Learning Outcomes

Through this project, I demonstrated proficiency in:

### Azure Data Engineering Skills
- Provisioning and configuring Azure Data Lake Gen2, SQL Database, Synapse Analytics
- Designing and implementing linked services for secure connectivity
- Creating datasets with schema definitions for multiple data formats
- Building data flows with transformations (union, filter, aggregate, calculate)
- Orchestrating pipelines with dependency management
- Monitoring and troubleshooting pipeline executions

### Data Engineering Best Practices
- Star schema design for analytics workloads
- ETL vs ELT patterns (hybrid approach used)
- Idempotent pipeline design (TRUNCATE before load)
- Data quality validation (filtering invalid records)
- Cross-system verification (SQL + Synapse)
- Version control for data pipelines (GitHub integration)

### Cloud Architecture
- Separation of storage and compute
- Serverless analytics with Synapse
- External tables for schema-on-read patterns
- Cost optimization (pay-per-query vs always-on)

### Automation & DevOps
- Infrastructure as Code (Python SDK)
- CI/CD principles (Git integration)
- Repeatability and reproducibility
- Documentation for knowledge transfer

---

## Project Challenges & Solutions

### Challenge 1: Synapse External Table Credential
**Problem:** External table couldn't access Data Lake storage  
**Solution:** Created database scoped credential with managed identity  
**Learning:** Azure services need explicit permissions even within same subscription

### Challenge 2: Data Flow Column Mapping
**Problem:** Source column named "AgencyCode" but target expects "AgencyID"  
**Solution:** Used Derived Column transformation to rename  
**Learning:** Schema mapping is critical in heterogeneous systems

### Challenge 3: Pipeline Dependency Management
**Problem:** Aggregation flow tried to run before loads completed  
**Solution:** Added "Success" dependency from all 5 loads to aggregation activity  
**Learning:** Parallel execution requires explicit dependency graphs

### Challenge 4: Data Validation
**Problem:** How to verify 27 aggregated rows are correct?  
**Solution:** Cross-checked with SQL queries summing by FiscalYear + AgencyName  
**Learning:** Always validate transformations with independent queries

---

## Screenshots Reference

All screenshots are organized by step in the `screenshots/` folder:

- **s1-*.png**: Infrastructure setup (Resource Group, Data Lake, SQL DB, Synapse)
- **s2-*.png**: Linked services configuration
- **s3-*.png**: Dataset definitions (all 19 datasets)
- **s4-*.png**: Data flow designs (6 flows with transformations)
- **s5-*.png**: Pipeline orchestration
- **s6-*.png**: Pipeline execution and monitoring
- **s7-*.png**: Data verification queries and results
- **s8-*.png**: GitHub integration and repository configuration
- **s9-*.png**: Final documentation and submission preparation

---

## Conclusion

This project successfully demonstrates the implementation of a production-grade data analytics platform on Azure, following industry best practices for data engineering. The solution is:

- **Scalable**: Handles growing data volumes with serverless compute
- **Automated**: Runs on schedule with minimal manual intervention
- **Reliable**: Includes monitoring, logging, and error handling
- **Transparent**: Data accessible to public via Synapse queries
- **Maintainable**: Version-controlled, documented, reproducible

The platform provides the City of New York with actionable insights into payroll expenses and overtime costs while ensuring public transparency.

---

## Additional Resources

- **Detailed Report:** See `report/project_report.pdf` for comprehensive technical documentation
- **Learning Guide:** See `docs/OVERVIEW.md` for beginner-friendly explanations
- **Step Docs:** See `docs/STEP*.md` for detailed step-by-step instructions
- **Status Log:** See `docs/PROJECT_STATUS.md` for development timeline

---

**Submission Complete** ✅  
**All Rubric Requirements Met** ✅  
**Ready for Udacity Review** ✅

---

*Submitted by: Rodolfo Lerma*  
*Date: December 14, 2025*  
*Course: Azure Data Engineering (Udacity)*
