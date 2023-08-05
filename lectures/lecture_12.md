# Topic
* langchain + streamlit

# Data
* https://cseweb.ucsd.edu/~jmcauley/datasets/amazon_v2/
** Amazon review data (2018)
** AMAZON FASHION reviews (883,636 reviews)

# Code
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

from tqdm import tqdm 
import ast

def load_review_data():
    json_file_path = './data/amazon_fashion_simple.json'
    with open(json_file_path, 'r') as f:
        json_data = [json.loads(line) for line in f]

    return pd.DataFrame(json_data)
    
def parse_string_to_list(string_data):
    try:
        parsed_list = ast.literal_eval(string_data)
        return parsed_list
    except Exception as e:
        print(string_data)
        print("Error parsing string:", e)
        return []

def get_taggings(review_text):
    chat = ChatOpenAI(openai_api_key = os.getenv("OPENAI_API_KEY"))

    template = "A tagging system that creates tags for use in an online shopping mall."
    system_message_prompt = SystemMessagePromptTemplate.from_template(template)
    human_template = "Create up to 5 tags for the given review. The result should be an python style array of strings: ```{text}```"
    human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)
    chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, human_message_prompt])

    chain = LLMChain(llm=chat, prompt=chat_prompt)
    res = chain.run(text = review_text)

    tags = parse_string_to_list(res)

    return tags

def load_review_tags():
    review_tags = pd.read_csv('./data/amazon_fashion_review_tags.csv')
    return review_tags

if __name__ == '__main__':
    reviews = load_review_data()

    tqdm.pandas()
    reviews['tags'] = reviews.progress_apply(lambda x: get_taggings(x['reviewText']), axis=1)
    reviews.to_csv('./data/amazon_fashion_review_tags.csv', index=False)
    
    all_tags = {}
    review_tags = load_review_tags()
    tag_column_df = review_tags['tags'].apply(ast.literal_eval)
    for tags in tag_column_df:
        for tag in tags:
            all_tags[tag] = all_tags.get(tag, 0) + 1
    
    print(dict(sorted(all_tags.items(), key=lambda item: item[1])))
```
