# 🏍️ Two Wheeler Sales Analysis

An end-to-end data analytics project that cleans a raw two-wheeler sales dataset using **MongoDB** and visualizes the results in an interactive **Power BI** dashboard.

---

## 📌 Table of Contents

- [Project Overview](#-project-overview)
- [Objectives](#-objectives)
- [Tech Stack](#-tech-stack)
- [Project Workflow](#-project-workflow)
- [Repository Structure](#-repository-structure)
- [Data Cleaning (MongoDB)](#-data-cleaning-mongodb)
- [Power BI Dashboard](#-power-bi-dashboard)
- [Key Insights](#-key-insights)
- [How to Reproduce](#-how-to-reproduce)
- [Future Improvements](#-future-improvements)
- [Author](#-author)

---

## 📖 Project Overview

Raw sales data is rarely analysis-ready. This project starts with an **unclean CSV dataset** of two-wheeler sales, which is imported into **MongoDB**, cleaned using MongoDB queries, exported back to CSV, and finally loaded into **Power BI** to build a dashboard that surfaces sales trends and business insights.

---

## 🎯 Objectives

- Import a messy, unclean CSV dataset into MongoDB
- Clean and standardize the data using MongoDB queries (handling duplicates, missing values, inconsistent formats, incorrect data types, etc.)
- Export the cleaned dataset back to CSV
- Build an interactive Power BI dashboard to analyze two-wheeler sales performance

---

## 🛠️ Tech Stack

| Tool | Purpose |
|------|---------|
| **CSV** | Raw and cleaned data format |
| **MongoDB** | Data import and cleaning using queries |
| **MongoDB Compass / Shell** | Running queries and exporting data |
| **Power BI** | Data modeling and dashboard creation |

---

## 🔄 Project Workflow

```
Unclean CSV Dataset
        │
        ▼
Import into MongoDB
        │
        ▼
Data Cleaning using Mongo Queries
        │
        ▼
Export Cleaned Data to CSV
        │
        ▼
Import into Power BI
        │
        ▼
Interactive Dashboard
```





## 🧹 Data Cleaning (MongoDB)

The raw CSV was imported into a MongoDB collection and cleaned entirely using MongoDB queries.

👉 **All cleaning queries are documented here:** [`queries/mongo_queries.md`](queries/mongo_queries.md)

### Typical cleaning steps performed

- ✅ Removed duplicate records
- ✅ Handled null / missing values
- ✅ Standardized text fields (casing, spacing, spelling)
- ✅ Converted fields to correct data types (numbers, dates)
- ✅ Fixed inconsistent or invalid entries
- ✅ Removed irrelevant or corrupted records

> 📝 *Edit this list to reflect exactly what you did in your queries.*

### Importing the data

```bash
mongoimport --db TwoWheelerDB --collection sales --type csv --headerline --file raw_data.csv
```

### Exporting the cleaned data

```bash
mongoexport --db TwoWheelerDB --collection sales --type=csv --fields <field1,field2,...> --out cleaned_data.csv
```

---

## 📊 Power BI Dashboard

The cleaned CSV was imported into Power BI, where the data was modeled and visualized.

### Dashboard Preview

![alt text](dashboard.png)



### Dashboard Features

- 📈 Sales trends over time
- 🏭 Sales by brand / manufacturer
- 🏍️ Sales by model / vehicle type
- 🌍 Region-wise performance
- 🔍 Interactive slicers and filters
- 🔢 Key KPI cards (total sales, units sold, revenue, etc.)

> 📝 *Adjust these to match the visuals in your dashboard.*

---

## 💡 Key Insights

- *Insight 1 – e.g., Top-selling brand and its share of total sales*
- *Insight 2 – e.g., Peak sales months or seasonal trends*
- *Insight 3 – e.g., Best-performing region or segment*
- *Insight 4 – e.g., Price range with the highest demand*

> 📝 *Replace these placeholders with the real findings from your dashboard.*

---

## ▶️ How to Reproduce

1. **Clone the repository**
   ```bash
   git clone https://github.com/<your-username>/<your-repo>.git
   cd <your-repo>
   ```
2. **Import the raw dataset into MongoDB** using `mongoimport` or MongoDB Compass.
3. **Run the cleaning queries** from [`queries/mongo_queries.md`](queries/mongo_queries.md).
4. **Export the cleaned collection** to CSV.
5. **Open Power BI Desktop** → *Get Data* → *Text/CSV* → select the cleaned CSV.
6. **Open the `.pbix` file** to explore the dashboard, or rebuild the visuals yourself.

---

## 🚀 Future Improvements

- Automate the cleaning pipeline with a script (Python / MongoDB aggregation pipelines)
- Connect Power BI directly to MongoDB
- Add forecasting and predictive analysis
- Publish the dashboard to Power BI Service

---

## 👨‍💻 Author

**Subhajit Chatterjee**  
**Data Analyst** at Farmershop Tech India Pvt. Ltd.

- **Role:** Data Analyst
- **Company:** Farmershop Tech India Pvt. Ltd.
- **Skills:** Google Sheets, SQL, Power BI, Data Cleaning, Data Visualization, and Business Intelligence

This project demonstrates my ability to clean and analyze hotel booking data, develop meaningful KPIs, and build interactive Power BI dashboards to support data-driven business decisions.

---

⭐ *If you found this project useful, consider giving it a star!*
