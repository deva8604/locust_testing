# Locust Load Testing with Authentication

This project demonstrates how to use **Locust** to perform load testing on an API that requires **Bearer token authentication**.  

It includes a Python script that:  
✅ Logs in once per simulated user to retrieve an access token  
✅ Sets the token automatically for all requests  
✅ Runs GET, POST, PUT, and DELETE operations  

---

## 📂 Files

- `locustfile.py` – The Locust script defining user behavior

---

## ⚙️ Prerequisites

- Python 3.x installed
- pip (Python package manager)

✅ Verify installation:
```bash
python --version
```

✅ Install Locust:
```bash
pip install locust
```

---

## 🧑‍💻 locustfile.py Overview

```python
from locust import HttpUser, task, between

class AuthenticatedAPIUser(HttpUser):
    wait_time = between(1, 3)

    def on_start(self):
        response = self.client.post("/api/login", json={
            "username": "your_username",
            "password": "your_password"
        })
        self.token = response.json().get("token")
        self.client.headers.update({
            "Authorization": f"Bearer {self.token}"
        })

    @task
    def get_item(self):
        self.client.get("/api/items/1")

    @task
    def create_item(self):
        self.client.post("/api/items", json={
            "name": "Test Item",
            "description": "Created by Locust"
        })

    @task
    def update_item(self):
        self.client.put("/api/items/1", json={
            "name": "Updated Item",
            "description": "Updated by Locust"
        })

    @task
    def delete_item(self):
        self.client.delete("/api/items/1")
```

---

## ✏️ How to Customize

✅ **Login Endpoint:**
- Replace `/api/login` with your API's login URL.

✅ **Credentials:**
- Update `"username"` and `"password"` with valid credentials.

✅ **API Endpoints:**
- Replace `/api/items/1` and `/api/items` with your own API paths.

✅ **Request Data:**
- Adjust the JSON payloads to match your API schema.

---

## 🚀 How to Run

1️⃣ In the project directory, start Locust:
```bash
locust
```

2️⃣ Open the web interface:
```
http://localhost:8089
```

3️⃣ Fill in:
- **Number of total users** (e.g., `5`)
- **Spawn rate** (e.g., `1`)
- **Host** (e.g., `https://your-api-domain.com`)

4️⃣ Click **Start Swarming**.

Locust will begin simulating users logging in and performing all four API actions.

---

## ✅ Notes

- Each simulated user:
  - Logs in **once**.
  - Uses the retrieved token for all API calls.
- Tasks are executed randomly with a delay between 1–3 seconds.
- You can adjust the wait time by changing `wait_time = between(1, 3)`.
