# Haystack RAG Implementation

A production-ready Retrieval-Augmented Generation (RAG) system built with Haystack, featuring PDF document ingestion, vector storage with Pinecone, and intelligent question-answering capabilities powered by Meta's Llama model.

## 🚀 Features

- **Document Ingestion**: Upload and process PDF documents with automatic text extraction
- **Intelligent Chunking**: Sentence-based document splitting for optimal retrieval performance
- **Vector Search**: High-performance similarity search using Pinecone vector database
- **LLM Integration**: Powered by Meta-Llama3.2-3B-Instruct model via Hugging Face API
- **Dual Interface**: Both REST API and Streamlit web interface
- **Cloud Storage**: AWS S3 integration for document storage
- **Containerized**: Docker support for easy deployment

## 🏗️ Architecture

The system follows a three-stage RAG pipeline:

### 1. Indexing Pipeline
- **Document Conversion**: PDF to structured text using PyPDFToDocument
- **Text Chunking**: Sentence-based splitting with configurable length
- **Embedding Generation**: Vector embeddings using SentenceTransformers
- **Vector Storage**: Embeddings stored in Pinecone for fast retrieval

### 2. Retrieval Pipeline  
- **Query Embedding**: Input queries converted to vector representations
- **Similarity Search**: Pinecone retriever finds most relevant document chunks
- **Context Assembly**: Retrieved documents prepared for generation

### 3. Generation Pipeline
- **Prompt Construction**: Custom template integrates query with retrieved context
- **LLM Generation**: Meta-Llama model generates contextually aware responses
- **Response Formatting**: Clean, structured answers delivered to users

## 📋 Prerequisites

- Python 3.8+
- Docker (optional)
- AWS Account (for S3 storage)
- Pinecone Account
- Hugging Face Account

## 🔧 Environment Setup

1. **Clone the repository**
   ```bash
   git clone <repository-url>
   cd sathyaneu-haystack-rag-implementation
   ```

2. **Create environment file**
   ```bash
   cp .env.example .env
   ```

3. **Configure environment variables**
   ```env
   # Pinecone Configuration
   PINECONE_API_KEY=your_pinecone_api_key
   
   # Hugging Face Configuration
   HF_TOKEN=your_huggingface_token
   
   # AWS Configuration
   AWS_ACCESS_KEY_ID=your_aws_access_key
   AWS_SECRET_ACCESS_KEY=your_aws_secret_key
   AWS_REGION=us-east-1
   ```

4. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## 🚦 Quick Start

### Option 1: Local Development

1. **Start the FastAPI server**
   ```bash
   uvicorn app:app --host 0.0.0.0 --port 8000 --reload
   ```

2. **Launch Streamlit interface** (in a new terminal)
   ```bash
   streamlit run streamlit-app.py --server.port=8501 --server.address=0.0.0.0
   ```

3. **Access the application**
   - Streamlit UI: http://localhost:8501
   - FastAPI docs: http://localhost:8000/docs

### Option 2: Docker Deployment

1. **Build the Docker image**
   ```bash
   docker build -t haystack-rag .
   ```

2. **Run the container**
   ```bash
   docker run -p 8000:8000 -p 8501:8501 --env-file .env haystack-rag
   ```

## 📖 Usage Guide

### Web Interface (Streamlit)

1. **Upload PDF Documents**
   - Navigate to the indexing section
   - Select and upload PDF files
   - Click "Index PDF" to process and store embeddings

2. **Ask Questions**
   - Enter your question in the text input
   - Click "Submit" to get AI-generated answers
   - Responses are based on your uploaded documents

### REST API

#### Index a PDF Document
```bash
curl -X POST "http://localhost:8000/index_pdf" \
     -F "file=@document.pdf"
```

#### Query the System
```bash
curl -X POST "http://localhost:8000/get_result" \
     -H "Content-Type: application/json" \
     -d '{"query": "What is the main topic of the document?"}'
```

## 🛠️ API Reference

### Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| POST | `/index_pdf` | Upload and index a PDF document |
| POST | `/get_result` | Submit a query and receive an answer |

### Request/Response Examples

**Index PDF**
```json
// Response
{
  "message": "PDF indexed successfully from https://haystack-docs.s3.us-east-1.amazonaws.com/uploads/uuid.pdf"
}
```

**Query System**
```json
// Request
{
  "query": "What are the key findings?"
}

// Response
{
  "answer": "Based on the provided documents, the key findings include..."
}
```

## 🔧 Configuration

### Pinecone Setup
- Index name: `haystack`
- Namespace: `default` 
- Dimension: `768` (matches SentenceTransformers embeddings)

### Model Configuration
- **Embeddings**: `sentence-transformers/all-MiniLM-L6-v2`
- **LLM**: `meta-llama/Llama-3.2-3B-Instruct`
- **Chunking**: Sentence-based, 2 sentences per chunk

## 🏭 Production Considerations

### Scaling
- Use Pinecone's serverless tier for automatic scaling
- Deploy FastAPI with ASGI servers like Gunicorn + Uvicorn
- Implement caching for frequently asked questions

### Security
- Secure API endpoints with authentication
- Encrypt sensitive environment variables
- Use IAM roles for AWS S3 access

### Monitoring
- Implement logging for all pipeline stages
- Monitor API response times and error rates
- Track vector database performance metrics

## 🤔 Why Haystack over LangChain?

- **Better Documentation**: More comprehensive and user-friendly guides
- **End-to-End Integration**: Seamless RAG pipeline components
- **Production Ready**: Lower barriers to entry and higher reliability
- **Performance**: Consistently higher accuracy and lower response deviation
- **Simplicity**: Ideal for straightforward RAG implementations

## 🛡️ Troubleshooting

### Common Issues

**Environment Variables Not Loading**
```bash
# Ensure .env file is in the project root
# Verify variable names match exactly
python -c "import os; print(os.getenv('PINECONE_API_KEY'))"
```

**Pinecone Connection Issues**
```bash
# Verify API key and index configuration
# Check if index exists and has correct dimensions
```

**S3 Upload Failures**
```bash
# Verify AWS credentials and bucket permissions
# Ensure bucket exists in the specified region
```

## 📦 Dependencies

### Core Libraries
- `haystack-ai`: Core RAG framework
- `pinecone-haystack`: Vector database integration
- `fastapi`: REST API framework
- `streamlit`: Web interface
- `sentence-transformers`: Embedding models

### Cloud Services
- `boto3`: AWS S3 integration
- `huggingface-hub`: Model access

---

**Built with ❤️ using Haystack RAG Framework**
