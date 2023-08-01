* [LangChain](https://python.langchain.com)

```
import os
from langchain import LLMChain
from langchain.chat_models import ChatOpenAI
from langchain.llms import OpenAI

from langchain.prompts.chat import (
    ChatPromptTemplate,
    SystemMessagePromptTemplate,
    HumanMessagePromptTemplate
)

chat = ChatOpenAI(openai_api_key = os.getenv("OPENAI_API_KEY"))

llm = OpenAI()

template = "tanslates {input_language} to {output_language}"
system_message_prompt = SystemMessagePromptTemplate.from_template(template)
human_template = "{text}"
human_message_prompt = HumanMessagePromptTemplate.from_template(human_template)
chat_prompt = ChatPromptTemplate.from_messages([system_message_prompt, human_message_prompt])

chain = LLMChain(llm=chat, prompt=chat_prompt)
res = chain.run(input_language="English", output_language="Korean", text = "I love programming.")
print(res)
```

```
import os
from langchain.utilities import SerpAPIWrapper

search = SerpAPIWrapper(serpapi_api_key=os.getenv("SERPAPI_API_KEY"))
res = search.run("Obama's first name?")
print(res)
```

```
import os
from langchain import LLMChain
from langchain.chat_models import ChatOpenAI
from langchain.llms import OpenAI

from langchain.agents import load_tools
from langchain.agents import initialize_agent
from langchain.agents import AgentType

chat = ChatOpenAI(openai_api_key = os.getenv("OPENAI_API_KEY"))

llm = OpenAI()
tools = load_tools(["serpapi", "llm-math"], llm=llm)
agent = initialize_agent(tools, chat, agent=AgentType.CHAT_ZERO_SHOT_REACT_DESCRIPTION, verbose=True)

res = agent.run("Who is Olivia Wilde's boyfriend? What is his current age raised to the 0.23 power?")
print(res)
```
