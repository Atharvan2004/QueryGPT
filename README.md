# QueryGPT

## Introduction

Built QueryGPT, a fully local AI assistant that lets users query a MySQL database in plain English instead of writing SQL. It understands the database schema through a vector store, generates safe read-only SQL queries, executes them, and explains the results in a conversational, easy-to-understand format.

**Tools:** Ollama (Qwen3:4B), n8n, MySQL, Pinecone Vector Store, Nomic Embed

## Description

The project uses a two-workflow architecture. The first workflow automatically indexes database schemas by extracting `CREATE TABLE` statements, generating semantic embeddings with Nomic Embed, and storing them in Pinecone Vector Store.

The second workflow retrieves the relevant schemas for a user's question, generates optimized read-only MySQL queries using retrieved schemas, validates query safety, executes the SQL and summarizes them in natural language.

## Key Features

1. Natural language to SQL conversion using a local Ollama-powered AI agent.
2. RAG using Pinecone Vector Store for semantic schema retrieval.
3. Automatically indexes MySQL table schemas using `SHOW CREATE TABLE` for semantic retrieval.
4. Validates every query to ensure only safe, read-only SQL is executed (`SELECT`, `SHOW`, `DESCRIBE`).
5. Formats raw SQL results into clean, conversational responses that are easy to understand.
6. Has a fallback conversational agent for generic non-SQL related questions.

![QueryGPT Workflow](/assets/image1.png)

![QueryGPT Agent](/assets/image2.png)

## 1. Upload Schemas Workflow

1. Retrieves schemas (`SHOW CREATE TABLE`) for every table in the connected MySQL database.
2. Generates embeddings using Nomic Embed through Ollama.
3. Stores embeddings and metadata inside Pinecone Vector Store.

## 2. QueryGPT Agent Workflow

1. Accepts natural language database questions through an n8n chat trigger.
2. Retrieves the most relevant table schemas from Pinecone using semantic search.
3. Generates a schema-aware MySQL query using Qwen3 LLM.
4. Validates that the query is read-only and executable before running it.
5. Executes the query and formats the results into a consistent structure.
6. Produces a concise, human-readable summary of the query output.

## Technical Architecture

### Schema Indexing Pipeline

```text
MySQL Database
      │
      ▼
Extract schemas using SHOW CREATE TABLE
      │
      ▼
Nomic Embed (Ollama)
      │
      ▼
Pinecone Vector Store
```

### Query Pipeline

```text
User Query
      │
      ▼
Pinecone Schema Retrieval
      │
      ▼
Qwen3 SQL Generator
      │
      ▼
SQL Validation
      │
      ▼
Execute MySQL Query
      │
      ▼
Result Formatter
      │
      ▼
AI Result Summarizer
```

## Use Cases

* SQL assistant for non-technical users - Ask questions in plain English without knowing SQL syntax.
* Business analytics on demand - Quickly fetch customer, sales, product, or order insights using conversational queries.
* Database exploration - Discover tables, columns, and relationships without manually browsing the schema.
* Developer productivity - Save time writing and validating SQL by letting the AI generate optimized read-only queries.
* Safe database querying - Interact with production or enterprise databases without risking accidental data modification.
