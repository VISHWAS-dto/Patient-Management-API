<div align="center">

# 🏥 Patient Management API

<p align="center">
  <img src="https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white" />
  <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Pydantic-E92063?style=for-the-badge&logo=pydantic&logoColor=white" />
  <img src="https://img.shields.io/badge/Uvicorn-2C974B?style=for-the-badge&logo=gunicorn&logoColor=white" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/version-1.0.0-blue?style=flat-square" />
  <img src="https://img.shields.io/badge/license-MIT-green?style=flat-square" />
  <img src="https://img.shields.io/badge/status-active-brightgreen?style=flat-square" />
  <img src="https://img.shields.io/badge/PRs-welcome-orange?style=flat-square" />
</p>

> **A blazing-fast REST API for managing patient health records with automatic BMI calculation and intelligent health verdict classification — built with FastAPI & Pydantic v2.**

</div>

---

## 📌 Table of Contents

- [✨ Features](#-features)
- [🛠 Tech Stack](#-tech-stack)
- [📁 Project Structure](#-project-structure)
- [🚀 Getting Started](#-getting-started)
- [📡 API Endpoints](#-api-endpoints)
- [🗂 Data Model](#-data-model)
- [⚖️ BMI Classification](#️-bmi-classification)
- [📦 Sample Data](#-sample-data)
- [🐛 Known Issues & Fixes](#-known-issues--fixes)
- [🤝 Contributing](#-contributing)
- [📄 License](#-license)

---

## ✨ Features

```
✅  Create patient records          — with full validation
✅  Update patient data             — partial updates supported
✅  Auto BMI calculation            — always derived from height & weight
✅  Health verdict classification   — Underweight / Normal / Overweight / Obese
✅  Persistent JSON storage         — no database setup needed
✅  Swagger UI out of the box       — at /docs
✅  Input validation                — powered by Pydantic v2
```

---

## 🛠 Tech Stack

| Technology | Purpose | Version |
|------------|---------|---------|
| ⚡ **FastAPI** | Web framework & OpenAPI docs | `>=0.110.0` |
| 🔍 **Pydantic v2** | Data validation & computed fields | `>=2.6.0` |
| 🦄 **Uvicorn** | ASGI server | `>=0.29.0` |
| 🐍 **Python** | Language runtime | `3.10+` |

---

## 📁 Project Structure

```
📦 patient-management-api
 ┣ 📜 main.py            # FastAPI app — routes, models, business logic
 ┣ 🗃️  patients.json      # Persistent patient data store
 ┣ 📋 requirements.txt   # Python dependencies
 ┣ 🚫 .gitignore
 ┗ 📖 README.md
```

---

## 🚀 Getting Started

### 🔧 Prerequisites

- Python `3.10+`
- `pip` package manager

### ⚙️ Installation

```bash
# 1️⃣  Clone the repository
git clone https://github.com/your-username/patient-management-api.git
cd patient-management-api

# 2️⃣  Create & activate virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3️⃣  Install dependencies
pip install -r requirements.txt

# 4️⃣  Run the server
uvicorn main:app --reload
```

🟢 Server running at → `http://127.0.0.1:8000`  
📚 Swagger UI docs → `http://127.0.0.1:8000/docs`  
📘 ReDoc docs → `http://127.0.0.1:8000/redoc`

---

## 📡 API Endpoints

### `GET /`
> Health check

```json
{ "message": "Hello" }
```

---

### `POST /create`
> Register a new patient

**Request Body:**
```json
{
  "id":     "P007",
  "name":   "Ravi Kumar",
  "city":   "Delhi",
  "age":    25,
  "gender": "male",
  "height": 1.70,
  "weight": 68.0
}
```

**Response `201`:**
```json
{ "message": "patient created successfully" }
```

| Status | Meaning |
|--------|---------|
| `201` | Patient created |
| `400` | Patient ID already exists |

---

### `PUT /edit/{patient_id}`
> Update an existing patient — all fields optional

**Example — update weight only:**
```json
{ "weight": 72.5 }
```

> 🔄 BMI and verdict are **automatically recalculated** on every update.

**Response `200`:**
```json
{ "message": "patient updated" }
```

| Status | Meaning |
|--------|---------|
| `200` | Patient updated |
| `404` | Patient not found |

---

## 🗂 Data Model

| Field | Type | Constraints | Auto-computed |
|-------|------|-------------|:---:|
| `id` | `str` | Unique, required | ❌ |
| `name` | `str` | Required | ❌ |
| `city` | `str` | Required | ❌ |
| `age` | `int` | `0 < age < 120` | ❌ |
| `gender` | `str` | `male / female / others` | ❌ |
| `height` | `float` | `> 0` (metres) | ❌ |
| `weight` | `float` | `> 0` (kg) | ❌ |
| `bmi` | `float` | Derived from height & weight | ✅ |
| `verdict` | `str` | Derived from BMI | ✅ |

---

## ⚖️ BMI Classification

```
BMI = weight (kg) / height² (m²)
```

| Range | Verdict | Emoji |
|-------|---------|-------|
| BMI < 18.5 | Underweight | 🔵 |
| 18.5 ≤ BMI < 25 | Normal | 🟢 |
| 25 ≤ BMI < 30 | Overweight | 🟡 |
| BMI ≥ 30 | Obese | 🔴 |

---

## 📦 Sample Data

The repo ships with **6 pre-loaded patients** across Indian cities:

| ID | Name | City | BMI | Verdict |
|----|------|------|-----|---------|
| P001 | Ananya Sharma | Guwahati | 33.06 | 🔴 Obese |
| P002 | Rahul Mehta | Mumbai | 27.76 | 🟡 Overweight |
| P003 | Sneha Kulkarni | Pune | 17.58 | 🔵 Underweight |
| P004 | Arjun Verma | Bangalore | 29.32 | 🟡 Overweight |
| P005 | Neha Sinha | Kolkata | 31.22 | 🔴 Obese |
| P006 | Shankar | Lucknow | 25.18 | 🟢 Normal |

---

## 🐛 Known Issues & Fixes

### ⚠️ Verdict Bug in `main.py`

The `Overweight` range (BMI 25–29.9) incorrectly returns `"Normal"`.

```python
# ❌ Current (wrong)
elif self.bmi < 30:
    return 'Normal'

# ✅ Fix
elif self.bmi < 30:
    return 'Overweight'
```

---

## 🤝 Contributing

Contributions are always welcome! 🎉

```bash
# 1. Fork the repo
# 2. Create your branch
git checkout -b feature/amazing-feature

# 3. Commit your changes
git commit -m "Add amazing feature"

# 4. Push and open a PR
git push origin feature/amazing-feature
```

---

## 📄 License

```
MIT License — feel free to use, modify, and distribute.
```

---

<div align="center">

Made with ❤️ using FastAPI & Python

⭐ **Star this repo if you found it helpful!** ⭐

</div>
