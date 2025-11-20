## Development and Deployment of a 'Chat with LLM' Application Using the Gradio Blocks Framework

### AIM:
To design and deploy a "Chat with LLM" application by leveraging the Gradio Blocks UI framework to create an interactive interface for seamless user interaction with a large language model.

### PROBLEM STATEMENT:

Building a user-friendly application that allows seamless interaction with a large language model (LLM) is challenging without requiring specialized API keys or external resources. This project addresses the need for an accessible, open-source solution to implement such applications using pre-trained models and the Gradio Blocks framework.

### DESIGN STEPS:

#### STEP 1:

Import Required Libraries. Install and import the necessary libraries: Gradio for the UI. Transformers for using pre-trained models.

#### STEP 2:

Use a pre-trained model like GPT-2 or DialoGPT from Hugging Face.Initialize the model using the pipeline API for straightforward interaction.

#### STEP 3:

Run the application locally with demo.launch(). Optionally deploy it to the cloud for broader accessibility.


## NAME : AJAY S

## REG NO: 212224230010

### PROGRAM:
```

import os
from dotenv import load_dotenv, find_dotenv
import gradio as gr
from text_generation import Client

load_dotenv(find_dotenv())

HF_API_KEY = os.environ["HF_API_KEY"]
FALCON_ENDPOINT = os.environ["HF_API_FALCOM_BASE"]


client = Client(
    FALCON_ENDPOINT,
    headers={"Authorization": f"Basic {HF_API_KEY}"},
    timeout=120
)


def format_chat_prompt(message, chat_history):
    prompt = ""
    for user_msg, bot_msg in chat_history:
        prompt += f"User: {user_msg}\nAssistant: {bot_msg}\n"
    prompt += f"User: {message}\nAssistant:"
    return prompt

def respond(message, chat_history):
    prompt = format_chat_prompt(message, chat_history)

    # Generate using Falcon
    bot_message = client.generate(
        prompt,
        max_new_tokens=512,
        stop_sequences=["User:", "<|endoftext|>"]
    ).generated_text

    chat_history.append((message, bot_message))
    return "", chat_history

with gr.Blocks() as demo:
    gr.Markdown("## 🦅 Falcon LLM — Chatbot (Jupyter Notebook Version)")

    chatbot = gr.Chatbot(height=350)
    msg = gr.Textbox(label="Type your message...")
    submit = gr.Button("Send")
    clear = gr.ClearButton([msg, chatbot])

    submit.click(respond, [msg, chatbot], [msg, chatbot])
    msg.submit(respond, [msg, chatbot], [msg, chatbot])

demo.launch(share=False)   # share=True only if you want a public link
```
### OUTPUT:

<img width="1370" height="828" alt="Screenshot 2025-11-20 233829" src="https://github.com/user-attachments/assets/a6e903af-ed5e-4d50-9bec-9a9c6a2ee441" />


### RESULT:

Thus the Chat with LLM Application Using the Gradio Blocks Framework is created successfully.
