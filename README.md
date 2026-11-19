# Multi-PDF RAG

A Python-based Retrieval-Augmented Generation (RAG) application that answers questions from PDF documents. The project uses LangChain, Hugging Face embeddings, FAISS vector search, and OpenAI-compatible chat models.

The current application loads `Brief Report.pdf`, splits it into searchable chunks, stores embeddings in a local FAISS index, and returns answers from the document in an interactive terminal interface.

## Features

- Load PDF documents with PyPDFLoader
- Split documents into overlapping chunks
- Generate embeddings with `sentence-transformers/all-MiniLM-L6-v2`
- Store and retrieve document vectors with local FAISS
- Retrieve the three most relevant text chunks per question
- Generate answers with `gpt-4.1-mini`
- Run answers through a simple command-line chat interface
- Return a fallback response when the PDF does not contain the answer

## Project Structure

```text
.
├── Brief Report.pdf  # Example PDF used by the application
├── rag_app.py        # RAG pipeline and interactive CLI
└── README.md
```

## Requirements

- Python 3.10 or newer
- An OpenAI API key
- Internet access for downloading the embedding model and calling the chat model

## Installation

1. Clone the repository:

```bash
git clone https://github.com/sureshsastika2026-glitch/multi_rag.git
cd multi_rag
```

2. Create and activate a virtual environment:

```bash
python -m venv .venv
```

On macOS/Linux:

```bash
source .venv/bin/activate
```

On Windows:

```bash
.venv\Scripts\activate
```

3. Install the required packages:

```bash
pip install python-dotenv langchain-community langchain-huggingface langchain-text-splitters langchain-openai faiss-cpu pypdf sentence-transformers
```

4. Create a `.env` file in the project root:

```dotenv
OPENAI_API_KEY=your_api_key_here
```

## Usage

Run the application:

```bash
python rag_app.py
```

The script creates a local `faiss_index/` directory the first time it runs. It then starts an interactive prompt:

```text
RAG app is ready.
Ask questions from your PDF.
Type 'exit' to stop.

Ask question: What is the brief report about?
```

Type `exit` or `quit` to stop the application.

## How It Works

1. `PyPDFLoader` extracts text from `Brief Report.pdf`.
2. `RecursiveCharacterTextSplitter` divides the text into 1,000-character chunks with 200-character overlap.
3. Hugging Face creates embeddings for each chunk.
4. FAISS stores the vectors locally and returns the three chunks most similar to each question.
5. The retrieved context and question are sent to `gpt-4.1-mini` with a prompt that restricts answers to the provided document.
6. The answer is printed in the terminal.

## Configuration

The main settings are currently defined in `rag_app.py`:

| Setting | Current Value |
|---|---|
| PDF path | `Brief Report.pdf` |
| FAISS index path | `faiss_index` |
| Embedding model | `sentence-transformers/all-MiniLM-L6-v2` |
| Chat model | `gpt-4.1-mini` |
| Chunk size | 1000 characters |
| Chunk overlap | 200 characters |
| Retrieved chunks | 3 |

## Notes

- The application currently reads one PDF (`Brief Report.pdf`), not multiple PDF uploads.
- A Streamlit interface is not implemented in the existing code.
- Keep the FAISS index in sync with document changes by deleting the `faiss_index/` directory and running the script again.
- Do not commit API keys or `.env` files to source control.
