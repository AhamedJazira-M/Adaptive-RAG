# Adaptive RAG for PDF Question Answering

**An Adaptive RAG (Retrieval-Augmented Generation) is an advanced AI framework that dynamically chooses whether and how to retrieve information based on the specific complexity of a user's query. Unlike standard RAG, which blindly performs external searches for every single prompt, adaptive RAG thinks before acting to save time and compute power.**.

Unlike traditional RAG, the system analyzes the user's question first and dynamically selects an appropriate retrieval strategy.

## What Does Adaptive RAG Actually Do?

Adaptive RAG improves traditional RAG by deciding how to retrieve information based on the user's question.
Instead of using the same retrieval process for every query, it adapts the retrieval approach according to the complexity, type, and context of the question.

```text
Main Processes

User Query
    ↓
Understand the Query
    ↓
Classify the Query
    ↓
Choose Retrieval Strategy
    ↓
Retrieve Relevant Information
    ↓
Evaluate the Retrieved Information
    ↓
Generate the Final Answer
```
## Features

* PDF upload and processing
* Text chunking and embeddings
* ChromaDB vector database
* Adaptive query classification
* Factual, analytical, opinion, and document-contextual modes
* Page-aware retrieval
* Relevance checking
* Groq LLM for answer generation
* FastAPI web interface
* Google Colab support

## Architecture

```text
PDF
 ↓
Text Chunking
 ↓
HuggingFace Embeddings
 ↓
ChromaDB
 ↓
User Query
 ↓
Query Classification
 ↓
Adaptive Retrieval
 ↓
Relevant Context
 ↓
Groq LLM
 ↓
Answer
```

## How It Works

### 1. PDF Processing

The uploaded PDF is loaded page by page and divided into smaller text chunks.

Each chunk is converted into an embedding and stored in ChromaDB along with metadata such as the page number.

For example:

```text
Pages: 5
Chunks: 20
```

This means that the five-page document was divided into 20 searchable chunks.

### 2. Query Classification

When a question is submitted, the system determines what type of question is being asked.

| Mode                | Example                                         |
| ------------------- | ----------------------------------------------- |
| Factual             | What dataset was used?                          |
| Analytical          | Why is this approach suitable for edge devices? |
| Opinion             | Which approach is more suitable?                |
| Document Contextual | What does the abstract say?                     |

The classification helps determine how the document should be searched.

### 3. Adaptive Retrieval

The retrieval strategy changes according to the query.

| Query Type          | Retrieval                |
| ------------------- | ------------------------ |
| Factual             | Precise retrieval        |
| Analytical          | Broader retrieval        |
| Opinion             | Evidence-based retrieval |
| Document Contextual | Page-aware retrieval     |

For example:

```text
What does the abstract say?
        ↓
Document Contextual
        ↓
Page-Aware Retrieval
        ↓
Relevant Chunks
```

### 4. Answer Generation

The retrieved document chunks are passed to the Groq LLM together with the user's question.

The LLM uses this context to generate the final answer.

```text
Question
   ↓
Retrieval
   ↓
Evidence
   ↓
Groq LLM
   ↓
Answer
```

## Output

The web interface displays information about the Adaptive RAG process.

| Output             | Meaning                                         |
| ------------------ | ----------------------------------------------- |
| Query Type         | How the question was classified                 |
| Scope              | Whether local or global information is required |
| Retrieval Strategy | How the document was searched                   |
| Target Page        | Page identified as relevant                     |
| Source Pages       | Pages containing retrieved evidence             |
| Relevance          | Whether the retrieved evidence is sufficient    |
| Answer             | Final LLM-generated response                    |

Example:

```text
Query Type: DOCUMENT_CONTEXTUAL
Scope: LOCAL
Retrieval Strategy: PAGE_AWARE
Target Page: 1
Source Pages: 1
Relevance: RELEVANT

Answer:
The abstract describes ...
```

## Technologies

* **Python** — Programming language
* **FastAPI** — API and web interface
* **ChromaDB** — Vector database
* **HuggingFace** — Text embeddings
* **Groq** — LLM inference
* **PyPDF** — PDF processing
* **Google Colab** — Development environment

  ## Models and Components

| Component       | Model / Technology                       | Purpose                                                          |
| --------------- | ---------------------------------------- | ---------------------------------------------------------------- |
| LLM             | `openai/gpt-oss-120b` via Groq           | Query classification, retrieval decisions, and answer generation |
| Embedding Model | `sentence-transformers/all-MiniLM-L6-v2` | Converts document chunks and queries into embeddings             |
| Vector Database | `ChromaDB`                               | Stores embeddings and performs similarity search                 |
| PDF Loader      | `PyPDFLoader`                            | Extracts text from PDF documents                                 |
| API Framework   | `FastAPI`                                | Provides the backend API and web interface                       |
| Server          | `Uvicorn`                                | Runs the FastAPI application                                     |


## Installation

Install the required packages in Google Colab:

```bash
pip install fastapi uvicorn chromadb groq langchain langchain-huggingface pypdf python-multipart nest-asyncio
```

Set your Groq API key:

```python
import os

os.environ["GROQ_API_KEY"] = "YOUR_GROQ_API_KEY"
```

## Running the Project

Run the three code blocks in order:

```text
Block 1
↓
Upload and process PDF

Block 2
↓
Adaptive RAG

Block 3
↓
FastAPI Web Interface
```

After starting the FastAPI server, open the **Colab port preview** for the port displayed by the server.

## Example Questions

```text
What does the abstract say?

What dataset was used in the study?

Why is this approach suitable for edge devices?

Summarize the entire document.

What is the conclusion of the paper?
```

These questions demonstrate different query classifications and retrieval strategies.

## FastAPI Endpoints

| Method | Endpoint  | Purpose                                                                      |
| ------ | --------- | ---------------------------------------------------------------------------- |
| `GET`  | `/`       | Opens and displays the Adaptive RAG web interface.                           |
| `POST` | `/ask`    | Sends the user's question to the Adaptive RAG system and returns the result. |
| `GET`  | `/health` | Checks whether the FastAPI server is running.                                |

### GET vs POST

| Method | Purpose                                                  |
| ------ | -------------------------------------------------------- |
| `GET`  | Used to request or retrieve information from the server. |
| `POST` | Used to send data to the server for processing.          |


## Summary

This project demonstrates an Adaptive RAG system that changes its retrieval behavior according to the user's question.

The main workflow is:

```text
PDF → ChromaDB → Adaptive Retrieval → Groq LLM → FastAPI
```

The goal is to retrieve the most appropriate document context for different types of questions instead of applying the same retrieval strategy to every query.
