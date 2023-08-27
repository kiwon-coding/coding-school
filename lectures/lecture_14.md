# Question Answering
* https://python.langchain.com/docs/use_cases/question_answering/

```
from langchain.document_loaders import TextLoader
from langchain.document_loaders import WebBaseLoader
from langchain.document_loaders import PyPDFLoader
from langchain.embeddings.openai import OpenAIEmbeddings
from langchain.embeddings import HuggingFaceEmbeddings
from langchain.text_splitter import CharacterTextSplitter
from langchain.vectorstores import FAISS
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

import os
openai_api_key = os.getenv("OPENAI_API_KEY")

## Step 1: LOAD: document loading (txt, web, pdf)
# raw_documents = TextLoader('state_of_the_union.txt').load()
# raw_documents = WebBaseLoader(
#     "https://lilianweng.github.io/posts/2023-06-23-agent/").load()
raw_documents = PyPDFLoader("llm_doc.pdf").load_and_split()

## Step 2: SPLIT: split a document into multiple chunk
text_splitter = CharacterTextSplitter(chunk_size=1000, chunk_overlap=0)
documents = text_splitter.split_documents(raw_documents)

## Step 3: STORE: embedding each chunk and store into vector DB
## 1) using openai embedding
# db = FAISS.from_documents(documents, OpenAIEmbeddings(openaai_api_key=openai_api_key))
# 2) or using huggingface embedding
db = FAISS.from_documents(documents, HuggingFaceEmbeddings())
db.save_local(f"llm_pdf")

## Step 4: RETRIEVE
selected_db = FAISS.load_local("llm_pdf", HuggingFaceEmbeddings())
question = "What is Task Decomposition?"
docs = selected_db.similarity_search(question)
# print(docs)
# print(len(docs))

## Step 5: Generate
llm = ChatOpenAI(model_name="gpt-3.5-turbo", temperature=0)

## 1) find the answer from the entire db
# qa_chain = RetrievalQA.from_chain_type(llm, retriever=selected_db.as_retriever())
# res = qa_chain({"query": question})
# print(res)

# 2) find the answer from the relevant splits
from langchain.chains.question_answering import load_qa_chain
chain = load_qa_chain(llm, chain_type="stuff")
res = chain({"input_documents": docs, "question": question},
      return_only_outputs=True)
print(res)
```
