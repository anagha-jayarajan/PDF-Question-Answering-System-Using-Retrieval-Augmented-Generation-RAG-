# PDF Question Answering System Using Retrieval-Augmented Generation (RAG)

## Project Overview

This project implements a Retrieval-Augmented Generation (RAG) pipeline for answering natural-language questions from PDF documents. It combines semantic search with a generative language model to retrieve relevant document passages and generate context-grounded answers.

The system extracts text from uploaded PDF files, divides the text into overlapping chunks, converts the chunks into vector embeddings, and indexes them using FAISS. When a user submits a question, the system retrieves the three most similar chunks and passes them to a language model to generate an answer based on the retrieved context.

Source page numbers are returned alongside the generated answer to support verification against the original document.

## Objectives

- Build an end-to-end document question-answering pipeline.
- Extract text from PDF documents while preserving page references.
- Implement overlapping text chunking for document preprocessing.
- Generate semantic embeddings for document passages and questions.
- Perform similarity-based retrieval using FAISS.
- Generate context-grounded answers using a sequence-to-sequence language model.
- Improve answer traceability by reporting the source page numbers.

## System Architecture

PDF Upload → Text Extraction → Text Chunking → Embedding Generation → FAISS Vector Index → Question Embedding → Similarity Search → Context Construction → FLAN-T5 Answer Generation → Answer and Source Pages

## Technologies Used

| Technology | Purpose |
|---|---|
| Python | Core programming language |
| Jupyter Notebook / Google Colab | Development and execution |
| PyMuPDF | PDF text extraction |
| Sentence Transformers | Semantic embedding generation |
| all-MiniLM-L6-v2 | Document and query embeddings |
| FAISS | Vector indexing and similarity search |
| Hugging Face Transformers | Language-model loading and inference |
| Google FLAN-T5 Large | Context-based answer generation |
| PyTorch | Model execution and device management |
| NumPy | Numerical and embedding operations |

## Methodology

### 1. PDF Upload and Text Extraction

The user uploads a PDF document. PyMuPDF extracts the text from each page, and the system records the corresponding page number for later source identification.

### 2. Text Chunking

Extracted text is divided into chunks of 150 words with a 30-word overlap. This overlap helps preserve context between consecutive chunks.

### 3. Semantic Embedding Generation

The `all-MiniLM-L6-v2` model converts each text chunk into a numerical vector representation. Embeddings are normalized before indexing.

### 4. Vector Indexing

FAISS stores the document embeddings in an `IndexFlatIP` index. Inner-product similarity on normalized embeddings is used to retrieve semantically similar passages.

### 5. Query Processing and Retrieval

The user's question is converted into an embedding using the same model. FAISS retrieves the three highest-ranked chunks based on similarity.

### 6. Context-Grounded Answer Generation

The retrieved passages and user question are combined into a prompt. The `google/flan-t5-large` model generates an answer with instructions to rely on the supplied context and indicate when the answer is unavailable.

### 7. Source Page Identification

The system returns the generated answer and the unique page numbers associated with the retrieved chunks.

## Installation

Install the required Python libraries:

```bash
pip install -r requirements.txt
```

This notebook uses Google Colab's file-upload functionality. The easiest way to execute it is in Google Colab.

## How to Run

1. Open the notebook in Google Colab.
2. Execute the library installation cell.
3. Run the PDF upload cell and select a text-based PDF.
4. Execute the remaining cells in order to extract text, create chunks, generate embeddings, and build the FAISS index.
5. Load the FLAN-T5 model.
6. Use the interactive question loop to ask questions about the uploaded document.
7. Review the generated answer and source page numbers.

Type `exit` in the question loop to stop the interaction.

## Example Interaction

**Example question:** What is the main topic discussed in the document?

**Expected output format:**

```text
Ask a question: What is the main topic discussed in the document?

Answer: [Answer generated from the retrieved context]

Source pages: [2, 4, 5]
```

The answer and page numbers above illustrate the output format; they are not measured results from a specific PDF.

## Advantages

- Uses semantic retrieval rather than relying exclusively on keyword matching.
- Returns source page numbers to help users verify answers.
- Uses open-source libraries and publicly available model weights.
- Provides a modular pipeline that can be extended with other embedding models, vector databases, or language models.
- Demonstrates the integration of information retrieval and natural-language generation.

## Limitations

- Scanned PDFs without extractable text are not handled by the current implementation.
- Fixed-size word chunking can separate related information.
- The system retrieves a fixed number of passages without a reranking stage.
- FLAN-T5 Large has input-length and computational limitations.
- Generated answers may be incomplete or incorrect even when relevant passages are retrieved.
- The current notebook does not include a formal retrieval or answer-quality evaluation framework.

## Future Enhancements

- Add OCR support for scanned documents.
- Implement section-aware or semantic chunking.
- Add a reranking model to improve passage selection.
- Display retrieved passages alongside answers for greater transparency.
- Evaluate retrieval and generation using suitable metrics and a curated question-answer dataset.
- Develop a Streamlit interface for interactive PDF question answering.
- Add support for multiple documents and persistent vector indexes.

## Repository Structure

```text
rag-document-qa/
├── RAG_Document_QA.ipynb
├── README.md
├── requirements.txt
└── .gitignore
```

## Conclusion

This project demonstrates a practical implementation of Retrieval-Augmented Generation for PDF-based question answering. By combining document preprocessing, semantic embeddings, vector similarity search, and language-model generation, the pipeline provides a foundation for building document-grounded question-answering applications.

The project can be extended into a more robust document assistant through improved retrieval, evaluation, error handling, and user-interface development.

## Disclaimer

This project is intended for educational and experimental purposes. Generated answers should be verified against the original source document.

