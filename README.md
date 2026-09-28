# 💘 Your Online Dating Advisor — A RAG Chatbot for Safer Modern Dating

## Project Overview

This project builds a conversational **online dating advisor** using **Retrieval-Augmented Generation (RAG)** with memory. Instead of relying only on what a language model happens to "know", the chatbot first searches a small library of trusted articles on dating safety, romance scams, healthy communication, consent and online-dating research, and then answers based on what it finds. If the library doesn't cover a question, the advisor says so rather than guessing.

The chatbot is wrapped in a custom-styled **Gradio** web app, designed to feel playful but polished, and can be shared through a temporary public link for demos and presentations.

## What This Project Offers

* **Grounded answers:** responses are based on trusted sources such as public-health, consumer-protection and research organisations, not on the model's guesswork.
* **Honest limits:** when a question falls outside its knowledge, the advisor says so instead of inventing an answer.
* **Conversation memory:** the chatbot follows the flow of the conversation, so follow-up questions make sense.
* **A friendly but safety-first persona:** warm and supportive in tone, while prioritising safety, respecting its limits and pointing people to professional help when needed.
* **A polished chat interface:** a custom Gradio app with its own theme, fonts, avatars and example questions.
* **An easy-to-follow notebook:** every step, from downloading the data to launching the app, is documented in one notebook.

## Technologies Used

* Python
* LlamaIndex (document loading, chunking, indexing, chat engine with memory)
* Groq API (`openai/gpt-oss-120b`) as the language model
* Hugging Face `sentence-transformers/all-MiniLM-L6-v2` for embeddings
* Gradio (chat interface, custom theme and CSS)
* Pillow (generating the chat avatars)
* requests, pandas, python-dotenv
* Jupyter Notebook

## How It Works

1. **Load the data**
   * Downloads the source pages into `content/data/`.
   * Reads them with LlamaIndex's `SimpleDirectoryReader`.

2. **Split into chunks**
   * Documents are split into overlapping chunks of about 800 characters (150 overlap) so each piece is small enough to be retrieved precisely.

3. **Create embeddings and a vector index**
   * Each chunk is turned into a vector with the MiniLM embedding model.
   * The vectors are stored in a `VectorStoreIndex` that is saved to disk and reloaded, so it only has to be built once.

4. **Retrieve and answer (RAG)**
   * For each question, the two most similar chunks are retrieved and passed to the language model as context.
   * The model answers using only that context and the previous conversation.

5. **Add memory and personality**
   * A `ContextChatEngine` with a memory buffer lets the advisor follow the conversation.
   * A system prompt gives it a warm "DatingCoach" persona, keeps answers short, prioritises safety and tells it to admit when something is outside its knowledge.

6. **Build the app**
   * A Gradio `ChatInterface` provides the chat window, with a rose-and-amber theme, the Lora font, custom CSS, example questions and generated avatars.

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repository-url>
cd <your-repository-folder>
```

### 2. Create a virtual environment and install the dependencies

```bash
python -m venv .venv

# Windows (PowerShell)
.venv\Scripts\Activate.ps1

pip install -r requirements.txt
```

### 3. Get your own API keys

The keys are **not** included in this repository, so you need to generate your own. Both are free to create.

| Key | What it is used for | Where to get it |
|---|---|---|
| `GROQ_API_KEY` | Runs the language model that writes the answers | [console.groq.com/keys](https://console.groq.com/keys) |
| `HF_TOKEN` | Lets the notebook download the embedding model from Hugging Face | [huggingface.co/settings/tokens](https://huggingface.co/settings/tokens) (a token with **Read** access is enough) |

### 4. Create your `.env` file

In the root of the project (next to the notebook), create a file named `.env` containing:

```
GROQ_API_KEY=<insert your key here>
HF_TOKEN=<insert your token here>
```

Replace the placeholders with your own values, with no quotes and no spaces around the `=`.

### 5. Run the notebook

Open `Online_Dating_Advisor.ipynb` in Jupyter or VS Code and use **Restart Kernel and Run All Cells**. The notebook will download the data, build the index, create the chatbot and launch the Gradio app.

When the last cell finishes, Gradio prints a local address and a public `gradio.live` link. The public link is temporary (about one week) and only works while the notebook is running.

## Limitations

* The advisor only knows what is in its small library of articles, so many questions will fall outside its knowledge, and it will say so.
* Some websites block automated downloads (a `403 Forbidden` error). If a page fails to download, re-run the download cell or replace it with another source.
* Retrieval quality depends on the chunk size and the number of retrieved passages, both of which can be tuned.
* Much of the data comes from US-based organisations, so legal and reporting advice (for example, where to report a scam) may not apply everywhere.

## Disclaimer

This project is an educational prototype. It is **not** a therapist, lawyer or emergency service. If you are experiencing abuse, harassment, stalking or are in immediate danger, please contact local emergency services or a qualified professional.

## Conclusion

This project shows how Retrieval-Augmented Generation can turn a general-purpose language model into a focused, more trustworthy advisor for a specific topic. Grounding the answers in trusted sources, adding conversation memory and a clear persona, and wrapping everything in a polished Gradio interface produces a chatbot that is friendly to use and honest about its limits. Future improvements could include showing the source of each answer, adding more documents, and evaluating answer quality more systematically.
