# Multi Doc Chat Application

### **Project Overview**

This repository provides an end-to-end retrieval-augmented generation (RAG) demo named MultiDocChat. It allows you to upload documents (PDF/TXT/DOCX), creates a FAISS index of the uploaded content, then lets you ask natural-language questions over those documents using a conversational RAG workflow.

### **Quick Highlights**
- **Upload & Index**: Upload multiple documents and build a FAISS index per session.
- **Chat Interface**: Ask questions about uploaded documents and receive context-aware answers.
- **Session Management**: Upload sessions are persisted client-side and can be reloaded.
- **Modern Frontend**: Responsive UI with file preview, typing effects, and polished micro-interactions (`templates/index.html`, `static/styles.css`).

### **Requirements**
- **Python**: 3.10+ recommended
- See `requirements.txt` for exact Python packages used in this project (FastAPI, LangChain, FAISS, sentence-transformers, etc.).

### **Setup**
- Create a virtual environment and install dependencies:
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
- Start the server (development):
```bash
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```
- Open the app in your browser: `http://localhost:8000`

### **Project Structure (important files)**
- `main.py`: FastAPI application and endpoints (`/upload`, `/chat`).
- `templates/index.html`: Frontend HTML (reworked to be responsive and dynamic).
- `static/styles.css`: Styles and animations for the UI.
- `multi_doc_chat/`: Project Python package containing ingestion, retrieval, and model glue code.
- `faiss_index/`: Directory where FAISS indexes are stored per session.
- `requirements.txt`: Python package requirements.

### **Usage**
1. Start the server (see Setup).
2. From the UI, upload one or more documents. The UI shows a file preview and an indexing indicator.
3. After indexing completes you will be able to ask questions in the chat area. The assistant replies will be typed out with a typing animation.
4. Sessions are saved locally (in browser `localStorage`) and can be reloaded from the Sessions panel.

### **Notes on Frontend Behavior**
- The UI uses these DOM IDs and endpoints which the backend expects; avoid renaming unless you update `main.py`/JS accordingly:
	- `#file-input` — file input element used by the uploader
	- `#upload-form` — form that posts to `/upload`
	- `#upload-btn`, `#indexing` — upload button and status text
	- `#chat`, `#messages`, `#message-input`, `#send-btn`, `#thinking` — chat UI elements
- The upload flow now includes improved error handling and console logging to help debug upload errors.

### **Troubleshooting**
- If uploads report an error but the server shows `200`:
	- Open browser DevTools → Console and look for `Upload response status` and `Upload response data` logs.
	- The frontend now validates the returned JSON includes a `session_id`; if not present, you will see an error toast explaining the problem.
- If chat keeps showing a loader (dots) after the backend responds:
	- Restart the server, re-upload the document, and check the console for any JS errors.
- If FAISS indexing fails:
	- Check server logs printed by FastAPI/Uvicorn — the ingestion stack (`multi_doc_chat.src.document_ingestion`) will log exceptions.

### **Running Tests / Quick Checks**
- There are no automated tests included by default. For manual checks:
	- Upload a small `.txt` file, confirm the `faiss_index/<session_id>` directory appears and contains `index.faiss`.
	- Use the chat to ask a question that appears in the uploaded text.

### **Extending / Next Steps**
- Persist sessions server-side (DB) and list them in the Sessions panel.
- Add streaming responses from the backend for faster perceived responses.
- Add authentication and user-specific session storage.

**Contribution & License**
- Contributions welcome — open an issue or a pull request. This repo currently has no explicit license file; add one if you intend to publish.

**Contact / Author**
- Maintained by the repository owner. Use GitHub issues for bug reports and feature requests.

# End2End_RAG