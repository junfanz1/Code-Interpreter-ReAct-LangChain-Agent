# React LangChain Agent

Build a simple ReAct agent using LangChain, integrated with a custom tool to calculate the length of a given text.

## Overview

This project showcases the following:

* **LangChain Agents:** Utilizes LangChain's agent framework to create a ReAct-style agent.
* **Custom Tools:** Defines a custom tool (`get_text_length`) to perform a specific task (calculating text length).
* **Prompt Engineering:** Constructs a prompt template for the agent to follow the ReAct pattern.
* **OpenAI Integration:** Leverages OpenAI's `ChatOpenAI` model for language processing.
* **Agent Interaction Loop:** Implements a loop to handle agent actions and observations.
* **Custom Output Parsing:** Creates a robust output parser (`ReActOutputParser`) to handle various agent responses.
* **Environment variable handling:** Uses `.env` files and `python-dotenv` to manage api keys.

## Prerequisites

Before running the project, ensure you have the following installed:

* Python 3.9+
* Poetry (recommended for dependency management) or pip
* OpenAI API key

## Setup

1.  **Clone the Repository:**

    ```bash
    git clone [https://github.com/junfanz1/react-langchain.git](https://www.google.com/search?q=https://github.com/junfanz1/react-langchain.git)
    cd react-langchain
    ```

2.  **Create a Virtual Environment (Recommended):**

    If you are using poetry:

    ```bash
    poetry install
    poetry shell
    ```

    If you are using pip:

    ```bash
    python3 -m venv .venv
    source .venv/bin/activate  # On macOS/Linux
    .venv\Scripts\activate  # On Windows
    pip install -r requirements.txt
    ```

3.  **Set Up Environment Variables:**

    * Create a `.env` file in the project's root directory.
    * Add your OpenAI API key to the `.env` file:

        ```
        OPENAI_API_KEY="your_openai_api_key"
        ```

4.  **Run the Script:**

    ```bash
    python main.py
    ```

## Code Explanation

### `main.py`

* **Imports:** Imports necessary modules from LangChain, OpenAI, and other libraries.
* **`get_text_length` Tool:** Defines a custom tool that calculates the length of a given text.
* **`find_tool_by_name` Function:** Finds a tool by its name.
* **Prompt Template:** Creates a prompt template for the ReAct agent.
* **LLM Initialization:** Initializes the `ChatOpenAI` model with the OpenAI API key.
* **Agent Pipeline:** Constructs the agent pipeline, including the prompt, LLM, and custom output parser.
* **Agent Interaction Loop:** Implements a loop to handle agent actions and observations.
* **`ReActOutputParser` Class:** Defines a custom output parser to handle various agent responses, including actions and final answers.

### `requirements.txt`

Lists the project's dependencies:

* `langchain`
* `langchain-openai`
* `langchain-community`
* `python-dotenv`

## How it Works

1.  The script initializes a ReAct agent with a custom tool (`get_text_length`).
2.  The agent receives a question: "What is the length of Dog in characters?".
3.  The agent uses the prompt template to generate a thought, action, and action input.
4.  The `ReActOutputParser` parses the agent's output.
5.  If the agent's output is an action, the script executes the tool and provides the observation back to the agent.
6.  If the agent's output is a final answer, the script prints the answer and exits.
7.  The process repeats until the agent provides a final answer.

## Custom Output Parser (`ReActOutputParser`)

The `ReActOutputParser` class is crucial for handling the agent's output. It uses regular expressions to parse the output and create either `AgentAction` or `AgentFinish` objects. This ensures that the script can correctly interpret the agent's responses.

## Future Improvements

* Add more tools to the agent.
* Implement error handling for tool execution.
* Improve the prompt template for better agent performance.
* Integrate with a web interface for a more interactive experience.
* Add more robust error handling.
* Add logging.
* Add unit tests.

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue.

## License

This project is licensed under the MIT License.
