* https://github.com/aghasemi/streamlit_js_eval

```
import streamlit as st
import os
from utils import set_openai_api_key

from langchain.document_loaders import PyPDFLoader
from langchain.embeddings import HuggingFaceEmbeddings
# from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import FAISS
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI
from PyPDF2 import PdfReader
from langchain.text_splitter import RecursiveCharacterTextSplitter


def clear_question():
    st.session_state["question_text"] = ""


def db_selector():
    db_folder_path = "db"
    dbs = [f for f in os.listdir(db_folder_path)]
    dbs.append("Upload a new file")
    selected_dbname = st.selectbox(
        'Select database', dbs, on_change=clear_question)
    db_full_path = ""
    if selected_dbname != "Upload a new file":
        db_full_path = os.path.join(db_folder_path, selected_dbname)
    return db_full_path


def create_new_db():
    uploaded_file = st.file_uploader("Choose a file", 'pdf')
    if uploaded_file is not None:
        filename = uploaded_file.name
        with st.spinner('Loading...'):
            db_path = os.path.join("db", filename)
            try:
                db = FAISS.load_local(db_path, HuggingFaceEmbeddings())
            except RuntimeError:
                # Step 1: LOAD: document loading (txt, web, pdf)
                texts = ""
                reader = PdfReader(uploaded_file)
                for page in reader.pages:
                    texts += page.extract_text()

                # Step 2: SPLIT: split a document into multiple chunk
                text_splitter = RecursiveCharacterTextSplitter(
                    chunk_size=1000, chunk_overlap=0)
                documents = text_splitter.create_documents([texts])

                # # Step 3: STORE: embedding each chunk and store into vector DB
                db_path = f"db/{uploaded_file.name}"
                db = FAISS.from_documents(documents, HuggingFaceEmbeddings())
                db.save_local(db_path)


if __name__ == "__main__":
    default_question = ""
    set_openai_api_key()
    db_fullpath = db_selector()
    if db_fullpath:
        try:
            db = FAISS.load_local(db_fullpath, HuggingFaceEmbeddings())
        except RuntimeError:
            st.write("DB loading error")

        # Step 4: RETRIEVE & Generate
        question = st.text_input(
            "Enter your question about the file.", key="question_text")
        if question:
            llm = ChatOpenAI(
                openai_api_key=st.session_state.get("OPENAI_API_KEY"))
            # st.write(llm)
            qa_chain = RetrievalQA.from_chain_type(
                llm, retriever=db.as_retriever())
            res = qa_chain({"query": question})
            st.write(res["result"])
    else:
        create_new_db()
```
