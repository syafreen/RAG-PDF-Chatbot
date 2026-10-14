
# RAG-PDF-Chatbot
### Full-Stack RAG-Based PDF Question Answering System

<p align="center">
  <img src="https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white" alt="Python"/>
  <img src="https://img.shields.io/badge/React-18-61DAFB?logo=react&logoColor=black" alt="React"/>
  <img src="https://img.shields.io/badge/Vite-5-646CFF?logo=vite&logoColor=white" alt="Vite"/>
  <img src="https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white" alt="FastAPI"/>
  <img src="https://img.shields.io/badge/LangChain-RAG-1C3C3C" alt="LangChain"/>
  <img src="https://img.shields.io/badge/Google-Gemini-4285F4?logo=google&logoColor=white" alt="Google Gemini"/>
  <img src="https://img.shields.io/badge/Vector%20Database-Pinecone-000000" alt="Pinecone"/>
</p>

<p align="center">
  A full-stack AI-powered document assistant for PDF upload, semantic retrieval, and context-aware question answering using Retrieval-Augmented Generation (RAG).
</p>

---

## Table of Contents

- [Overview](#overview)
- [Problem Statement](#problem-statement)
- [System Architecture](#system-architecture)
- [Key Features](#key-features)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
  - [1. Prerequisites](#1-prerequisites)
  - [2. Backend Setup](#2-backend-setup)
  - [3. Frontend Setup](#3-frontend-setup)
- [How It Works](#how-it-works)
- [API Reference](#api-reference)
- [Configuration](#configuration)
- [Future Roadmap](#future-roadmap)
- [Author](#author)
- [Acknowledgements](#acknowledgements)
- [License](#license)

---

## Overview

**RAG-PDF-Chatbot** is a full-stack Retrieval-Augmented Generation (RAG) application that allows users to interact with PDF documents through natural-language questions.

The application combines document processing, vector search, and large language models to retrieve relevant information and generate answers grounded in document content.

The frontend is built with React and Vite, while the backend uses FastAPI, LangChain, Google Gemini, and Pinecone to support the document question-answering workflow.

---

## System Architecture

The application follows a frontend-backend architecture connected to a RAG pipeline.

```mermaid


### 🧠 Architecture

```mermaid
flowchart TB
    UI["React Frontend<br/>(PDF Upload · Chat · Citations)"]
    API["FastAPI Backend<br/>(Port 8000)"]

    UI -->|"HTTP: POST /upload, POST /query"| API

    subgraph RAG["rag_engine.py"]
        direction TB

        subgraph INGEST["PDF Processing"]
            direction TB
            PDF["PDF Bytes"]
            EXTRACT["pypdf<br/>(Extract Text)"]
            CHUNK["LangChain<br/>(Text Chunking)"]
            EMB["Gemini Embedding API"]
            STORE[("Pinecone<br/>(Vectors + Metadata)")]

            PDF --> EXTRACT --> CHUNK --> EMB --> STORE
        end

        subgraph QUERY["Question Answering"]
            direction TB
            Q["User Question"]
            QEMB["Gemini Embedding API<br/>(Query Vector)"]
            SEARCH["Pinecone Similarity Search"]
            CONTEXT["Retrieved Context"]
            LLM["Gemini 2.5 Flash<br/>(Answer Generation)"]

            Q --> QEMB --> SEARCH --> CONTEXT --> LLM
        end

        STORE --> SEARCH
    end

    API --> PDF
    API --> Q
    LLM --> API
    API --> UI
```
1. **PDF Upload:** The user uploads a PDF through the React frontend.
2. **Text Extraction:** The FastAPI backend extracts text from the PDF using `pypdf`.
3. **Document Chunking:** LangChain splits the extracted text into smaller chunks.
4. **Embedding and Storage:** Gemini generates embeddings, which are stored in Pinecone.
5. **Question Processing:** The backend converts the user's question into an embedding and searches Pinecone for relevant document chunks.
6. **Answer Generation:** Google Gemini uses the retrieved context to generate an answer.
7. **Response Display:** The answer is returned to the frontend and displayed to the user.

```

## Key Features

- **PDF Upload:** Upload PDF documents through the web interface.
- **Document Question Answering:** Ask natural-language questions about document content.
- **Retrieval-Augmented Generation:** Retrieve relevant context before generating responses.
- **Semantic Search:** Retrieve information based on query meaning.
- **AI-Powered Responses:** Integrate Google Gemini into the question-answering workflow.
- **Vector Database:** Use Pinecone for document vector storage and retrieval.
- **Interactive Chat Interface:** Present questions and answers in a React-based interface.
- **Modular Backend:** Separate API handling, chat functionality, and RAG logic.
- **API-Based Integration:** Connect the frontend and backend using HTTP requests.

---

## Tech Stack

| Component | Technology | Role |
|---|---|---|
| Frontend | React 18, Vite 5 | Interactive user interface |
| Language | Python | Backend application logic |
| Backend API | FastAPI, Uvicorn | API endpoints and request handling |
| HTTP Client | Axios | Frontend API communication |
| RAG Framework | LangChain | Retrieval and document processing |
| LLM & Embeddings | Google Gemini | Answer generation and text embeddings |
| Vector Database | Pinecone | Document vector storage and retrieval |
| PDF Processing | pypdf | PDF text extraction |

---

## Project Structure

```text
RAG-PDF-Chatbot/
│
├── assets/
│   ├── Homepage.png              # Application homepage screenshot
│   └── chat_response.png         # Chat response screenshot
│
├── backend/
│   ├── main.py                   # FastAPI application and endpoints
│   ├── chat.py                   # Chat functionality
│   ├── rag_engine.py             # RAG and retrieval logic
│   ├── requirements.txt          # Python dependencies
│   └── .env.example              # Backend environment template
│
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   ├── Message.jsx       # Chat message component
│   │   │   └── Sidebar.jsx       # Sidebar and PDF upload interface
│   │   ├── api.js                # Frontend API communication
│   │   ├── App.jsx               # Main React application
│   │   ├── index.css             # Application styles
│   │   └── main.jsx              # React entry point
│   ├── index.html                # HTML entry point
│   ├── package.json              # Frontend dependencies and scripts
│   ├── package-lock.json         # Locked npm dependency versions
│   └── vite.config.js            # Vite configuration
│
├── .gitignore                    # Files excluded from Git
├── LICENSE                       # Project license
└── README.md                     # Project documentation
```

---

## ## Getting Started

Follow these steps to set up and run the application locally.

### 1. Prerequisites

Install the following:

1. Python compatible with the backend dependencies.
2. Node.js and npm.
3. ### Prerequisites

- Python 3.11
- Node.js 18+ and npm
- Free [Gemini API key](https://aistudio.google.com/app/api-keys)
- Free [Pinecone API key](https://app.pinecone.io/) 

---

### 2. Backend Setup

Open a terminal and navigate to the backend directory.

**Windows (PowerShell):**

```powershell
cd backend
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

**macOS / Linux:**

```bash
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
cp .env.example .env
```

Configure your API credentials in `backend/.env`:

```env
GEMINI_API_KEY=your_gemini_api_key
PINECONE_API_KEY=your_pinecone_api_key
PINECONE_INDEX_NAME=doc-chatbot
```

Start the backend server:

```bash
uvicorn main:app --reload --port 8000
```

The backend will run at:

`http://localhost:8000`

Test the backend health endpoint:

`http://localhost:8000/health`

Open the interactive API documentation at:

`http://localhost:8000/docs`

---

### 3. Frontend Setup

Open a **second terminal** and navigate to the frontend directory. Keep the backend running in the first terminal.

**Windows (PowerShell):**

```powershell
cd frontend
npm install
npm run dev
```

**macOS / Linux:**

```bash
cd frontend
npm install
npm run dev
```

Vite will display the local development URL in the terminal. The default is usually:

`http://localhost:5173`

Open that URL in your browser to access the application.

Ensure that `frontend/src/api.js` points to the correct backend address:

`http://localhost:8000`

---
---

## API Reference

The backend exposes HTTP endpoints through FastAPI for health checks, PDF uploads, and document question answering.

### Health Check

- **Endpoint:** `GET /health`
- **Purpose:** Check backend availability.
- **Test URL:** `http://localhost:8000/health`

### PDF Upload

- **Endpoint:** `POST /upload`
- **Content-Type:** `multipart/form-data`
- **Request field:** `file`
- **Purpose:** Upload a PDF for document processing.

Example using cURL:

```bash
curl -X POST "http://localhost:8000/upload" \
  -F "file=@document.pdf"
```

Replace `document.pdf` with the path to your PDF file.

### Document Question Answering

- **Endpoint:** `POST /query`
- **Content-Type:** `application/json`
- **Purpose:** Submit a question about uploaded document content.

Example request body:

```json
{
  "question": "What are the main points in this document?",
  "chat_history": []
}
```

Example using cURL:

```bash
curl -X POST "http://localhost:8000/query" \
  -H "Content-Type: application/json" \
  -d '{"question":"What are the main points in this document?","chat_history":[]}'
```

The response fields depend on the response schema implemented by the backend.

### Interactive API Documentation

Start the backend and open:

`http://localhost:8000/docs`

The Swagger UI lets you inspect available endpoints, request parameters, schemas, and response formats.
---

## Configuration

The application uses environment variables to configure the backend and frontend.

### Backend (`backend/.env`)

Create a local `.env` file inside the `backend/` directory using `.env.example` as a template.

```env
GEMINI_API_KEY=[your_gemini_api_key](https://aistudio.google.com/app/apikey)
PINECONE_API_KEY=[your_pinecone_api_key](https://app.pinecone.io)
PINECONE_INDEX_NAME=doc-chatbot
```

Replace the placeholder values with your own credentials. Ensure that the configured Pinecone index is compatible with your embedding model.

### Frontend API Configuration

The frontend API client is configured in:

`frontend/src/api.js`

The local backend URL is:

```text
http://localhost:8000
```

Ensure that the backend is running at this address when developing locally.

### Security Considerations

- Never commit API keys or real `.env` files to GitHub.
- Keep `.env` files excluded from version control.
- Use your own API credentials and Pinecone resources.
- Review how uploaded documents and their contents are processed by external services.

---

## Future Roadmap

- [ ] Support for additional document formats.
- [ ] Improved retrieval using hybrid search and reranking.
- [ ] Source references and page-level citations.
- [ ] Persistent conversation history.
- [ ] User authentication and document access controls.
- [ ] Retrieval and answer-quality evaluation.
- [ ] Docker-based deployment and CI/CD integration.

---

#### Author

**Syed Yellanoor Afreen**

- GitHub: [@syafreen](https://github.com/syafreen)
- Repository: [RAG-PDF-Chatbot](https://github.com/syafreen/RAG-PDF-Chatbot)

---

## License

This project is licensed under the [MIT License](LICENSE).
