# Pipenv Tips
* How to create "requirements.txt"
    * pipenv requirements > requirements.txt

# Sample Streamlit Deploy Code
```
import streamlit as st
import openai
import os


if __name__ == "__main__":
    api_key_input = st.text_input("OpenAI API Key", type="password", value=os.getenv("OPENAI_API_KEY") or st.session_state.get("OPENAI_API_KEY", ""))
    api_key_button = st.button("Add")
    if api_key_button:
        st.session_state["OPENAI_API_KEY"] = api_key_input

    openai_api_key = st.session_state.get("OPENAI_API_KEY")
    if openai_api_key:
        openai.api_key = openai_api_key
        response = openai.ChatCompletion.create(
            model = "gpt-3.5-turbo",
            messages = [{"role": "user", "content": "Hello"}]
        )

        st.write(response)
    else:
        st.warning("WARNING: Enter your OpenAI API key!")
```
