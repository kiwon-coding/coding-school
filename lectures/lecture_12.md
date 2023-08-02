* langchain + streamlit

```
import pandas as pd
import json
import os
from langchain import LLMChain
from langchain.chat_models import ChatOpenAI

from langchain.prompts.chat import (
    ChatPromptTemplate,
    SystemMessagePromptTemplate,
    HumanMessagePromptTemplate
)

def load_review_data():
    json_file_path = './data/amazon_fashion_simple.json'
    with open(json_file_path, 'r') as f:
        json_data = [json.loads(line) for line in f]

    return pd.DataFrame(json_data)
    
def request_openai():
    chat = ChatOpenAI(openai_api_key = os.getenv("OPENAI_API_KEY"))

    template = "A tagging system that creates tags for use in an online shopping mall."
    system_message_prompt = SystemMessagePromptTemplate.from_template(template)
    human_template = "Create up to 5 tags for the given review. The result should be an python style array of strings: ```{text}```"
    human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)
    chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, human_message_prompt])

    chain = LLMChain(llm=chat, prompt=chat_prompt)

    review_sample = "Love these... I am going to order another pack to keep in work; someone (including myself) is always losing the back to an earring.  I don't understand why all fish hook earrings don't have them.  Just wish that they were a tiny bit longer.  :)"
    res = chain.run(text = review_sample)
    return res

if __name__ == '__main__':
    df = load_review_data()
    print(df)

    res = request_openai()
    print(res)
```
