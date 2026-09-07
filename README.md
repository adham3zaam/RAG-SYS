# 📄 RAG-Based PDF Question Answering System

An AI-powered **Retrieval-Augmented Generation (RAG)** system that allows users to upload PDF documents and ask questions about their content.

The application combines **LangChain, FAISS, HuggingFace Sentence Transformers, Groq LLM, and Streamlit** to retrieve relevant information from documents and generate accurate, context-grounded answers with source page references.

---

## 🚀 Overview

This project demonstrates how **Retrieval-Augmented Generation (RAG)** can be used to build a document question-answering system.

Instead of asking the Large Language Model to answer directly from its internal knowledge, the system first searches the uploaded PDF for relevant information and then provides that information to the LLM as context.

### Workflow

```text
PDF Document
     ↓
Text Extraction
     ↓
Text Cleaning
     ↓
Text Chunking
     ↓
HuggingFace Embeddings
     ↓
FAISS Vector Database
     ↓
User Question
     ↓
Similarity Search
     ↓
Relevant Document Chunks
     ↓
Groq LLM
     ↓
Context-Grounded Answer
     ↓
Source Pages
```

---

## ✨ Features

* 📄 Upload PDF documents
* 🔍 Semantic search over document content
* 🧩 Automatic document chunking
* 🧠 HuggingFace sentence embeddings
* ⚡ Fast similarity search using FAISS
* 🤖 Large Language Model integration using Groq
* 🔗 Retrieval-Augmented Generation (RAG)
* 🛡️ Context-grounded answers
* 📚 Source page tracking
* 🖥️ Interactive Streamlit interface
* ⚙️ Configurable number of retrieved chunks

---

## 🛠️ Technologies Used

* **Python**
* **LangChain**
* **LangChain Community**
* **LangChain Groq**
* **LangChain HuggingFace**
* **FAISS**
* **HuggingFace Sentence Transformers**
* **PyPDF**
* **Groq API**
* **Streamlit**
* **python-dotenv**

---

## 🧠 Models

### LLM

The project uses the following Groq-hosted model:

```text
openai/gpt-oss-20b
```

The model is configured with:

```python
temperature = 0
```

This helps produce more deterministic and consistent responses.

### Embedding Model

The project uses:

```text
sentence-transformers/all-MiniLM-L6-v2
```

The embedding model converts document chunks and user questions into numerical vectors that can be compared using semantic similarity.

---

## 🔎 How RAG Works

The RAG pipeline consists of two main stages:

### 1. Retrieval

The uploaded PDF is processed and divided into smaller chunks.

Each chunk is converted into an embedding and stored inside a **FAISS vector database**.

When the user asks a question, the question is also converted into an embedding.

FAISS then searches for the most semantically similar document chunks.

### 2. Generation

The retrieved chunks are provided to the Groq LLM as context.

The LLM generates an answer based only on the retrieved information.

This helps reduce hallucinations and keeps the answer grounded in the uploaded document.

---

## 📄 PDF Processing

The project uses `PyPDFLoader` to extract text from PDF documents.

The extracted text is cleaned before being divided into chunks.

```python
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)
```

### Chunking Strategy

* **Chunk Size:** 1000 characters
* **Chunk Overlap:** 200 characters

The overlap helps preserve context between neighboring chunks.

---

## 🧠 Embeddings

HuggingFace Sentence Transformers are used to create semantic vector representations of the document chunks.

```python
embeddings = HuggingFaceEmbeddings(
    model_name="sentence-transformers/all-MiniLM-L6-v2"
)
```

These embeddings allow the system to perform semantic search instead of relying only on keyword matching.

---

## ⚡ FAISS Vector Database

FAISS is used for efficient similarity search.

```python
vectorstore = FAISS.from_documents(
    chunks,
    embeddings
)
```

The vector store is then converted into a retriever:

```python
retriever = vectorstore.as_retriever(
    search_kwargs={"k": 3}
)
```

The retriever returns the most relevant document chunks for the user's question.

---

## 🤖 LLM Integration

The system uses Groq through LangChain:

```python
from langchain_groq import ChatGroq

llm = ChatGroq(
    model="openai/gpt-oss-20b",
    temperature=0
)
```

The retrieved document context is passed to the LLM together with the user's question.

---

## 🛡️ Hallucination Reduction

The prompt instructs the model to use only the information retrieved from the PDF.

The system follows these rules:

```text
- Use only the provided context.
- Do not invent or guess information.
- If the answer is not contained in the context,
  say that there is not enough information in the document.
- Answer clearly and concisely.
- Mention relevant page numbers when possible.
```

This makes the application more reliable for document-based question answering.

---

## 📚 Source Tracking

The application keeps track of the PDF pages associated with retrieved chunks.

For example:

```text
Answer:

The document discusses Machine Learning, Deep Learning,
Computer Vision, and Generative AI.

Sources:

Page 1
Page 2
```

This allows users to verify the information against the original document.

---

## 🖥️ Streamlit Interface

The project provides a simple web interface using Streamlit.

Users can:

1. Upload a PDF.
2. Wait for the document to be processed.
3. Enter a question.
4. Retrieve relevant information.
5. Generate an answer using the LLM.
6. View the source pages.

Run the application with:

```bash
streamlit run app.py
```

---

## 📂 Project Structure

```text
RAG-PDF-Question-Answering/
│
├── app.py
├── requirements.txt
├── .env.example
├── .gitignore
└── README.md
```

---

## 📦 Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/RAG-PDF-Question-Answering.git
```

Navigate to the project directory:

```bash
cd RAG-PDF-Question-Answering
```

Install the required packages:

```bash
pip install -r requirements.txt
```

---

## 🔑 API Key Setup

This project requires a **Groq API Key**.

Create a `.env` file in the project directory:

```env
GROQ_API_KEY=your_groq_api_key
```

The application loads the API key using:

```python
from dotenv import load_dotenv
import os

load_dotenv()

api_key = os.getenv("GROQ_API_KEY")
```

### ⚠️ Security

Never upload your `.env` file or API key to GitHub.

Make sure `.gitignore` contains:

```text
.env
```

---

## ▶️ Run the Application

After configuring the API key, run:

```bash
streamlit run app.py
```

Then open the Streamlit URL displayed in the terminal.

---

## 💬 Example Questions

After uploading a PDF, users can ask questions such as:

```text
What is this document about?

What skills are mentioned in the document?

What technologies are used?

What projects are discussed?

What experience does the candidate have?

What are the main topics discussed in the document?
```

---

## 📊 Example RAG Pipeline

```text
User
 │
 │ Upload PDF
 ↓
PyPDFLoader
 │
 │ Extract Text
 ↓
Text Cleaning
 │
 │ Split Document
 ↓
RecursiveCharacterTextSplitter
 │
 │ Generate Embeddings
 ↓
HuggingFace Sentence Transformers
 │
 │ Store Vectors
 ↓
FAISS
 │
 │ Similarity Search
 ↓
Relevant Chunks
 │
 │ Add Context
 ↓
Prompt Template
 │
 │ Generate Response
 ↓
Groq LLM
 │
 ↓
Answer + Source Pages
```

---

## 🎯 Project Objectives

The main objectives of this project are:

* Build a practical RAG application.
* Understand semantic search and vector embeddings.
* Implement document retrieval using FAISS.
* Integrate Large Language Models into real-world applications.
* Reduce LLM hallucinations through context grounding.
* Build an interactive AI application using Streamlit.
* Provide traceable answers with document page references.

---

## 📚 Learning Outcomes

Through this project, I gained practical experience in:

* Retrieval-Augmented Generation (RAG)
* Large Language Models (LLMs)
* Natural Language Processing (NLP)
* Semantic Search
* Vector Embeddings
* Vector Databases
* FAISS
* LangChain
* HuggingFace Sentence Transformers
* Groq API
* Prompt Engineering
* PDF Document Processing
* Streamlit Application Development

---

## 🔮 Future Improvements

Future versions of the project can include:

* 📚 Multiple PDF support
* 💬 Chat history
* 🧠 Multi-turn conversations
* 🔍 Advanced retrieval and reranking
* 📊 Retrieval evaluation metrics
* 📈 Similarity score visualization
* 🗂️ Persistent document collections
* 📄 Support for DOCX and TXT files
* 🌐 Cloud deployment
* 🔐 Improved API key management
* 🧪 Automated RAG evaluation
* 🎯 Hybrid keyword + semantic search

---

## 👨‍💻 Author

### Adham Azzam

Computer Science / Artificial Intelligence Student

### Areas of Interest

* Machine Learning
* Deep Learning
* Computer Vision
* Generative AI
* Natural Language Processing
* Large Language Models
* Retrieval-Augmented Generation

---

## ⭐ Conclusion

This project demonstrates a complete **Retrieval-Augmented Generation pipeline** that combines document processing, semantic embeddings, vector search, and Large Language Models.

By using **LangChain, HuggingFace Embeddings, FAISS, Groq, and Streamlit**, the system provides an efficient and practical way to interact with PDF documents through natural language questions.

The project highlights the practical application of **Generative AI and RAG architectures** in building intelligent document-based question answering systems.
# RAG-SYS
