---
description: "Build Retrieval-Augmented Generation pipelines with document loaders, embeddings, vector stores, and retrieval chains. Use when implementing search, document Q&A, or knowledge-base features. Critical for Lawlyfy AI and any product with document retrieval."
---

# LangChain RAG

## When to use

When building any feature that retrieves documents and uses them as context for LLM responses. This includes: legal case search (Lawlyfy), knowledge bases, document Q&A, semantic search, and chat-with-your-data features.

## Pipeline architecture

```
Documents → Loader → Splitter → Embeddings → Vector Store → Retriever → Chain → Response
```

## Document loading

```python
from langchain_community.document_loaders import PyPDFLoader, TextLoader, CSVLoader

loader = PyPDFLoader("document.pdf")
docs = loader.load()
```

## Text splitting

```python
from langchain_text_splitters import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=["\n\n", "\n", ". ", " "]
)
chunks = splitter.split_documents(docs)
```

## Embeddings and vector store

```python
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import Chroma

embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(chunks, embeddings)
retriever = vectorstore.as_retriever(search_kwargs={"k": 5})
```

## Retrieval chain

```python
from langchain.chains import create_retrieval_chain
from langchain.chains.combine_documents import create_stuff_documents_chain

combine_chain = create_stuff_documents_chain(llm, prompt)
rag_chain = create_retrieval_chain(retriever, combine_chain)
result = rag_chain.invoke({"input": "What is the ruling on...?"})
```

## Hybrid retrieval (vector + keyword)

For production RAG (recommended for Lawlyfy):

```python
from langchain.retrievers import EnsembleRetriever
from langchain_community.retrievers import BM25Retriever

bm25 = BM25Retriever.from_documents(docs, k=5)
ensemble = EnsembleRetriever(
    retrievers=[vectorstore.as_retriever(), bm25],
    weights=[0.6, 0.4]
)
```

## Anti-patterns

- Do NOT embed entire documents without splitting. Chunk size matters.
- Do NOT skip overlap. Zero overlap loses context at chunk boundaries.
- Do NOT use cosine similarity alone for legal/factual retrieval. Hybrid (vector + BM25) performs better.
- Do NOT skip reranking for production. Use Cohere reranker or cross-encoder.
