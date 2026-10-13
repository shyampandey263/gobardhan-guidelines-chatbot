# GOBARdhan Guidelines Chatbot

A free, open-source chatbot that answers questions about a scanned 78-page government
document, the *Component-wise Operational Guidelines under the GOBARdhan Scheme* for the
Compressed Biogas (CBG) sector. It runs on Google Colab with no paid services.

> This is a personal learning project. It is not an official government tool, and its
> answers can contain mistakes. Always check important numbers and dates against the
> original document.

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/shyampandey263/gobardhan-guidelines-chatbot/blob/main/gobardhan_chatbot.ipynb)

## What it does

- Reads a **scanned PDF (Portable Document Format)** using OCR (Optical Character Recognition)
- Splits the text into chunks and finds the most relevant ones for each question
- Writes an answer with a free open-source LLM (Large Language Model)
- Shows the **source text** used for every answer, so you can verify it
- Answers broad questions such as "What is the scheme?" by quoting page 1 exactly,
  which avoids invented details

## Tools used

| Part | Tool |
|---|---|
| Platform | Google Colab (free T4 GPU, Graphics Processing Unit), Google Drive |
| OCR | Tesseract, pytesseract, Poppler, pdf2image, pypdf |
| Embeddings | sentence-transformers (`all-MiniLM-L6-v2`) |
| Vector search | FAISS (Facebook AI Similarity Search) |
| Keyword search | rank_bm25 (BM25, Best Matching 25) |
| Language model | Qwen2.5-3B-Instruct via Hugging Face Transformers |
| Interface | Gradio |

## How it works

1. **OCR:** each PDF page becomes an image, and Tesseract reads the text.
2. **Chunking:** the text is split into paragraph-based chunks of up to 700 characters.
3. **Hybrid search:** semantic search (FAISS) and keyword search (BM25) are merged with
   Reciprocal Rank Fusion.
4. **RAG (Retrieval-Augmented Generation):** the best chunks go into the prompt, and the
   model answers using only that text.
5. **Safeguards:** deterministic decoding, strict prompt rules, extractive answers for
   broad questions, and a "Sources used" line under every answer.

## How to run

1. Click the **Open in Colab** badge and set **Runtime → Change runtime type → T4 GPU**.
2. Download the GOBARdhan guidelines PDF from the Ministry of Petroleum and Natural Gas
   website and upload it to Colab's file panel. The PDF is not included in this repository.
3. Run Cell 1 (connect Google Drive), then Cell 2 (one-time OCR, about 5 minutes).
4. Run Cell 3 (loads the model and builds the search), then Cell 4 (starts the chat).

The OCR text is saved to your Google Drive, so after the first run you can skip Cell 2.

## Example questions

- What is the GOBARdhan scheme?
- What are the six components?
- Who chairs the Project Approval Board?
- Which ministry issued the memorandum?

## Limitations

- Small language models can still add or mix up details, so check the "Sources used" text.
- OCR can misread symbols. For example, the rupee sign was read as a yen sign and had to be
  corrected in the code.
- It is text-only. Tables, charts and diagrams are flattened by OCR, so the system is not
  multimodal.
- Colab sessions reset after inactivity, so the model and search index are rebuilt each time.

## Future work

- Deploy on Hugging Face Spaces
- Add table and chart understanding with a vision-language model
- Evaluate answer accuracy on a fixed set of test questions
