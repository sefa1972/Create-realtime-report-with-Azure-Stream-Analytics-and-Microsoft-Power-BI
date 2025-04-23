# Create-realtime-report-with-Azure-Stream-Analytics-and-Microsoft-Power-BI

This project demonstrates how to build a **real-time analytics solution** using **Azure Stream Analytics**, **Azure Event Hubs**, and **Power BI**. The scenario simulates an online retail platform sending continuous order data to Event Hubs, which is processed in real-time and visualized in a Power BI dashboard.

## Technologies Used

- Azure Stream Analytics
- Azure Event Hubs
- Azure Synapse Analytics (setup only)
- Microsoft Power BI
- Azure Cloud Shell (PowerShell)
- Git

---

## Scenario Overview

1. **Order data** is generated and pushed to **Azure Event Hubs**.
2. **Azure Stream Analytics** processes this data in real time.
3. Results are sent to **Power BI** for visualization in a live dashboard.

---

## Getting Started

### Prerequisites

- Azure subscription with admin access
- Microsoft Power BI (Pro or Trial account)
- Git and Node.js available in Azure Cloud Shell

---

### Azure Resources Setup
 - The script provisions the following:
 - Azure Synapse workspace
 - Azure Data Lake Storage
 - Azure Event Hub
 - Required resource group and identities

### Power BI Workspace
 - Go to Power BI
 - Create a new workspace (e.g., mslearn-streaming)
 - Copy the workspace GUID from the URL for later use

### Azure Stream Analytics Job
 - In Azure Portal, create a Stream Analytics Job
 - Set up the input from Azure Event Hub (alias: orders)
 - Set up the output to Power BI (alias: powerbi-dataset)

  Sample Query:

  SELECT
  
      DateAdd(second,-5,System.TimeStamp) AS StartTime,
      
      System.TimeStamp AS EndTime,
      
      ProductID,
      
      SUM(Quantity) AS Orders
      
  INTO
  
      [powerbi-dataset]
      
  FROM
  
      [orders] TIMESTAMP BY EventEnqueuedUtcTime
      
  GROUP BY ProductID, TumblingWindow(second, 5)
  
  HAVING COUNT(*) > 1

 - Save and start the job

### Simulate Streaming Orders
 Submit fake order data with:
 node ~/dp-203/Allfiles/labs/19/orderclient

### Visualize in Power BI
1- In Power BI, create a new Dashboard: Order Tracking
2- Add a custom streaming data tile:
 - Dataset: realtime-data
 - Visualization: Line Chart
 - Axis: EndTime
 - Value: Orders
 - Time window: 1 Minute

### Clean Up
- To avoid extra charges, delete the created resources after testing:
- Azure Resource Group (dp203-xxxxx)
- Power BI workspace (optional)

### Learning Objectives
- Ingest and process streaming data using Azure Stream Analytics
- Send real-time data to Power BI for dynamic visualization
- Integrate multiple Azure services for an end-to-end analytics solution

👤 Author
Sefa Öztürk

IT Trainee | Azure Data Engineer in progress

📇 LinkedIn: https://www.linkedin.com/in/sefa-ozturk1972

