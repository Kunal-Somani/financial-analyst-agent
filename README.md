# 💰 Project 1: Advanced Financial Trend Analyst Agent
### Dual-Tool Agentic Reasoning & Custom API Integration

This project demonstrates proficiency in building complex, self-hosted AI Agents capable of dynamic tool selection between quantitative database analysis (SQL) and qualitative compliance retrieval (RAG). This fully satisfies all application requirements regarding **n8n (self-hosted)**, **LLM-RAG**, and **custom API integration**.

## 🌟 Project Summary (For Resume / Portfolio)

| Feature | Technical Implementation |
| :--- | :--- |
| **Custom Dual-Tool Agent** | Developed a Gemini 2.5 Pro Agent that performs dynamic reasoning to select between a **Custom SQL Executor** (for data analysis/calculations) and a **Custom Python RAG Retriever** (for policy documents). |
| **Advanced RAG & Tooling** | Built a robust RAG stack using **Ollama/PGVector** with custom **Python Code Node chunking** for text pre-processing, ensuring superior RAG accuracy and bypassing standard node limitations. |
| **API Integration** | Implemented a **Custom SQL Executor** for database interaction and utilized the **Telegram API** for real-time reporting, demonstrating advanced API control over the workflow. |

---

## 🛠️ Technical Stack & Architecture

| Component | Tool / Technology | Purpose |
| :--- | :--- | :--- |
| **Workflow Orchestration** | **n8n (Self-Hosted)** | Manages the end-to-end multi-step workflow. |
| **LLM / Reasoning** | **Google Gemini 2.5 Pro** | Core decision-making engine for tool selection and summarization. |
| **Database & Vector Store** | **PostgreSQL** with **pgvector** | Stores both transactional data (SQL) and policy embeddings (RAG). |
| **Embeddings** | **Ollama** (`nomic-embed-text:latest`) | Generates vectors for RAG document indexing. |

---

## 📝 RAG Ingestion Pipeline (Step 1: Data Preparation)

This workflow runs first to prepare the knowledge base, ensuring the Agent has data to retrieve:

1.  **File Read & Extract:** Reads the policy file from a dedicated Docker volume (`/rag_data`) and converts the binary output to a usable text string.
2.  **Custom Chunking:** Uses a **Custom Python Code Node** for text splitting (by double newline) to ensure logical context is preserved in each chunk.
3.  **Loading:** Inserts chunks and Ollama-generated vectors into the **`financial_policy_rag`** table (addressing the column schema mismatch by renaming `content` to `text`).

---

## 🧠 Agentic Reasoning Workflow (Step 2: The Service)

This workflow operates continuously, using the data prepared above to service user requests.

### **Mechanism**

The Agent uses a structured **System Prompt** to analyze the user's query and trigger one of two tools based on intent:

* **Quantitative Query:** $\rightarrow$ Calls the **`sql_query_executor`** tool, forcing the LLM to generate raw SQL (`SELECT...`).
* **Policy Query:** $\rightarrow$ Calls the **`custom_rag_retriever`** tool (Python), which performs a vector similarity search in the PGVector store.

### **Final Output**

* The final synthesized summary is automatically delivered to the user's Telegram chat via the **Telegram API** (the external service integration).

---

## 🚀 Setup & Local Deployment

To run this project, follow these steps:

1.  **Clone Repository:** `git clone [repository URL]`
2.  **Dependencies:** Ensure **Docker Desktop** and **Ollama** (`nomic-embed-text:latest`) are running locally.
3.  **Start Services:** Run `docker compose up -d` in the project root.
4.  **Configure Credentials:** Add your **Gemini API Key**, **Postgres**, and **Telegram Bot Token** in the n8n Credentials UI.
5.  **Run Ingestion:** Execute the **`RAG Ingestion: Financial Policy Load`** workflow once to populate the database.
6.  **Activate Agent:** The **`Financial Analyst Agent`** workflow is now ready to respond via the Chat Trigger.

---

## ⚖️ License

This project is protected under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International Public License (CC BY-NC-SA 4.0)**.

This license allows for **sharing and adaptation** only under the condition of **attribution** and strictly **prohibits commercial use** of the material, preserving the uniqueness of this solution.

The full legal text is available in the **LICENSE** file in this repository.