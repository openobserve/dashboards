# **Airflow Uptime and Performance Monitoring Dashboard**

This repository provides a JSON file for a detailed **Airflow uptime and performance monitoring dashboard** in **OpenObserve**. Importing this dashboard gives you a **real-time overview** of Airflow's **DAG execution, scheduler health, executor performance, and task reliability** to ensure efficient workflow orchestration.  

## **Dashboard Features**  

The JSON file includes panels that track key metrics such as:  

- **DAG Execution Metrics** – Queue time, and run duration  
- **Scheduler Health** – Heartbeats, loop duration, orphaned tasks  
- **Task Execution** – Running, queued, and failed tasks  
- **Executor Performance** – Open slots, queued tasks, and parallel execution analysis  
- **Pool Utilization** – Open, queued, and running slots in Airflow pools  
- **Latency Trends** – Task execution delays and scheduling efficiency  
- **Historical Uptime and Performance Trends** – Long-term insights into system health  

### **Preview:**  
![airflow_dashboard](./screenshots/dashboard.gif)  

### **How to Use:**  
1. Import the **JSON dashboard file** into OpenObserve  
2. Configure it to visualize your **Airflow monitoring data**  
3. Get **real-time insights** into Airflow's health and performance  