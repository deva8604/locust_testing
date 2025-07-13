# 📘 Locust Load Testing Guide

✅ *Goal:*
Simulate users interacting with a sample REST API (reqres.in) and measure API performance under load.

✅ *Audience:*
QA Engineers, Developers, DevOps, or anyone learning performance testing.

---

## 🟢 1️⃣ Prerequisites

You need:

* *Python 3.x* installed
* *pip* (Python package manager)

✅ *Verify Python installation:*

Open terminal or command prompt:

 ```bash
 python --version
 ```

✅ Expected Output:


Python 3.x.x


If not installed, download here:
👉 [https://www.python.org/downloads/](https://www.python.org/downloads/)

---

## 🟢 2️⃣ Install Locust

In your terminal:

 ```bash
 pip install locust
 ```

✅ Verify installation:

```bash
 locust --version
 ```

Example output:


locust 2.x.x


---

## 🟢 3️⃣ Create Project Directory

Choose or create a folder to hold your test scripts:
 
 ```bash
 mkdir locust-demo
 cd locust-demo
 ```

---

## 🟢 4️⃣ Create locustfile.py

This script defines *user behavior*.

✅ **Create a file named locustfile.py:**

Example content:

```python
from locust import HttpUser, task, between

class ReqresUser(HttpUser):
    wait_time = between(1, 3)

    @task(2)
    def list_users(self):
        self.client.get("/api/users?page=2")

    @task(1)
    def create_user(self):
        self.client.post("/api/users", json={
            "name": "Devesh",
            "job": "QA Engineer"
        })

    @task(1)
    def get_single_user(self):
        self.client.get("/api/users/2")

    @task(1)
    def update_user(self):
        self.client.put("/api/users/2", json={
            "name": "Updated Reetesh",
            "job": "Senior QA"
        })
```

✅ *Explanation:*

* wait_time: each simulated user waits 1–3 seconds between tasks.
* @task: defines actions the user performs.

  * Higher numbers run more frequently.
* self.client: the HTTP session to make requests.

---

## 🟢 5️⃣ Start Locust

Run:

 ```bash
 locust
 ```

✅ Output Example:


[2024-XX-XX 10:00:00,000] Starting web interface at http://0.0.0.0:8089


---

## 🟢 6️⃣ Launch Web Interface

Open your browser:

```bash
http://localhost:8089
```

✅ You’ll see the Locust web UI.

---

## 🟢 7️⃣ Configure Test

Fill in the form:

| Field                 | Example Value          |
| --------------------- | ---------------------- |
| Number of total users | 10                   |
| Spawn rate            | 2 (users per second) |
| Host                  | https://reqres.in    |

✅ Click *Start Swarming*.

---

## 🟢 8️⃣ Monitor Load Test

While the test is running, the UI shows:

* Requests per second
* Response times (average, median, percentiles)
* Failures (if any)
* Charts

✅ You can *Stop* the test anytime.

----
## 🟢 1️⃣0️⃣ Example Output Snapshot

```bash
Name                             # reqs      # fails  |     Avg     Min     Max  Median  ...
GET /api/users?page=2              120     0(0.00%)     110      90     200    105
POST /api/users                     60     0(0.00%)     130     100     220    125
```


---

## 🟢 1️⃣1️⃣ How to Extend This Script

* ✅ *Add More Tasks:*

```bash
@task
def delete_user(self):
    self.client.delete("/api/users/2")
```

* ✅ *Validate Responses:*

```bash
@task
def get_user(self):
    with self.client.get("/api/users/2", catch_response=True) as response:
        if response.status_code != 200:
            response.failure("Unexpected status code!")
```

* ✅ *Add Authentication (Bearer Token):*

```bash
def on_start(self):
    self.client.headers.update({
        "Authorization": "Bearer YOUR_TOKEN_HERE"
    })
```

* ✅ *Change Wait Times:*

```python
wait_time = between(0.5, 2)
```

---

## 🟢 1️⃣2️⃣ Common Tips

* ✅ *Tip 1:* Always start small (5–10 users) to verify everything works.

* ✅ *Tip 2:* Watch failure % carefully—if many requests fail, inspect API limits.

* ✅ *Tip 3:* Use --run-time to control duration.

* ✅ *Tip 4:* If you need more concurrency, Locust supports *distributed load generation* (multiple machines).

* ✅ *Tip 5:* You can export reports in CSV for analysis.

---

## 🟢 1️⃣3️⃣ Useful Commands Reference

| Command                                                 | Description                  |
| ------------------------------------------------------- | ---------------------------- |
| locust                                                | Start Locust with web UI     |
| locust -f locustfile.py                               | Start Locust specifying file |
| locust --headless -u 10 -r 2 --host=https://reqres.in | Run headless mode            |
| locust --help                                         | See all options              |

---

## 🟢 1️⃣4️⃣ Resources

* 🌐 *Locust Documentation:*
  [https://docs.locust.io/](https://docs.locust.io/)
* 🌐 *reqres.in API Reference:*
  [https://reqres.in/](https://reqres.in/)
* 🌐 *Locust GitHub Repository:*
  [https://github.com/locustio/locust](https://github.com/locustio/locust)

---

* ✅ *You’re Ready!*

With this guide, you can:
* ✅ Install Locust
* ✅ Write your own test scripts
* ✅ Run and monitor load tests
* ✅ Extend tests as needed
