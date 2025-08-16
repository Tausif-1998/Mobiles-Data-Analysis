# 📊 Mobile Data Analysis
Data source: MS SharePoint Folder

## 📌 Project Overview
The **Mobile Data Analysis** project provides insights into mobile device specifications and pricing trends using CSV datasets stored in **SharePoint** and visualized in **Power BI**.  
The model is designed to handle new incoming files automatically through scheduled refresh in the Power BI Service.

---

## ⚙️ Tech Stack
- **MS SharePoint Folder** – centralized storage of CSV files  
- **Power BI Desktop & Service** – data modeling and dashboarding  
- **Power Query (M language)** – for data import & transformations  
- **DAX (Data Analysis Expressions)** – for calculations and summarization  

---

## 📂 Data Source
- Location:  
```
[https://powerappptest.sharepoint.com/sites/MobileDataAnalysis/Shared](https://powerappptest.sharepoint.com/sites/MobileDataAnalysis/Shared) Documents/Mobile Data - Sharepoint/
```
- At the time of project creation, **two CSV files** were available.  
- Files were imported using **Combine Files** in Power Query.  
- Any **new CSV files** arriving in this folder are automatically included in the dataset when the report is refreshed (manual or scheduled refresh in Power BI Service).

---

## 🔌 Steps: Connect Power BI to SharePoint Folder (Multiple CSVs)

1. **Copy the SharePoint Site URL**  
 - From your folder link, copy only the **site root** (not the entire file path).  
 - Example:  
   ```
   https://powerappptest.sharepoint.com/sites/MobileDataAnalysis
   ```

2. **Open Power BI Desktop → Get Data**  
 - Go to **Home > Get Data > More… > SharePoint Folder**.  
 - Paste the site URL and click **OK**.  
 - Sign in with your **organization credentials**.

3. **Filter the Files**  
 - Power BI will show all files in the site.  
 - In **Navigator / Power Query Editor**, filter:  
   - `Folder Path` → keep only your folder:  
     `/Shared Documents/Mobile Data - Sharepoint/`  
   - `Extension` → only `.csv` files.

4. **Combine Files**  
 - Click **Combine Files**.  
 - Power BI will automatically create a function to import all CSVs with the same structure into a single table.

5. **Apply Transformations**  
 - Use Power Query to clean columns (e.g., remove “GB” from RAM).  
 - Ensure headers and datatypes are correct.  
 - Close & Load.

6. **Publish & Enable Refresh**  
 - Publish the `.pbix` file to **Power BI Service**.  
 - In Service, go to **Settings → Datasets**:  
   - Update credentials for SharePoint Online.  
   - Turn on **Scheduled Refresh**.  
 - Now, any **new CSV file** added to the folder will automatically be included after refresh.

---

## 🧮 DAX Implementation

### 🔹 New Table
```DAX
New Table = 
SUMMARIZE(
  'Mobile Dataset',
  'Mobile Dataset'[Company Name],
  'Mobile Dataset'[Model Name],
  "Average Wt", AVERAGE('Mobile Dataset'[Numerical wt]),
  "Average Price USD", AVERAGE('Mobile Dataset'[Launch Price (USD)]),
  "Average RAM", AVERAGE('Mobile Dataset'[RAM (GB)])
)
````

### 🔹 Concatenated Column

```
Concatenated column =
CONCATENATE('Mobile Dataset'[Company Name], 'Mobile Dataset'[Model Name])
```

### 🔹 Length Column

```
length = LEN('Mobile Dataset'[Mobile Weight])
```

### 🔹 Company & Model Combined

```
Mobile Device =
CONCATENATE(CONCATENATE('Mobile Dataset'[Company Name], " - "), 'Mobile Dataset'[Model Name])
```

### 🔹 Numerical Weight Extraction

```
Numerical wt =
IF(
    'Mobile Dataset'[length] = 4,
    LEFT('Mobile Dataset'[Mobile Weight], 3),
    LEFT('Mobile Dataset'[Mobile Weight], 5)
)
```

---

## 📊 Dashboard

The Power BI dashboard includes:

* **Mobile Specs Overview**: RAM, Weight, Launch Price
* **Aggregated Insights**: Average Price, RAM, and Weight per company/model
* **Interactive Visuals**: Filters by brand, model, and specs

🔗 [**View the Dashboard in Power BI Service**](https://app.powerbi.com/reportEmbed?reportId=72fd2558-dcdf-4b9e-b788-6918281b7ea9&autoAuth=true&ctid=52a7ae34-9624-43da-928e-df112e7e6ac4)

📹 *(Attached project demo video included in repo: `Sharepoint - Mobile Data Analysis.mp4`)*

---

## 🚀 How to Use

1. Clone/download this repository.
2. Open the `.pbix` file in **Power BI Desktop**.
3. Update your SharePoint credentials to access the folder.
4. Refresh data to pull the latest CSV files.
5. Publish to **Power BI Service** to enable **scheduled refresh**.

---

## 🤝 Contribution

Contributions are welcome! Please open an issue or submit a pull request if you’d like to enhance the project.



