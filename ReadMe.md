# 📌 Apache Airflow - Local Installation Guide

## 🚀 Overview
Apache Airflow is an open-source workflow orchestration tool for scheduling and monitoring data pipelines. This guide will help you **install and run Apache Airflow locally** using Docker and WSL2.

---

## 🛠️ Prerequisites
Ensure you have the following installed on your system:

- **Windows 10/11** (with WSL2 enabled)
- **Ubuntu (via WSL2)**
- **Docker**

---

## ⚙️ Installation Steps

### 1️⃣ Enable WSL2
Open **PowerShell as Administrator** and run:
```powershell
wsl --install
```
Restart your computer if prompted.

### 2️⃣ Install Docker in WSL2 (Ubuntu)
Run the following inside **Ubuntu**:
```bash
sudo apt update && sudo apt upgrade -y
```
```bash
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
```bash
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
```
```bash
sudo usermod -aG docker $USER
newgrp docker
```
Verify Docker installation:
```bash
docker --version
```

---

## 📦 Set Up Apache Airflow

### 3️⃣ Create an Airflow Project Folder
```bash
mkdir ~/airflow && cd ~/airflow
```

### 4️⃣ Download the Airflow Docker Configuration
```bash
curl -LfO 'https://airflow.apache.org/docs/apache-airflow/stable/docker-compose.yaml'
```

### 5️⃣ Set Up Environment Variables
```bash
echo -e "AIRFLOW_UID=$(id -u)" > .env
```

### 6️⃣ Initialize Airflow
```bash
docker compose up airflow-init
```

### 7️⃣ Start Airflow
```bash
docker compose up -d
```

✅ **Airflow is now running!** Open your browser and go to:
👉 `http://localhost:8080`

Login credentials:
- **Username:** `airflow`
- **Password:** `airflow`

---

## 🏗️ First Hands-On: Creating a Simple DAG

### 1️⃣ Open the Airflow DAGs Folder
```bash
cd ~/airflow/dags
```

### 2️⃣ Create a New Python DAG File
```bash
nano my_first_dag.py
```

### 3️⃣ Add a Simple DAG
```python
from airflow import DAG
from airflow.operators.dummy_operator import DummyOperator
from datetime import datetime

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2024, 1, 1),
    'retries': 1
}

dag = DAG(
    'my_first_dag',
    default_args=default_args,
    schedule_interval='@daily'
)

start = DummyOperator(task_id='start', dag=dag)
end = DummyOperator(task_id='end', dag=dag)

start >> end
```

### 4️⃣ Save and Restart Airflow
```bash
docker compose restart
```

### 5️⃣ Open the Airflow Web UI and Enable the DAG
Go to **`http://localhost:8080`** → Find `my_first_dag` → Enable it!

---

## 🛑 Stopping Airflow
To stop all running containers:
```bash
docker compose down
```
To start again:
```bash
docker compose up -d
```

---

## 🎯 Next Steps
✅ Learn how to schedule tasks in Airflow.
✅ Connect Airflow with **Snowflake** or **PostgreSQL**.
✅ Explore real-world **ETL workflows**.

---

## 📌 Resources
- [Apache Airflow Docs](https://airflow.apache.org/)
- [Docker Installation Guide](https://docs.docker.com/get-docker/)

🚀 Happy coding! 🎉
