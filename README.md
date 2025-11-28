# AI-Powered Technical Documentation Assistant

An intelligent question-answering system that helps engineers and technicians quickly extract information from technical manuals. Instead of manually searching through hundreds of pages, users can ask questions in natural language and receive accurate, cited answers in seconds.

[![Live Demo](https://img.shields.io/badge/🚀_Try_Live_Demo-Streamlit-FF4B4B?style=for-the-badge)](https://rag-chatbot-gpt5series.streamlit.app/)
[![Python](https://img.shields.io/badge/Python-3.11-blue?style=flat-square)](https://www.python.org/)
[![LangChain](https://img.shields.io/badge/LangChain-0.2.16-green?style=flat-square)](https://github.com/langchain-ai/langchain)

---

## 🎯 The Problem

Engineers working with technical documentation face a common challenge:
- **Manual search is slow**: Finding specific information in a 500-page manual takes hours
- **Keywords fail**: Traditional Ctrl+F can't understand context or related concepts
- **Multiple documents**: Information is scattered across different manuals and versions

**This system solves that** by using AI to understand your questions and retrieve the exact information you need, complete with source references.

---

## ✨ What This Does

Upload any technical PDF (user manuals, engineering specifications, maintenance guides), and the system:

1. **Breaks down the document** into searchable chunks while preserving context
2. **Understands your questions** using semantic search (meaning-based, not just keywords)
3. **Retrieves relevant sections** from the document that contain the answer
4. **Generates clear answers** using GPT-5, grounded in the retrieved content
5. **Shows you the sources** so you can verify the information

**Example Use Cases:**
- "What are the torque specifications for the front axle assembly?"
- "Explain the troubleshooting steps for error code E-42"
- "List all safety warnings mentioned in Section 3"

---

## 🚀 Live Demo

**Try it now:** [https://rag-chatbot-gpt5series.streamlit.app/](https://rag-chatbot-gpt5series.streamlit.app/)

1. Upload a technical PDF document
2. Select your preferred GPT-5 model
3. Ask questions in plain English
4. View answers with source citations

---

## 🏗️ How It Works (RAG Architecture)

This system uses **Retrieval-Augmented Generation (RAG)**, which combines:
- **Retrieval**: Finds relevant information from your document
- **Generation**: Uses AI to create natural language answers

### Architecture Flow

```
┌─────────────┐
│  Upload PDF │
└──────┬──────┘
       │
       ▼
┌──────────────────────────┐
│ Split into 1,000-token   │  ← Preserves context between chunks
│ chunks (250 overlap)     │
└──────┬───────────────────┘
       │
       ▼
┌──────────────────────────┐
│ Convert to vector        │  ← Creates searchable embeddings
│ embeddings (ChromaDB)    │
└──────┬───────────────────┘
       │
       ▼
┌──────────────────────────┐
│ User asks question       │
└──────┬───────────────────┘
       │
       ▼
┌──────────────────────────┐
│ Find top-4 most          │  ← Semantic search
│ relevant chunks          │
└──────┬───────────────────┘
       │
       ▼
┌──────────────────────────┐
│ GPT-5 generates answer   │  ← Only uses retrieved context
│ with source citations    │
└──────────────────────────┘
```

**Why RAG instead of just asking GPT-5 directly?**
- ✅ **Accurate**: Answers grounded in your specific document
- ✅ **Up-to-date**: Works with proprietary or recent documents GPT-5 hasn't seen
- ✅ **Verifiable**: Shows exact sources for fact-checking
- ✅ **No hallucination**: AI can't make up information not in the document

---

## 🛠️ Tech Stack

| Component | Technology | Purpose |
|-----------|-----------|---------|
| **LLM** | OpenAI GPT-5 (Mini/Nano/5/5.1) | Natural language understanding and generation |
| **Framework** | LangChain 0.2.16 | RAG orchestration and chain management |
| **Vector Database** | ChromaDB | Stores document embeddings for fast semantic search |
| **Embeddings** | OpenAI text-embedding-ada-002 | Converts text to vector representations |
| **UI** | Streamlit | Interactive web interface |
| **PDF Processing** | PyPDFLoader | Extracts text from uploaded documents |
| **Deployment** | Streamlit Community Cloud | Free cloud hosting with auto-scaling |

---

## 🎯 Key Features

### 1. **Semantic Search**
Unlike keyword search, the system understands meaning:
- Query: "How do I fix overheating?"
- Finds sections about: "thermal management," "cooling procedures," "temperature errors"

### 2. **Multi-Model Support**
Choose the right model for your needs:
- **GPT-5-Nano**: Fastest, cheapest (simple queries)
- **GPT-5-Mini**: Balanced speed and accuracy
- **GPT-5**: High-quality answers
- **GPT-5.1**: Complex reasoning and multi-step questions

### 3. **Citation System**
Every answer shows the source text it came from:
- View the exact chunks used to generate the answer
- Verify accuracy against the original document
- Build trust with transparent AI

### 4. **Session Management**
- Maintains conversation history within a session
- Upload multiple documents
- Clear context to start fresh

---

## 💻 Installation & Local Setup

### Prerequisites
- Python 3.11 or higher
- OpenAI API key ([Get one here](https://platform.openai.com/api-keys))

### Step 1: Clone Repository
```bash
git clone https://github.com/YounusVersiani/ai-powered-technical-documentation-assistant.git
cd ai-powered-technical-documentation-assistant
```

### Step 2: Create Virtual Environment
```bash
# Windows
python -m venv venv
venv\Scripts\activate

# macOS/Linux
python3 -m venv venv
source venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 4: Configure API Key
Create a `.env` file in the project root:
```bash
OPENAI_API_KEY=sk-your-api-key-here
```

### Step 5: Run Application
```bash
streamlit run app.py
```

Open browser to: `http://localhost:8501`

---

## 📊 Performance Metrics

| Metric | Value | Details |
|--------|-------|---------|
| **Chunking** | 1,000 tokens | Optimal balance between context and retrieval precision |
| **Chunk Overlap** | 250 tokens | Prevents information loss at chunk boundaries |
| **Retrieval** | Top-4 chunks | Provides sufficient context without token waste |
| **Indexing Speed** | <5 seconds | For typical 50-200 page technical manuals |
| **Max File Size** | 200 MB | Streamlit Cloud limitation |

---

## 📁 Project Structure

```
ai-powered-technical-documentation-assistant/
├── app.py                 # Main Streamlit application
│   ├── PDF upload and processing
│   ├── Vector database management
│   ├── Chat interface
│   └── Citation display
├── requirements.txt       # Python dependencies
├── .env                   # API keys (not in repo)
├── .gitignore            # Excludes temp files, vector DB, venv
└── README.md             # This file
```

---

## 🔧 Technical Implementation Details

### Document Processing
```python
# Split documents into overlapping chunks
RecursiveCharacterTextSplitter(
    chunk_size=1000,      # ~750 words
    chunk_overlap=250     # Preserves context
)
```

### Vector Storage
```python
# Persistent ChromaDB with OpenAI embeddings
Chroma.from_documents(
    documents=splits,
    embedding=OpenAIEmbeddings(),
    persist_directory="./chroma_db"
)
```

### RAG Chain
```python
# LangChain retrieval chain with custom prompt
retriever = vector_db.as_retriever(search_kwargs={"k": 4})
rag_chain = create_retrieval_chain(retriever, question_answer_chain)
```

### Custom System Prompt
The AI is instructed to act as a **"Technical Solutions Architect"** to:
- Maintain professional engineering tone
- Provide concise, accurate answers
- Acknowledge when information is missing
- Focus on technical accuracy over conversational flair

---

## 🚀 Deployment on Streamlit Cloud

This app is deployed on Streamlit Community Cloud (free tier):

**Deployment Steps:**
1. Push code to GitHub
2. Connect Streamlit Cloud to repository
3. Add `OPENAI_API_KEY` in Streamlit secrets
4. Auto-deploys on every push to `main`

**Deployment URL:** [https://rag-chatbot-gpt5series.streamlit.app/](https://rag-chatbot-gpt5series.streamlit.app/)

---

## 🎓 Use Cases

### Manufacturing
- Query PCB assembly manuals for defect troubleshooting
- Extract safety protocols from equipment documentation
- Find maintenance schedules and part specifications

### Automotive
- Search ADAS system documentation for calibration procedures
- Retrieve diagnostic trouble code (DTC) explanations
- Access repair instructions for specific components

### Aerospace
- Query flight manual procedures and checklists
- Extract technical specifications from engineering drawings
- Find compliance requirements in regulatory documents

### General Engineering
- Search API documentation for function descriptions
- Extract design guidelines from standards documents
- Find test procedures in validation protocols

---

## 🔮 Future Enhancements

- [ ] **Multi-document cross-referencing**: Query across multiple uploaded manuals
- [ ] **Table and diagram extraction**: Better handling of structured data
- [ ] **Conversation export**: Download Q&A history as PDF report
- [ ] **Fine-tuned embeddings**: Domain-specific models for technical terminology
- [ ] **Advanced filters**: Search by document section, date, or metadata
- [ ] **Batch processing**: Upload and index multiple documents at once

---

## 🤝 Contributing

Contributions welcome! Areas for improvement:
- Support for additional file formats (DOCX, HTML, Markdown)
- Better visualization of retrieved chunks
- Performance optimization for large documents
- Multi-language support

---

## 📄 License

MIT License - See LICENSE file for details

---

## 👨‍💻 Author

**Younus Versiani**  
Autonomous Vehicle Engineering Student  
Technische Hochschule Ingolstadt

[![GitHub](https://img.shields.io/badge/GitHub-YounusVersiani-black?style=flat-square&logo=github)](https://github.com/YounusVersiani)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-younusversiani-blue?style=flat-square&logo=linkedin)](https://linkedin.com/in/younusversiani)

---

## 🙏 Acknowledgments

- **LangChain** for the RAG framework
- **OpenAI** for GPT-5 API and embeddings
- **Streamlit** for rapid prototyping and free hosting
- **ChromaDB** for efficient vector storage

---

**⭐ If you find this project useful, consider starring the repository!**
