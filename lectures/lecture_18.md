* [sample pdf 01](https://github.com/kiwon-coding/coding-school/blob/main/lectures/The%20Robotic%20Exploration%20of%20the%20Moon.pdf)
* [sample pdf 02](https://github.com/kiwon-coding/coding-school/blob/main/lectures/A%20BRIEF%20HISTORY%20OF%20SPACE%20EXPLORATION.pdf)

```
import streamlit as st
from PyPDF2 import PdfReader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.vectorstores import FAISS

uploaded_file = st.file_uploader("Choose a file", 'pdf')
if uploaded_file is not None:
    # Step 1: LOAD: document loading (txt, web, pdf)
    texts = ""
    reader = PdfReader(uploaded_file)
    for page in reader.pages:
        texts += page.extract_text()

    # Step 2: SPLIT: split a document into multiple chunk
    text_splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
    documents = text_splitter.create_documents([texts])

    # # Step 3: STORE: embedding each chunk and store into vector DB
    db_path = f"db/{uploaded_file.name}"
    db = FAISS.from_documents(documents, HuggingFaceEmbeddings())
    db.save_local(db_path)
```
