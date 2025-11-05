---
description: >-
  Excellent question 🔥 — before you jump into writing endpoints, it’s super
  smart to understand the overall syntax and structure of a FastAPI app.
---

# 🧩 FastAPI Syntax & Structure — The Big Picture

Let’s break this down clearly so you’ll _never feel lost_ in any FastAPI codebase — whether it’s small or production-grade.

***

Every FastAPI project, big or small, follows the same **core building pattern**.

***

### 🧠 Step 1: Import and Create the App

```python
from fastapi import FastAPI

app = FastAPI()
```

**What’s happening here:**

* `FastAPI()` creates the main **application instance**.
* Think of it as your API “brain” — all routes, middlewares, and configurations are attached to this object.
* You’ll later give this object to **Uvicorn** (the server) to run.

***

### 🧠 Step 2: Define Endpoints (Routes)

These are your **API URLs** — where users send requests.

#### Basic example:

```python
@app.get("/")
def home():
    return {"message": "Hello FastAPI!"}
```

The `@app.get("/")` is a **decorator** that tells FastAPI:

> “Whenever someone sends a GET request to `/`, call this function.”

***

#### Common HTTP methods

| Method   | Decorator       | Use for             |
| -------- | --------------- | ------------------- |
| `GET`    | `@app.get()`    | Read data           |
| `POST`   | `@app.post()`   | Create data         |
| `PUT`    | `@app.put()`    | Update all data     |
| `PATCH`  | `@app.patch()`  | Update part of data |
| `DELETE` | `@app.delete()` | Delete data         |

Example:

```python
@app.post("/items")
def create_item(item: dict):
    return {"message": "Item created", "item": item}
```

***

### 🧠 Step 3: Parameters and Data Inputs

You can take input in 3 ways:

1. **Path parameters** → inside the URL\
   `/users/{user_id}`
2. **Query parameters** → after `?` in URL\
   `/search?name=John`
3. **Request body** → JSON data sent by POST/PUT methods

Example:

```python
from fastapi import FastAPI

app = FastAPI()

@app.get("/users/{user_id}")
def read_user(user_id: int, active: bool = True):
    return {"user_id": user_id, "active": active}
```

Call:

> `/users/5?active=false`

You’ll get:

```json
{"user_id": 5, "active": false}
```

***

### 🧠 Step 4: Using Pydantic Models (for validation)

FastAPI automatically uses **Pydantic** to validate and serialize input/output.

```python
from pydantic import BaseModel

class Item(BaseModel):
    name: str
    price: float
    in_stock: bool

@app.post("/items")
def create_item(item: Item):
    return {"item_name": item.name, "price": item.price}
```

➡️ If you send wrong data (like a string instead of a number),\
FastAPI automatically returns a 422 validation error.

***

### 🧠 Step 5: Responses & Status Codes

You can control how your API responds.

```python
from fastapi import status

@app.post("/items", status_code=status.HTTP_201_CREATED)
def create_item(item: Item):
    return item
```

FastAPI will automatically return:

* ✅ `201 Created` status code
* ✅ JSON body from the return value

***

### 🧠 Step 6: Run the App

To run any FastAPI project:

```bash
uvicorn main:app --reload
```

* `main` = your Python file name (`main.py`)
* `app` = your FastAPI instance
* `--reload` = auto-restarts when code changes (useful for dev)

***

### 🧠 Step 7: Auto Docs (Free!)

FastAPI auto-generates two docs UIs:

| Docs type | URL      | Description              |
| --------- | -------- | ------------------------ |
| Swagger   | `/docs`  | Interactive testing UI   |
| ReDoc     | `/redoc` | Clean documentation view |

***

### 🧱 Minimal template for any FastAPI app

Here’s a **base structure** that works for any small to medium API:

```
project/
│
├── main.py           # entry point
├── models.py         # (optional) Pydantic or DB models
├── routers/          # (optional) route files
│   ├── users.py
│   ├── items.py
│
└── requirements.txt
```

And `main.py` might look like:

```python
from fastapi import FastAPI
from routers import users, items

app = FastAPI()

app.include_router(users.router)
app.include_router(items.router)
```

***

### 🧠 Step 8: Key Concepts Summary

| Concept               | Description                    |
| --------------------- | ------------------------------ |
| **FastAPI()**         | Creates the app instance       |
| **Decorators**        | Map routes to functions        |
| **Path/Query Params** | Extract data from URL          |
| **Pydantic Models**   | Validate request bodies        |
| **Return values**     | Automatically JSONified        |
| **Uvicorn**           | Runs the server                |
| **Docs**              | Auto-generated from type hints |

***

If you understand this syntax flow 👆 you can open **any FastAPI project** and immediately tell what’s happening.
