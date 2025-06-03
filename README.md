# RAG-Powered AI Agent with n8n

This repository contains an n8n workflow-powered RAG (Retrieval Augmented Generation) AI Agent designed to provide contextual answers based on a custom knowledge base. This project demonstrates how to build a powerful AI agent for document Q&A without writing extensive code, leveraging n8n's visual workflow automation capabilities.

## Project Overview

The RAG AI Agent addresses the common challenge of AI models lacking specific, up-to-date knowledge about private or niche documents. By integrating a retrieval mechanism, the agent can fetch relevant information from a vector store (Pinecone) before generating a response, ensuring answers are accurate, context-aware, and aligned with the provided data.

## Features

* **Contextual Q&A:** Answers user queries based on information retrieved from a custom knowledge base.
* **n8n Workflows:** Utilizes n8n for seamless automation and integration of AI services.
* **Vector Store Integration:** Employs Pinecone as the vector database for efficient semantic search and retrieval.
* **OpenAI Models:** Leverages OpenAI's chat models for natural language understanding and generation, and embedding models for converting text into vectors.
* **Recursive Character Text Splitter:** Efficiently breaks down documents into manageable chunks for embedding.
* **Google Drive Integration:** Automates the loading of documents from Google Drive into the vector store.

## How It Works (Architecture)

The RAG AI Agent operates in two main phases, each handled by a dedicated n8n workflow:

### 1. Data Loading Workflow (RAG Agent 2.json)

This workflow is responsible for preparing and uploading your documents to the vector store.

* **Trigger:** Manually triggered (e.g., when new documents need to be added or updated).
* **Google Drive Node:** Downloads documents from a specified Google Drive folder.
* **Recursive Character Text Splitter:** Divides the downloaded documents into smaller, semantically coherent chunks to optimize for embedding and retrieval.
* **OpenAI Embeddings:** Converts each text chunk into a high-dimensional vector representation.
* **Pinecone Vector Store:** Uploads these vectors along with their original text content to your Pinecone index, creating your searchable knowledge base.

![Data Loading Workflow](images/RAG%20AI%20Agent%202.png)

### 2. Chat/Query Workflow (RAG based AI Agent.json)

This workflow handles user queries and retrieves relevant information before generating a response.

* **Trigger:** Activated when a chat message is received (e.g., via a webhook or n8n chat UI).
* **OpenAI Embeddings:** Converts the incoming user query into a vector.
* **Pinecone Vector Store (Query):** Uses the query vector to search the knowledge base in Pinecone, retrieving the most relevant document chunks.
* **OpenAI Chat Model:** Combines the original user query with the retrieved context from Pinecone and a system prompt to instruct the AI model to generate a well-informed answer.
* **Response:** Delivers the contextual answer back to the user.

![Chat/Query Workflow](images/RAG%20AI%20Agent%201.png)

## Setup and Installation

To set up and run this RAG AI Agent, you will need:

1.  **n8n Instance:** A running n8n instance (cloud or self-hosted).
2.  **OpenAI API Key:** For accessing OpenAI's embedding and chat models.
3.  **Pinecone Account & API Key:** A Pinecone index set up to store your document embeddings.
4.  **Google Cloud Credentials:** Configured for Google Drive access (if using Google Drive for document loading).

### Steps:

1.  **Clone this repository:**
    ```bash
    git clone [https://github.com/Muneeb20019/RAG-Agent-n8n.git](https://github.com/Muneeb20019/RAG-Agent-n8n.git)
    ```
2.  **Import n8n Workflows:**
    * Open your n8n instance.
    * Import `RAG Agent 2.json` and `RAG based AI Agent.json` into your n8n workspace.
3.  **Configure Credentials:**
    * In both n8n workflows, update the OpenAI, Pinecone, and Google Drive (if applicable) credentials with your own API keys/secrets.
4.  **Prepare Pinecone Index:**
    * Ensure you have a Pinecone index created (e.g., with 1536 dimensions for OpenAI `text-embedding-ada-002` embeddings).
5.  **Load Data (Data Loading Workflow):**
    * Configure the `Google Drive` node in `RAG Agent 2.json` to point to your document folder.
    * Execute `RAG Agent 2.json` to load your documents into Pinecone.
6.  **Activate Chat Workflow:**
    * Activate the `RAG based AI Agent.json` workflow.
    * Set up its trigger (e.g., a webhook URL if integrating with another application, or use n8n's built-in chat UI for testing).
7.  **Start Querying:** Begin sending queries to your RAG AI Agent!

## Files in this Repository

* `RAG Agent 2.json`: n8n workflow for loading data into the Pinecone vector store.
* `RAG based AI Agent.json`: n8n workflow for handling chat queries and retrieving contextual answers.
* `README.md`: This project overview and guide.
* `images/`: Contains screenshots of the n8n workflows.
    * `RAG AI Agent 1.png`
    * `RAG AI Agent 2.png`

## Author

[Muneeb Ali Khan]    (Your_Profile_URL_Here)

## License

This project is open-source and available under the MIT License.
