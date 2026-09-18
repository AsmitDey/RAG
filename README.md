Local Document RAG Q&A System

A simple Retrieval-Augmented Generation (RAG) application built with Python, LangChain, ChromaDB, and Ollama.

This project lets you provide a PDF or text document, converts the document into searchable chunks, creates embeddings, stores them in ChromaDB, and then answers questions using a local LLM. The application also displays the top retrieved chunks and their similarity distance scores before generating the answer.

Features

📄 Supports PDF and text documents

✂️ Splits documents into chunks using RecursiveCharacterTextSplitter

🧠 Generates embeddings locally with Ollama's nomic-embed-text

🗃️ Stores vectors locally using ChromaDB

🔎 Retrieves the top 3 relevant chunks for each question

🤖 Generates answers using the local llama3.1 Ollama model

🛡️ Uses a grounded prompt that instructs the model to answer only from retrieved context

📊 Displays retrieved chunks, page information, and distance scores

💻 Runs entirely from the command line

How It Works

The application follows a basic RAG pipeline:

             ┌─────────────────┐
             │  PDF / TXT File │
             └────────┬────────┘
                      │
                      ▼
             ┌─────────────────┐
             │ Document Loader │
             └────────┬────────┘
                      │
                      ▼
             ┌─────────────────┐
             │     Chunking    │
             │ 1000 / 200 chars│
             └────────┬────────┘
                      │
                      ▼
             ┌─────────────────┐
             │    Embeddings   │
             │ nomic-embed-text│
             └────────┬────────┘
                      │
                      ▼
             ┌─────────────────┐
             │    ChromaDB     │
             └────────┬────────┘
                      │
              User Question
                      │
                      ▼
             ┌─────────────────┐
             │ Similarity Search│
             │    Top 3 chunks │
             └────────┬────────┘
                      │
                      ▼
             ┌─────────────────┐
             │   Llama 3.1     │
             │ Local LLM       │
             └────────┬────────┘
                      │
                      ▼
                Final Answer

Tech Stack

Python

LangChain

ChromaDB – vector database

Ollama – local model runtime

Llama 3.1 – generation model

nomic-embed-text – embedding model

PyPDF – PDF document loading

Project Structure

.
├── Gen AI RAG.py
├── requirements.txt
├── README.md
└── chroma_db/              # Created automatically after indexing

Requirements

Before running the project, make sure you have:

Python 3.10 or newer

Ollama installed and running

Enough system resources to run the selected local models

The project uses two Ollama models:

llama3.1
nomic-embed-text

Ollama Setup

After installing Ollama, pull the required models:

ollama pull llama3.1
ollama pull nomic-embed-text

Make sure the Ollama service is running before starting the Python application.

You can verify the installed models with:

ollama list

Installation

Clone the repository:

git clone <YOUR_GITHUB_REPOSITORY_URL>
cd <YOUR_REPOSITORY_FOLDER>

Create a virtual environment:

Windows

python -m venv .venv
.venv\Scripts\activate

If PowerShell blocks script execution, you can activate the environment from Command Prompt instead:

.venv\Scripts\activate.bat

Linux / macOS

python3 -m venv .venv
source .venv/bin/activate

Install the Python dependencies:

pip install -r requirements.txt

Usage

Run the application:

python "Gen AI RAG.py"

The program will ask:

Enter path to your document (e.g., sample.pdf):

Enter the path to your PDF or text file.

For example:

sample.pdf

or:

documents/my_notes.pdf

After the document is indexed, the application will display:

RAG SYSTEM READY: Type your question or 'exit' to quit.

You can then ask questions about the document.

Example:

Ask a question: What is the main topic of this document?

The program first performs similarity search and displays the top 3 retrieved chunks. It then generates a grounded response using the retrieved context.

To exit:

exit

You can also use:

quit

or:

q

RAG Configuration

The current implementation uses:

Chunk size

chunk_size=1000

Chunk overlap

chunk_overlap=200

Retrieved chunks

search_kwargs={"k": 3}

The same top-3 retrieval setting is also used when displaying similarity-search results.

Vector database location

CHROMA_PATH = "./chroma_db"

The ChromaDB data is stored locally in the chroma_db directory.

Grounded Answers

The system prompt instructs the LLM to:

Use only the retrieved document context.

Avoid speculation.

Clearly state when the requested information is not available in the document.

This makes the application suitable for document-based question answering where responses should remain tied to the indexed source.

Important Notes

The application is designed for local execution and does not require a cloud LLM API key.

Ollama must be installed and running.

The first model download may take some time because the models need to be downloaded locally.

The chroma_db directory is generated automatically.

If you want a fresh vector database, stop the application and remove the existing chroma_db directory before indexing again.

Text files are loaded using LangChain's TextLoader; PDFs are loaded using PyPDFLoader.

The current code treats every non-.pdf input as a text file.

Example Output

[Step 1: Document Processing] Loading: sample.pdf
-> Successfully loaded 5 document page(s)/section(s).

[Step 2: Chunking]
-> Split into 18 text chunks (Size: 1000 chars, Overlap: 200 chars).

[Step 3 & 4: Embeddings & Vector Database Indexing]
-> Chunks embedded and indexed into ChromaDB at './chroma_db'.

==================================================
 RAG SYSTEM READY: Type your question or 'exit' to quit.
==================================================

Ask a question: What is the document about?

--- Performing Similarity Search ---
[Chunk 1] (Distance Score: ...)
[Chunk 2] (Distance Score: ...)
[Chunk 3] (Distance Score: ...)

--- Generating Grounded Response ---

Final Answer:
...
