# NAAC Compliance System with RAG based Chatbot Assistance

This repository documents the design and architecture of the **NAAC Automation Portal**, a proposed AI-powered compliance automation platform for the NAAC accreditation process. It outlines how artificial intelligence, large language models, document processing, and RAG-based chatbot systems can be integrated to streamline documentation, validation, and communication with the Internal Quality Assurance Cell (IQAC).

---

## Overview

The NAAC Automation Portal provides an AI-based backend and web-based frontend to handle key stages in the accreditation workflow:

* Excel metadata validation and proof document checking
* Centralized aggregation of documents from various campuses
* RAG-based chatbot for user assistance on NAAC criteria
* OCR support for scanned documents
* Summarization, document comparison, and missing data identification

The portal is designed to reduce manual effort, minimize submission errors, and improve the efficiency of preparing for NAAC accreditation.

[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)


---


## System Architecture

The NAAC Automation Portal is structured as a modular, end-to-end automation pipeline designed to handle the entire documentation process for NAAC accreditation. It combines document ingestion, AI-based analysis, real-time interaction, and cloud storage integration.

### 1. Frontend (User Interface)

* Developed using **Streamlit**, with HTML/CSS/JavaScript components for interactivity
* Provides functionality for:

  * Campus and criterion selection
  * File uploads (Excel and PDFs)
  * Template viewing and editing (via Handsontable)
  * Interaction with the AI chatbot
  * Triggering summarization, document comparison, and missing information checks
 
<p align="center">
  <img src="https://i.postimg.cc/tC4bG55y/Picture1.jpg" alt="App Screenshot" width="600">
</p>  

### 2. Backend (Data and Logic Layer)

Built on **Python (Flask or FastAPI)** and **Node.js** services, the backend handles:

* File processing (validation, merging, extraction)
* Session handling and routing requests from frontend
* Query dispatching to document databases and LLMs
* Summarization and NLP-based insights

<p align="center">
  <img src="https://i.postimg.cc/9XxjYk3T/Picture2.jpg" alt="App Screenshot" width="600">
</p>  

### 3. Optical Character Recognition (OCR)

* Utilizes **Tesseract OCR** to extract machine-readable text from scanned PDFs or image-based documents
* Converts uploaded files into `.txt` format for consistency
* Supports page-wise PDF conversion using `pdf2image`, preprocessing, and noise filtering

### 4. Document Vectorization Pipeline

* Converts cleaned `.txt` documents into semantic vector embeddings
* Pipeline steps:

  * **DirectoryLoader**: Recursively loads documents from campus/criterion directories
  * **RecursiveCharacterTextSplitter**: Divides large documents into overlapping chunks
  * **OpenAIEmbeddings**: Generates semantic embeddings from text
  * **FAISS**: Indexes embeddings into a local searchable vector database
<p align="center">
  <img src="https://i.postimg.cc/yYcxpB7t/Picture3.png" alt="App Screenshot" width="600">
</p>  

### 5. Retrieval-Augmented Generation (RAG) System

* Central AI agent that processes user queries contextually
* Combines:

  * LangChain’s `AgentExecutor` to orchestrate tools
  * Tools like `WikipediaQueryRun`, `GoogleSearchAPIWrapper`
  * A retriever that queries the FAISS vector store using semantic similarity
* The agent selects between internal data, Wikipedia, and web search based on the query

<p align="center">
  <img src="https://i.postimg.cc/65gVnBYP/Picture4.jpg" alt="App Screenshot" width="200">
</p>  

### 6. Chatbot Interface

* Real-time assistant built on **OpenAI GPT models**
* Responds to:

  * Queries about NAAC criteria
  * Missing template data
  * Process flow clarification
* Connects to the document embeddings, so answers are context-aware and grounded in uploaded documentation

<p align="center">
  <img src="https://i.postimg.cc/0QMGh5dg/Picture5.jpg" alt="App Screenshot" width="400">
</p>  

### 7. Summarization Pipeline

* Implements a **Map-Reduce** strategy using LangChain:

  * **Map Phase**: Generates summaries for each chunk
  * **Reduce Phase**: Merges intermediate summaries into a cohesive final summary
* Ensures scalable summarization for large datasets (e.g., 80+ documents per criterion)

<p align="center">
  <img src="https://i.postimg.cc/MKMkt1jt/Picture6.jpg" alt="App Screenshot" width="600">
</p>  

### 8. Document Comparison and Merge

* Accepts multiple document uploads
* Runs a vector-based semantic alignment and comparison
* Detects redundant sections, differences, and structural mismatches
* Outputs:

  * Aligned text comparisons
  * Highlighted missing data
  * Merged documents with contextual alignment

<p align="center">
  <img src="https://i.postimg.cc/wMNCxpzZ/Picture7.png" alt="App Screenshot" width="600">
</p>  

### 9. Missing Information Generator

* Accepts a base (parent) document and a partially filled template
* Identifies missing fields via:

  * Embedding comparison
  * Query chains
  * Prompt engineering (e.g., “Compare X with Y, fill in Z”)
* Returns either text or completed templates

<p align="center">
  <img src="https://i.postimg.cc/wMNCxpzZ/Picture7.png" alt="App Screenshot" width="600">
</p>  

### 10. File and Cloud Integration

* Uses Node.js/Express for:

  * Directory creation per campus and criterion
  * File downloads/uploads
* Supports OneDrive API integration for cloud aggregation of proof documents

---

## Features

The system provides a comprehensive feature set that transforms the traditionally manual and error-prone NAAC compliance process into an intelligent, efficient, and automated workflow.
Each of the results mentioned are explained in detail in the document above with picture proofs representing the functionality.

### Document Validation

* Excel templates are parsed and checked for:

  * Metadata completeness
  * Matching document names and formats
  * Required fields for each NAAC metric
* Immediate feedback is given through the frontend

### Document Aggregation

* Documents are sorted automatically by:

  * Campus
  * Criterion
  * Document type (Proof, Template, etc.)
* Integrated into OneDrive for persistent storage

### Intelligent Chatbot (RAG-based)

* Context-aware Q\&A over uploaded documents
* Answers template-specific questions (e.g., “What is missing in 1.1.1?”)
* Uses LLM with vector search and fallback to web search tools

### Summarization

* Handles multi-document summarization
* Outputs include:

  * Summary by criterion
  * Aggregated summaries for full submissions
* Designed for IQAC reporting and review

### OCR and File Conversion

* Converts scanned PDFs into text
* Supports:

  * English documents with complex layouts
  * Table and column recognition (basic)

### Missing Information Detection

* Compares input files with parent documents
* Finds unfilled fields or mismatched entries
* Suggests or fills data automatically using LLMs

### Document Comparison & Merging

* Compares multiple documents chunk-by-chunk
* Identifies:

  * Overlapping content
  * Deviations in structure
  * Additional or missing sections
* Produces consolidated output

### Excel Editing and Web Interface

* Handsontable-based Excel preview and editing
* Users can:

  * Edit directly in browser
  * Download updated versions
  * Save progress locally during sessions

### Modular and Extensible

* LangChain tool-based architecture
* Easily extendable with:

  * New NAAC criteria
  * Additional agents (e.g., CV parser, LMS sync tool)
  * Custom summarizers or validators

---




## Technology Stack

### Backend

* Python 3.11
* Node.js, Express
* Flask or FastAPI
* OpenAI API
* FAISS for vector storage
* LangChain
* Tesseract OCR

### Frontend

* Streamlit
* HTML5, CSS3, JavaScript
* Handsontable.js
* Bootstrap 4+

---

## Installation and Deployment (Reference Design)

> This repository contains **documentation only**. The steps below describe how to set up the system **if implementing the described architecture.**

### 1. Python Environment Setup

Install dependencies:

```bash
pip install langchain openai streamlit faiss-cpu pytesseract pdf2image PyPDF2
pip install python-dotenv sse_starlette bs4 wikipedia
```

Install Tesseract OCR and poppler for PDF conversion:

* **Ubuntu/Debian**:

  ```bash
  sudo apt install tesseract-ocr poppler-utils
  ```
* **Windows**:

  * Download [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
  * Add the executable path to the system environment variables

### 2. Node.js Backend (Optional - For Download Automation)

Install dependencies:

```bash
npm install express node-fetch fs cors
```

Use Node.js scripts to manage file uploads/downloads and directory structure.

### 3. Environment Variables

Configure the following keys in a `.env` file:

```
OPENAI_API_KEY=your_openai_key
GOOGLE_API_KEY=your_google_api_key
GOOGLE_CSE_ID=your_custom_search_id
LANGCHAIN_API_KEY=your_langchain_api_key
```

### 4. Running the Application

Start the frontend:

```bash
streamlit run app.py
```

Start the backend (if used):

```bash
node index.js
```

---

## Usage

* Navigate to the Streamlit frontend
* Select NAAC criterion and upload Excel/PDF files
* Validate templates or use chatbot for questions
* Run document comparison or summarization tools
* Export outputs (text or PDF reports)

---

## Repository Contents

This repository includes:

* `NAAC Automation.docx`: Complete project documentation
* System architecture, block diagrams, and design flow
* Details on modules such as RAG chatbot, OCR, vector database, and summarization

---


