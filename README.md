# AI-Electrical-Fault-Diagnosis-App

AI Electrical Fault Diagnosis App

A full-stack AI-powered application that helps users diagnose electrical problems based on symptoms.


---

Project Features

Describe electrical symptoms

AI suggests possible causes

Safety warning detection

Severity level estimation

Recommended troubleshooting steps

REST API backend

Modern frontend UI

Ready for GitHub portfolio



---

Project Structure

ai-electrical-fault-app/
│
├── backend/
│   ├── app.py
│   ├── requirements.txt
│   ├── ai_engine.py
│   └── prompts.py
│
├── frontend/
│   ├── index.html
│   ├── style.css
│   └── script.js
│
└── README.md


---

1. BACKEND

backend/requirements.txt

fastapi
uvicorn
openai
python-dotenv
pydantic


---

backend/prompts.py

SYSTEM_PROMPT = """
You are an expert electrical technician assistant.

Your task:
1. Analyze electrical fault symptoms.
2. Suggest likely causes.
3. Provide troubleshooting steps.
4. Give safety warnings.
5. Estimate severity.

Return concise and practical responses.

Response format:

Problem:
Possible Causes:
Safety Risk:
Recommended Actions:
Severity:
"""


---

backend/ai_engine.py

from openai import OpenAI
from prompts import SYSTEM_PROMPT
import os

client = OpenAI(api_key=os.getenv("OPENAI_API_KEY"))


def diagnose_fault(user_input):
    response = client.chat.completions.create(
        model="gpt-4.1-mini",
        messages=[
            {
                "role": "system",
                "content": SYSTEM_PROMPT
            },
            {
                "role": "user",
                "content": user_input
            }
        ],
        temperature=0.4
    )

    return response.choices[0].message.content


---

backend/app.py

from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from pydantic import BaseModel
from ai_engine import diagnose_fault
from dotenv import load_dotenv

load_dotenv()

app = FastAPI()

app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)


class FaultRequest(BaseModel):
    symptom: str


@app.get("/")
def home():
    return {"message": "AI Electrical Fault Diagnosis API"}


@app.post("/diagnose")
def diagnose(data: FaultRequest):
    result = diagnose_fault(data.symptom)

    return {
        "diagnosis": result
    }


---

2. FRONTEND

frontend/index.html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Electrical Fault Diagnosis</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">
    <h1>⚡ AI Electrical Fault Diagnosis</h1>

    <textarea
        id="symptom"
        placeholder="Describe the electrical issue..."
    ></textarea>

    <button onclick="diagnoseFault()">
        Diagnose Problem
    </button>

    <div id="loading" class="hidden">
        Diagnosing...
    </div>

    <div id="result"></div>
</div>

<script src="script.js"></script>
</body>
</html>


---

frontend/style.css

body {
    font-family: Arial, sans-serif;
    background: #101820;
    color: white;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
    margin: 0;
}

.container {
    width: 90%;
    max-width: 700px;
    background: #1b263b;
    padding: 30px;
    border-radius: 12px;
}

h1 {
    text-align: center;
}

textarea {
    width: 100%;
    height: 150px;
    padding: 15px;
    margin-top: 20px;
    border-radius: 10px;
    border: none;
    font-size: 16px;
}

button {
    width: 100%;
    padding: 15px;
    margin-top: 20px;
    background: #ffb703;
    border: none;
    border-radius: 10px;
    font-size: 18px;
    cursor: pointer;
}

button:hover {
    background: #fca311;
}

#result {
    margin-top: 30px;
    background: #0d1b2a;
    padding: 20px;
    border-radius: 10px;
    white-space: pre-wrap;
}

.hidden {
    display: none;
}


---

frontend/script.js

async function diagnoseFault() {
    const symptom = document.getElementById("symptom").value;
    const resultDiv = document.getElementById("result");
    const loading = document.getElementById("loading");

    if (!symptom) {
        alert("Please describe the issue.");
        return;
    }

    loading.classList.remove("hidden");
    resultDiv.innerHTML = "";

    try {
        const response = await fetch("http://127.0.0.1:8000/diagnose", {
            method: "POST",
            headers: {
                "Content-Type": "application/json"
            },
            body: JSON.stringify({
                symptom: symptom
            })
        });

        const data = await response.json();

        resultDiv.innerHTML = data.diagnosis;
    } catch (error) {
        resultDiv.innerHTML = "Error connecting to server.";
    }

    loading.classList.add("hidden");
}


---

3. ENVIRONMENT VARIABLES

Create:

backend/.env

Add:

OPENAI_API_KEY=your_openai_api_key_here


---

4. RUN THE PROJECT

Backend

cd backend

pip install -r requirements.txt

uvicorn app:app --reload

Backend runs on:

http://127.0.0.1:8000


---

Frontend

Open:

frontend/index.html

in your browser.


---

5. EXAMPLE TEST INPUTS

The circuit breaker trips every time I switch on the kettle.

Lights flicker when the microwave starts.

There is a burning smell near the DB box.

The wall socket sparks when plugging in devices.


---

6. FUTURE IMPROVEMENTS

Add Voice Input

Use:

Web Speech API

Whisper AI



---

Add Image Upload

Users upload:

damaged wires

burnt sockets

DB boards


Then AI analyzes images.


---

Add Sesotho Support

Translate electrical advice into Sesotho.


---

Add Fault Categories

Wiring faults

Overload faults

Short circuits

Earth leakage

Appliance faults



---

Add Authentication

Use:

JWT

Firebase

Clerk



---

7. GITHUB README TEMPLATE

# AI Electrical Fault Diagnosis App

An AI-powered application that helps diagnose electrical problems using natural language.

## Features
- AI diagnostics
- Safety analysis
- Severity estimation
- Troubleshooting steps
- Modern frontend UI

## Tech Stack
- FastAPI
- OpenAI API
- HTML/CSS/JavaScript

## Installation

### Backend
```bash
pip install -r requirements.txt
uvicorn app:app --reload

Frontend

Open index.html in browser.

Author

Karabo Motsomi

---

# 8. HOW TO MAKE THIS PROJECT LOOK PROFESSIONAL

## Add these screenshots

- home page
- diagnosis results
- mobile view

---

## Deploy Backend

Deploy using:

- Render
- Railway
- Fly.io

---

## Deploy Frontend

Use:

- Netlify
- Vercel
- GitHub Pages

---

# 9. ADVANCED AI VERSION IDEAS

## Use Local AI Models

Instead of OpenAI:

- Ollama
- Mistral
- Llama 3

---

## Add Knowledge Base

Store:

- electrical manuals
- regulations
- troubleshooting guides

Use RAG architecture.

---

## Add Technician Dashboard

Electricians can:

- review cases
- respond to users
- export reports

---

# 10. NEXT UPGRADE YOU SHOULD BUILD

After this starter works, upgrade it to:

## AI Electrician Assistant SaaS

Features:

- user accounts
- subscriptions
- image diagnosis
- voice assistant
- WhatsApp integration
- PDF report generation
- multilingual support

That version becomes a serious portfolio project.
