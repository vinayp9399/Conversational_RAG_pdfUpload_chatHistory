# Conversational RAG with PDF Uploads and Chat History

A powerful, interactive Streamlit application that allows users to upload multiple PDF documents and have natural, context-aware conversations about their content. Powered by LangChain, Groq (`llama-3.1-8b-instant`), Hugging Face embeddings, and Chroma DB.

Live Link - https://conversationalragpdfuploadchathistory.streamlit.app/

---

## 📌 Introduction

When dealing with lengthy multi-page PDFs—such as research papers, legal contracts, or technical manuals—finding specific information can feel like searching for a needle in a haystack. 

This application utilizes **Retrieval-Augmented Generation (RAG)** to turn static PDF documents into dynamic chat partners. Unlike basic search engines or standard Q&A bots, this application **remembers the context of your conversation**. It uses a stateful chat history tracking mechanism to understand follow-up questions (e.g., if you ask "What is its profit margin?" after asking about a specific company, the system contextually maps "its" back to that company).

---

## 💼 Practical Use Cases

* **Academic Research:** Upload multiple research papers, cross-reference data points across texts, and summarize findings instantly.
* **Legal & Compliance Analysis:** Query dense contract documents, look up specific clauses, and verify compliance criteria without manual skimming.
* **Business Intelligence:** Upload annual financial reports, quarterly summaries, or product documentations to extract KPIs and performance metrics.
* **Customer Support & IT Documentation:** Turn product manuals or technical knowledge bases into an instantly searchable conversational assistant for fast troubleshooting.

---

## ⚙️ How It Works

The architecture follows a standard advanced RAG pipeline with a contextual query-rewriting layer:

1.  **Document Ingestion:** PDFs are loaded via `PyPDFLoader` and split into smaller, semantic chunks using a `RecursiveCharacterTextSplitter`.
2.  **Vector Embedding & Storage:** Text chunks are transformed into dense vector representations using Hugging Face's `all-MiniLM-L6-v2` model and indexed locally in a `Chroma` vector database.
3.  **Contextual Question Formulation:** If a user asks a follow-up question, a dedicated LLM prompt analyzes the `chat_history` and rewrites the user's prompt into a completely standalone query.
4.  **Semantic Retrieval:** The rewritten query is used to fetch the most relevant text chunks out of the Chroma vector store.
5.  **Generation:** The retrieved chunks (context) alongside the full chat history and original question are fed into Groq's high-speed `llama-3.1-8b-instant` model to generate a accurate, concise answer.

---

## 🔄 The Process Flow

```text
[User Uploads PDFs] 
       │
       ▼
[Text Splitting (5000 chars)] ──► [HuggingFace Embeddings] ──► [Chroma DB Vector Store]
                                                                        │
[User Inputs Question]                                                  │
       │                                                                │
       ▼                                                                ▼
[Contextualize Question Prompt] ──► [Rewritten Standalone Query] ──► [Semantic Search]
       ▲                                                                │
       │                                                                ▼
[Streamlit Chat History Store] ◄─────────────────────────────── [Generate Answer (Groq)]

---

## Interacting with the UI

1. **Provide your Credentials:** Paste your Groq API key securely into the password input box on the screen.

2. **Set Session ID:** Use the default session ID or create a unique session string (e.g., user_project_abc). This isolates your conversation threads.

3. **Upload Documents:** Drag and drop or browse one or more PDF files.

4. **Chat:** Start asking questions about your uploaded documents! The UI will show you the exact response, structured application session states, and a raw message log of your chat history.

---

## Conclusion

This application bridges the gap between static enterprise data and dynamic LLM inferences. By pairing ultra-fast inference speeds from Groq with the cost-effective semantic embeddings of Hugging Face, it delivers a highly responsive, contextual chatbot experience tailored exclusively to your private documents.
