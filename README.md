# QueryGPT - Natural Language to SQL Assistant

QueryGPT is an AI-powered assistant that lets users ask questions about a MySQL database in natural language and receive meaningful answers without writing SQL manually.

It uses **Retrieval-Augmented Generation (RAG)** to retrieve relevant database schemas from Pinecone, generate SQL queries, validate them before execution, and summarize query results in plain English. Users can interact with it through the n8n chat interface or a React frontend connected through a webhook.

## Key Features

- **Natural language to SQL:** Ask questions in plain English instead of manually writing MySQL queries.
- **RAG-based schema retrieval:** Retrieves relevant database documentation from Pinecone before generating SQL.
- **Automated schema discovery:** Discovers database tables and extracts their `CREATE TABLE` definitions.
- **Duplicate-resistant schema refresh:** Clears the existing Pinecone namespace before re-indexing to avoid accumulating stale or duplicate schema vectors during a full sync.
- **Metadata-enriched documents:** Attaches table-level metadata to improve document identification and retrieval context.
- **Database overview indexing:** Stores database-level information separately, enabling direct answers to questions such as how many tables exist and which tables are available.
- **Read-oriented query validation:** Checks generated queries against an allowlist-style pattern before execution.
- **Structured result formatting:** Converts raw SQL output into a consistent representation with rows, columns, and row count.
- **Conversational result summaries:** Explains query results in natural language rather than exposing only raw database output.
- **Fallback conversational agent:** Handles greetings, rejected requests, and cases where the system cannot answer confidently from the available context.
- **Multiple chat interfaces:** Supports the n8n chat interface and a React frontend through an HTTP webhook.
- **Consistent API response:** Returns the answer, request type, and generated SQL in a predictable JSON structure.

## Screenshots

### Chat Interface
![QueryGPT Chat Interface](./screenshots/chat-interface.png)

### n8n Workflow
![QueryGPT n8n Workflow](./screenshots/workflow-agent.png)
![QueryGPT n8n Workflow](./screenshots/workflow-upload.png)

## Example Interaction
User: Which customers have placed the most orders?

QueryGPT converts the question into SQL, validates and runs it on MySQL, then explains the results in simple English.
Frontend API Response
The webhook returns a JSON response with three fields: answer, requestType, and sql.

## Technology Stack

| Component | Technology |
|---|---|
| Workflow orchestration | n8n |
| Database | MySQL |
| Vector database | Pinecone |
| Embedding model | Ollama — `nomic-embed-text` |
| Chat model | OpenRouter |
| Retrieval approach | Retrieval-Augmented Generation (RAG) |
| Frontend | React |
| API integration | n8n Webhook |

## Setup and Usage

1. Configure the MySQL connection in n8n.
2. Configure the Pinecone index and credentials.
3. Configure Ollama and download the `nomic-embed-text` embedding model.
4. Configure the OpenRouter chat model credentials.
5. Run the schema indexing workflow to populate Pinecone with table schemas and the database overview.
6. Start the query workflow and test questions through the n8n chat interface.
7. Configure the webhook URL and allowed frontend origin to connect the React application.
8. Re-run schema indexing after structural database changes.

## Safety and Limitations

QueryGPT is designed for read-oriented database access, and its workflow checks generated SQL before execution. For stronger protection, use a dedicated MySQL account with read-only permissions and apply query timeouts and result limits.

The current validator uses a pattern-based check rather than a full SQL parser, so it should not be treated as a complete SQL security boundary. The assistant's answers also depend on the accuracy of the indexed schema and the context retrieved from Pinecone.

## Future Improvements

Potential enhancements include stronger SQL parsing, query timeouts and row limits, more granular retrieval filters, improved error tracking, and automated schema synchronization.

---

QueryGPT brings database discovery, semantic schema retrieval, SQL generation, validation, execution, and conversational responses together in one workflow-driven application.
