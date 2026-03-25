# lang_chain
# Colab Gemini LLM Toolkit: Chat & File Name Generator

## Project Title

**Colab Gemini LLM Toolkit: Interactive Chat & Smart File Name Generator**

## Brief Description

This Google Colab notebook provides a convenient and interactive environment to experiment with Google's Gemini Large Language Model (LLM) using the `langchain_google_genai` library. It features two core functionalities:

1.  **Interactive Chatbot:** Engage in a real-time conversation with the Gemini LLM, inputting prompts and receiving immediate responses.
2.  **Smart File Name Generator:** Provide a topic, and the LLM will intelligently suggest multiple, descriptive, and valid file names, complete with appropriate extensions.

This project is ideal for developers, researchers, or anyone looking to quickly prototype, test, and interact with the Gemini LLM in a user-friendly Colab environment, with secure API key management.

## Features

*   **Secure API Key Management:** Utilizes Google Colab Secrets for storing and accessing your `GEMINI_API_KEY` securely.
*   **Interactive LLM Chat:** A continuous loop for back-and-forth conversations with Gemini.
*   **Contextual File Name Generation:** Leverages Gemini's understanding to create relevant file names based on user input.
*   **Easy Setup:** Minimal configuration required to get started within Google Colab.
*   **LangChain Integration:** Demonstrates basic usage of LangChain for prompt engineering and model interaction.

## Prerequisites

Before running this notebook, you will need:

*   A Google account to access Google Colab.
*   Access to the Google Gemini API. You can obtain a `GEMINI_API_KEY` from [Google AI Studio](https://aistudio.google.com/app/apikey).

## Setup Instructions

Follow these steps to set up and run the project in Google Colab:

1.  **Open in Google Colab:**
    *   Create a new Google Colab notebook, or upload this `README.md` content into a new `.ipynb` file and convert the markdown cells to code cells where specified.
    *   Alternatively, you can copy the code snippets directly into your Colab notebook.

2.  **API Key Management (Crucial Step!):**
    *   **Obtain your `GEMINI_API_KEY`**: Go to [Google AI Studio](https://aistudio.google.com/app/apikey) and create an API key.
    *   **Add to Colab Secrets**:
        *   In your Google Colab notebook, look for the "🔑 Secrets" icon on the left sidebar.
        *   Click on it, then click "Add new secret".
        *   For the **Name**, enter `GEMINI_API_KEY` (it must match this exact name).
        *   For the **Value**, paste your API key copied from Google AI Studio.
        *   Make sure the "Notebook access" toggle is **ON** for your current notebook.
        *   This method securely stores your API key without embedding it directly into your code or notebook, preventing accidental exposure.

## Dependencies and Installation

The project relies on a few Python libraries. These will be installed in the first code cell of your Colab notebook.

### Installation Code Snippet

Execute the following code in the first cell of your Colab notebook:

```python
# Initial Setup Cell: Install dependencies and configure API Key

!pip install -qU langchain-google-genai langchain google-generativeai ipywidgets

import os
from google.colab import userdata
from IPython.display import display, Markdown, clear_output
import ipywidgets as widgets
from langchain_google_genai import ChatGoogleGenerativeAI
from langchain.prompts import ChatPromptTemplate
from langchain.chains import LLMChain

print("Dependencies installed successfully!")

# Retrieve API key from Colab Secrets
GEMINI_API_KEY = userdata.get('GEMINI_API_KEY')

if not GEMINI_API_KEY:
    print("Error: GEMINI_API_KEY not found in Colab Secrets.")
    print("Please go to the '🔑 Secrets' tab on the left sidebar, add a new secret named 'GEMINI_API_KEY', and paste your key.")
    print("Ensure 'Notebook access' is enabled for this notebook.")
else:
    print("GEMINI_API_KEY successfully loaded from Colab Secrets.")
    # Langchain_google_genai automatically picks up GOOGLE_API_KEY from os.environ
    # Alternatively, you can pass it directly: llm = ChatGoogleGenerativeAI(model="gemini-pro", google_api_key=GEMINI_API_KEY)
    os.environ['GOOGLE_API_KEY'] = GEMINI_API_KEY
    print("API key configured for LangChain.")

# Initialize the chat model (will be used by both features)
llm = ChatGoogleGenerativeAI(model="gemini-pro", temperature=0.7)
print("Gemini LLM model initialized!")
```

## How to Run the Interactive LLM Chat

After successfully running the initial setup cell:

1.  **Run the Chat Code Cell:** Execute the following code in a new cell in your Colab notebook.
2.  **Interact:** A prompt "You: " will appear. Type your message and press Enter.
3.  **Exit:** To end the chat, type `exit` or `quit` and press Enter.

```python
# Code Snippet 1: Interactive LLM Chat

print("--- Interactive Gemini Chat ---")
print("Type 'exit' or 'quit' to end the chat.")

# Define the chat prompt template
chat_template = ChatPromptTemplate.from_messages([
    ("human", "{user_input}"),
])

# Create the LLM chain
chat_chain = LLMChain(llm=llm, prompt=chat_template)

while True:
    user_input = input("You: ")
    if user_input.lower() in ["exit", "quit"]:
        print("Exiting chat.")
        break
    
    # Clear previous output and re-display user input for better readability in Colab
    # The get_ipython().events.track_output() context manager helps prevent duplicate output in some Colab environments.
    with get_ipython().events.track_output(): 
        clear_output(wait=True) 
        display(Markdown(f"You: **{user_input}**"))
        
        # Get response from LLM
        response = chat_chain.run(user_input=user_input)
        display(Markdown(f"**Gemini:** {response}"))
```

## How to Use the File Name Generator

After successfully running the initial setup cell (and optionally the chat):

1.  **Run the File Name Generator Code Cell:** Execute the following code in another new cell in your Colab notebook.
2.  **Input Topic:** A prompt will ask you to "Enter a topic for file names".
3.  **Get Suggestions:** The LLM will process your topic and provide a list of suggested file names formatted for common use.

```python
# Code Snippet 2: File Name Generator

print("\n--- File Name Generator ---")

# Define a specific prompt template for file name generation
file_name_template = ChatPromptTemplate.from_messages([
    ("system", """You are a helpful assistant that generates short, descriptive, and valid file names.
    Generate 3-5 file names based on the user's topic.
    Each file name should be lowercase, use hyphens instead of spaces, and end with a common extension like '.txt', '.md', '.py', '.json', or '.csv'.
    List each file name on a new line."""),
    ("human", "Generate file names for the topic: {topic}"),
])

file_name_chain = LLMChain(llm=llm, prompt=file_name_template)

topic = input("Enter a topic for file names (e.g., 'project ideas', 'meeting notes'): ")

if topic:
    print(f"\nGenerating file names for: '{topic}'...")
    generated_names = file_name_chain.run(topic=topic)
    print("\nSuggested File Names:")
    # Display as a markdown code block for clean formatting
    display(Markdown(f"```\n{generated_names}\n```"))
else:
    print("No topic entered. Exiting file name generator.")
```

---

## Contributing

Contributions are welcome! If you have suggestions for improvements, new features, or bug fixes, please feel free to:

1.  Fork the repository (if this were a GitHub repo).
2.  Create a new branch (`git checkout -b feature/AmazingFeature`).
3.  Make your changes.
4.  Commit your changes (`git commit -m 'Add some AmazingFeature'`).
5.  Push to the branch (`git push origin feature/AmazingFeature`).
6.  Open a Pull Request.

## License

This project is open-sourced under the MIT License. See the [LICENSE](LICENSE) file for details.

---
