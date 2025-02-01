# How to Start Docker and Airflow

## **1️⃣ Start Docker**
1. Click on the **Start Menu** and search for **Docker Desktop**.
2. Click **Open** and wait for Docker to start.

## **2️⃣ Run Airflow Using Docker**
1. Open **PowerShell** or **Command Prompt**.
2. Move to the folder where Airflow is configured:
   ```sh
   cd C:\airflow-docker
   ```
3. Start Airflow containers:
   ```sh
   docker-compose up -d
   ```
4. Access the Airflow webserver:
   ```sh
   docker-compose exec airflow-webserver bash
   ```
5. Open the URL to access the Airflow UI:
   ```
   http://localhost:8080/home
   ```

## **Next Steps in Your Airflow Learning Journey 🚀**

### **1️⃣ Understand the Basics of Airflow**
- Learn about **DAGs (Directed Acyclic Graphs)** – these define workflows.
- Study **Tasks & Operators** – different types of tasks you can run (Python, Bash, SQL, etc.).
- Explore **Scheduling & Triggers** – how Airflow schedules and executes tasks.
- Understand **Task Dependencies** – controlling task execution order.

📌 **Suggested Actions:**
- Read the official **Airflow documentation**: [https://airflow.apache.org/](https://airflow.apache.org/)
- Explore the **Airflow UI** (at `http://localhost:8080`) to see how DAGs are visualized.

---

### **2️⃣ Create Your First Custom DAG (Workflow)**
- Write a simple DAG in Python that runs a task (e.g., printing "Hello Airflow").
- Use **BashOperator** or **PythonOperator** to execute simple tasks.
- Test your DAG by placing it in the `dags/` folder of your Airflow setup.

📌 **Example DAG:**
```python
from airflow import DAG
from airflow.operators.python import PythonOperator
from datetime import datetime

def hello():
    print("Hello, Airflow!")

default_args = {
    'owner': 'airflow',
    'start_date': datetime(2024, 1, 1),
}

with DAG('hello_airflow', default_args=default_args, schedule_interval=None) as dag:
    task1 = PythonOperator(
        task_id='say_hello',
        python_callable=hello
    )

task1
```
- Restart Airflow: `docker-compose restart airflow-webserver`.
- Check if your DAG appears in the UI under **DAGs**.

---

### **3️⃣ Work with Task Dependencies**
- Learn how to **set dependencies** between tasks using `.set_upstream()` and `.set_downstream()`.
- Understand **parallel vs sequential execution** of tasks.

📌 **Example DAG with dependencies:**
```python
from airflow import DAG
from airflow.operators.bash import BashOperator
from datetime import datetime

default_args = {'owner': 'airflow', 'start_date': datetime(2024, 1, 1)}

with DAG('task_dependencies', default_args=default_args, schedule_interval=None) as dag:
    task1 = BashOperator(task_id='task1', bash_command='echo "Task 1 Done!"')
    task2 = BashOperator(task_id='task2', bash_command='echo "Task 2 Done!"')
    task3 = BashOperator(task_id='task3', bash_command='echo "Task 3 Done!"')

task1 >> [task2, task3]  # task1 runs first, then task2 and task3 run in parallel
```
- Check the **Graph View** in the Airflow UI.

---

### **4️⃣ Learn About Airflow Connections & Integrations**
- Understand how Airflow connects to **databases, APIs, and cloud services**.
- Explore **Airflow Connections** (PostgreSQL, MySQL, Snowflake, AWS, etc.).
- Learn about **XComs** (data sharing between tasks).

📌 **Example PostgreSQL task:**
```python
from airflow.providers.postgres.operators.postgres import PostgresOperator

task4 = PostgresOperator(
    task_id='create_table',
    postgres_conn_id='my_postgres_conn',
    sql='CREATE TABLE IF NOT EXISTS test (id SERIAL PRIMARY KEY, name TEXT);'
)
```
- Check the **Admin → Connections** tab in the Airflow UI.

---

### **5️⃣ Deploy a Real-World Use Case**
- Automate a simple **ETL workflow** (Extract, Transform, Load).
- Schedule it to run at specific intervals.
- Monitor DAG execution and logs.

📌 **Suggested Actions:**
- Create a DAG that:
  - Fetches data from an API.
  - Processes it with Python (transforms).
  - Stores it in a database (loads).

---

### **Final Thoughts: Your Roadmap for Mastery**
📌 **Beginner:** Learn DAGs, tasks, and dependencies.
📌 **Intermediate:** Work with connections, databases, APIs, and XComs.
📌 **Advanced:** Develop complex workflows, integrate with Big Data tools (Snowflake, Spark, etc.), and deploy in production.

🚀 **You're on the right track!** Start with the basics, practice with small projects, and gradually move toward more complex workflows.

