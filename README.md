# 💰 Project 1: Advanced Financial Trend Analyst Agent
### Dual-Tool Agentic Reasoning & Custom API Integration

This project demonstrates proficiency in building complex, self-hosted AI Agents capable of dynamic tool selection between quantitative database analysis (SQL) and qualitative compliance retrieval (RAG).

**Live Demo/Chat UI:** [Link to your n8n Chat UI if accessible, otherwise remove]

---

## 🛠️ Technical Stack & Architecture

| Component | Tool / Technology | Purpose |
| :--- | :--- | :--- |
| **Workflow Orchestration** | **n8n (Self-Hosted)** | Manages the end-to-end multi-step workflow and agent integration. |
| **LLM / Reasoning** | **Google Gemini 2.5 Pro** | Core decision-making engine for tool selection and final summarization. |
| **Database & Vector Store** | **PostgreSQL** with **pgvector** | Stores both transactional data (SQL queries) and policy embeddings (RAG). |
| **Embeddings** | **Ollama** (`nomic-embed-text:latest`) | Generates high-quality vector embeddings for RAG document indexing. |
| **Custom APIs / Tools** | **Custom Postgres Executor** & **Telegram API** | Custom tooling to meet advanced API integration requirements. |

---

## 🧠 Agentic Reasoning Workflow (Phase 5)

The Agent operates on a Dual-Tool architecture, where Gemini Pro decides the execution path based on the user's intent:

1.  **Input:** User message is received via the `Chat Trigger`.
2.  **Reasoning:** The `AI Agent` (Gemini Pro) analyzes the user prompt against a structured **System Prompt** that describes the two tools: `sql_query_executor` (for numbers) and `custom_rag_retriever` (for policy).
3.  **Tool Execution:** The Agent outputs the required tool call and arguments (either a raw SQL query or a RAG search query).
4.  **Final Output:** The Agent synthesizes the retrieved data/policy and pushes the final professional summary via the `Telegram API`.

### 1. **Custom Tool Implementation: SQL Executor (Advanced API)**

| Node | Configuration Detail | Technical Showcase |
| :--- | :--- | :--- |
| **Postgres Node** | Named: `sql_query_executor` | **Custom API Integration:** Converts a generic DB node into a dedicated Agent Tool. |
| **Query Logic** | `Execute Query` using `{{ $fromAI("query") }}` | **Dynamic Query Injection:** Forcing the LLM to generate the raw SQL string, which is executed safely by the n8n node, demonstrating control over LLM output. |

### 2. **Custom Tool Implementation: RAG Retriever (Advanced RAG)**

| Node | Configuration Detail | Technical Showcase |
| :--- | :--- | :--- |
| **Custom Code Node (Python)** | Named: `custom_rag_retriever` | **LLM-RAG Mastery:** Performs the entire retrieval and context formatting logic using manual Python code (including Ollama embedding generation and direct PGVector similarity search) to bypass known bugs in community nodes. |
| **Purpose** | Retrieves policy from the `financial_policy_rag` table. | Proves ability to manually construct and optimize RAG retrieval pipelines outside of pre-built modules. |

---

## 📝 RAG Ingestion Pipeline (Phase 4)

This workflow was used to prepare the knowledge base for the agent.

1.  **File Read:** Reads `compliance_policy.txt` from a dedicated `/rag_data` Docker volume.
2.  **Custom Chunking:** Uses a **Custom Python Code Node** for text splitting, outputting structured JSON items.
3.  **Embedding:** Calls the **Ollama** API to generate 384-dimension vectors.
4.  **Loading:** Inserts chunks and vectors into the **`financial_policy_rag`** table (addressing the column schema mismatch by renaming `content` to `text` for compatibility).

---

## 🛠️ Setup & Local Deployment

To run this project, follow these steps:

1.  **Clone Repository:** `git clone [repository URL]`
2.  **Dependencies:** Ensure **Docker Desktop** and **Ollama** are running locally.
3.  **Start Services:** Run `docker compose up -d` in the project root.
4.  **Configure Credentials:** Add your **Gemini API Key** and **Telegram Bot Token** in the n8n Credentials UI.
5.  **Run Ingestion:** Execute the **`RAG Ingestion: Financial Policy Load`** workflow once to populate the database.
6.  **Activate Agent:** The **`Financial Analyst Agent`** workflow is now ready to respond via the Chat Trigger.

---

## ⚖️ License

[Insert your license details here - See Part 2 below]

***

## ⚖️ Part 2: Project Licensing and Protection

You cannot *prevent* someone from downloading a public repository (that's the nature of Git), but you can absolutely **prevent them from legally copying, using, or distributing it without your permission.**

### 1. Choose a Restrictive License

The most common, restrictive license that maintains copyright while being clear about usage rights is the **MIT License**. However, if you want to be stricter and preserve your uniqueness, you should look at licenses that restrict commercial use and require attribution.

A good choice for maximum protection while remaining standard is: **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)**.

### 2. Add the License File

1.  Create a file named **`LICENSE`** (no extension) in your project root.

    ```powershell
    New-Item -Path . -Name "LICENSE" -ItemType File
    ```
2.  Paste the text of your chosen license into this file (e.g., the text for CC BY-NC-SA 4.0).

### 3. Update the README

Replace the placeholder in the `README.md` with the following:

```markdown
## ⚖️ License

This project, the **Advanced Financial Trend Analyst Agent**, is licensed under the **Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)**.

**You are free to:**
* **Share:** Copy and redistribute the material in any medium or format.

**Under the following terms:**
* **Attribution:** You must give appropriate credit.
* **NonCommercial:** You may not use the material for commercial purposes.
* **ShareAlike:** If you remix, transform, or build upon the material, you must distribute your contributions under the same license as the original.

**Commercial use, redistribution without attribution, or modification without sharing the source are strictly prohibited.**