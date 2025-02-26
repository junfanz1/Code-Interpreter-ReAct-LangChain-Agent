
<!-- TOC --><a name="react-langchain-agent-example"></a>
# ReAct LangChain Agent

This repository demonstrates the creation of a simple ReAct agent using LangChain, integrated with a custom tool to calculate the length of a given text. It serves as a foundational example for building more complex, tool-augmented LLM applications.

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=junfanz1/react-langchain&type=Date)](https://star-history.com/#junfanz1/react-langchain&Date)

## Contents
<!-- TOC start (generated with https://github.com/derlin/bitdowntoc) -->

   * [Project Purpose](#project-purpose)
   * [LLM Technology Stack](#llm-technology-stack)
   * [Challenges and Difficulties](#challenges-and-difficulties)
   * [Future Business Impact and Further Improvements](#future-business-impact-and-further-improvements)
   * [Target Audience and Benefits](#target-audience-and-benefits)
   * [Advantages and Disadvantages](#advantages-and-disadvantages)
   * [Tradeoffs](#tradeoffs)
   * [Highlight and Summary](#highlight-and-summary)
   * [Future Enhancements](#future-enhancements)
   * [Prerequisites](#prerequisites)
   * [Setup](#setup)
   * [Code Explanation](#code-explanation)
      + [`main.py`](#mainpy)
      + [`requirements.txt`](#requirementstxt)
   * [How it Works](#how-it-works)
   * [Custom Output Parser (`ReActOutputParser`)](#custom-output-parser-reactoutputparser)
   * [Future Improvements](#future-improvements)
   * [Contributing](#contributing)
   * [License](#license)

<!-- TOC end -->

<!-- TOC --><a name="project-purpose"></a>
## Project Purpose

The primary goal of this project is to provide a practical, hands-on example of building an agent using LangChain's ReAct framework. It aims to demystify the process and serve as a learning resource for developers looking to understand how to:

* Build custom tools for LangChain agents to extend their capabilities.
* Implement the ReAct pattern to enable agents to reason and act in a structured manner.
* Integrate LangChain with OpenAI's powerful language models for natural language processing.
* Manage agent interactions through an iterative loop, handling actions and observations effectively.
* Develop a robust output parser to interpret and process various agent responses, including actions and final answers.
* Securely manage API keys using environment variables.

This project is intended to be a stepping stone for building more complex and sophisticated agents capable of performing a wide range of tasks by interacting with tools and external data sources.

Example Input:

`md
What is the length of the word: DOG
`

Example Prompt Template:

`md
template = """
    Answer the following questions as best you can. You have access to the following tools:

    {tools}

    Use the following format:

    Question: the input question you must answer
    Thought: you should always think about what to do
    Action: the action to take, should be one of [{tool_names}]
    Action Input: the input to the action
    Observation: the result of the action
    ... (this Thought/Action/Action Input/Observation can repeat N times)
    Thought: I now know the final answer
    Final Answer: the final answer to the original input question

    Begin!

    Question: {input}
    Thought: {agent_scratchpad}
    """
`

Example Output (with intermediate CoT steps):

`md
Hello ReAct LangChain!
***Prompt to LLM was:***
Human: 
    Answer the following questions as best you can. You have access to the following tools:

    get_text_length(text: str) -> int - Returns the length of a text by characters

    Use the following format:

    Question: the input question you must answer
    Thought: you should always think about what to do
    Action: the action to take, should be one of [get_text_length]
    Action Input: the input to the action
    Observation: the result of the action
    ... (this Thought/Action/Action Input/Observation can repeat N times)
    Thought: I now know the final answer
    Final Answer: the final answer to the original input question

    Begin!

    Question: What is the length of the word: DOG
    Thought: 
    
******
***LLM Response:***
I should use the get_text_length function to determine the length of the word "DOG".
    Action: get_text_length
    Action Input: "DOG"
    
******
tool='get_text_length' tool_input='DOG"\n' log='I should use the get_text_length function to determine the length of the word "DOG".\n    Action: get_text_length\n    Action Input: "DOG"\n    '
get_text_length enter with text='DOG"\n'
observation=3
***Prompt to LLM was:***
Human: 
    Answer the following questions as best you can. You have access to the following tools:

    get_text_length(text: str) -> int - Returns the length of a text by characters

    Use the following format:

    Question: the input question you must answer
    Thought: you should always think about what to do
    Action: the action to take, should be one of [get_text_length]
    Action Input: the input to the action
    Observation: the result of the action
    ... (this Thought/Action/Action Input/Observation can repeat N times)
    Thought: I now know the final answer
    Final Answer: the final answer to the original input question

    Begin!

    Question: What is the length of the word: DOG
    Thought: I should use the get_text_length function to determine the length of the word "DOG".
    Action: get_text_length
    Action Input: "DOG"
    
Observation: 3
Thought: 
    
******
***LLM Response:***
I now know the final answer
Final Answer: 3
******
return_values={'output': '3'} log='I now know the final answer\nFinal Answer: 3'
{'output': '3'}

Process finished with exit code 0
`

<!-- TOC --><a name="llm-technology-stack"></a>
## LLM Technology Stack

This project leverages the following LLM technologies:

* **LangChain:** A framework for developing LLM-powered applications, providing abstractions for agents, chains, and tools.
* **OpenAI `ChatOpenAI`:** Used as the LLM for reasoning and generating responses, leveraging its chat-based capabilities and natural language understanding.
* **ReAct Pattern:** Implemented to enable the agent to interleave reasoning (thoughts) and actions, improving its ability to solve complex tasks.
* **Custom Tools:** Python functions wrapped as LangChain tools, allowing the agent to interact with external functionality (e.g., `get_text_length`).
* **Prompt Engineering:** Carefully designed prompts to guide the agent's behavior and ensure it adheres to the ReAct pattern.
* **`python-dotenv`:** Used to manage environment variables, keeping sensitive API keys secure.
* **Regular Expressions:** Used in the Custom Output Parser to reliably parse the LLM output.

<!-- TOC --><a name="challenges-and-difficulties"></a>
## Challenges and Difficulties

During the development of this project, several challenges were encountered:

* **Output Parsing Complexity:** Reliably parsing the LLM's output to extract actions, action inputs, and final answers required a custom parser (`ReActOutputParser`) using regular expressions to handle variations in LLM responses.
* **Agent Interaction Loop Management:** Implementing the loop to manage agent actions and observations required careful state management and handling of intermediate steps.
* **LangChain Evolution:** Keeping up with the rapid evolution of LangChain, including API changes and deprecations, required continuous learning and adaptation.
* **Error Handling:** Implementing robust error handling to gracefully handle unexpected LLM outputs or tool execution failures was essential for stability.
* **ReAct Pattern Implementation:** Correctly implementing the ReAct pattern and debugging the agent's reasoning process required a deep understanding of the pattern and careful testing.
* **Handling AgentFinish and AgentAction:** Distinguishing between the Agent's decision to use a tool, and the Agent's decision to provide a final answer was a complex parsing challenge.

<!-- TOC --><a name="future-business-impact-and-further-improvements"></a>
## Future Business Impact and Further Improvements

This project, while simple, demonstrates the foundational concepts of building LLM-powered agents, which have significant potential for commercial applications. Potential impacts and improvements include:

* **Automated Customer Support:** Agents can be developed to handle complex customer inquiries, providing personalized support and troubleshooting.
* **Data Analysis and Reporting:** Agents can automate data analysis tasks, generating reports and extracting insights from large datasets.
* **Task Automation:** Agents can automate repetitive tasks, such as scheduling, email management, and document processing, increasing efficiency.
* **Personalized Assistants:** Agents can create personalized assistants that can help users with various tasks, such as managing calendars, making reservations, and providing information.
* **Code Generation and Debugging:** Agents can assist developers in writing and debugging code, automating parts of the software development process.
* **Content Creation:** Agents can generate content for marketing, social media, and other purposes, freeing up human resources.
* **Enhanced Output Parsing:** Implement more robust output parsing to handle a wider range of LLM responses and edge cases.
* **Tool Expansion:** Integrate more tools to expand the agent's capabilities and enable it to perform a wider range of tasks.
* **Web Interface Integration:** Develop a web interface to provide a more interactive and user-friendly experience.
* **Improved Error Handling and Logging:** Implement more comprehensive error handling and logging to improve stability and debugging.
* **Unit and Integration Testing:** Add unit and integration tests to ensure the reliability and maintainability of the agent.
* **Contextual Memory:** Implement a form of memory to allow the agent to retain context across multiple interactions.
* **Agent Orchestration:** Expand to agent orchestration, allowing multiple agents to coordinate and solve complex problems.

<!-- TOC --><a name="target-audience-and-benefits"></a>
## Target Audience and Benefits

This project is primarily targeted at:

* **Developers:** Those interested in learning how to build LLM-powered applications using LangChain and OpenAI. They can use this as a starting point for more complex projects.
* **AI/ML Enthusiasts:** Individuals who want to explore the practical applications of LLMs and agents.
* **Students and Researchers:** Those studying AI and natural language processing, looking for hands-on examples.
* **Businesses:** Organizations looking to explore how LLMs can automate tasks and improve efficiency.

**Benefits:**

* Provides a clear and concise example of building a ReAct agent.
* Offers practical insights into using LangChain and OpenAI for agent development.
* Demonstrates how to create custom tools and manage agent interactions.
* Provides a foundation for building more complex LLM-powered applications.
* Shows how to secure API keys using environment variables.

<!-- TOC --><a name="advantages-and-disadvantages"></a>
## Advantages and Disadvantages

**Advantages:**

* **Educational Value:** Serves as an excellent learning resource for understanding LangChain agents and the ReAct pattern.
* **Customization:** Demonstrates how to create custom tools and tailor the agent's behavior.
* **Practical Example:** Provides a working example that can be easily adapted for other use cases.
* **Robust Output Parsing:** The custom parser ensures reliable handling of LLM outputs.

**Disadvantages:**

* **Simplicity:** The agent's functionality is limited to a single tool, which may not be representative of real-world applications.
* **Lack of Memory:** The agent does not retain context across multiple interactions.
* **Reliance on OpenAI API:** Requires an OpenAI API key, which may incur costs for high usage.
* **Limited Error Handling:** While some error handling is implemented, it could be further improved.

<!-- TOC --><a name="tradeoffs"></a>
## Tradeoffs

In this project, several tradeoffs were made to balance simplicity and functionality:

* **Single Tool Implementation:** The project focuses on demonstrating the core concepts of ReAct agents using a single, simple tool. This simplifies the code and makes it easier to understand, but limits the agent's capabilities.
* **Basic Prompt Design:** The prompt template is relatively simple to focus on the core logic of the agent. In a real-world scenario, more complex and nuanced prompts might be necessary.
* **Minimal Error Handling:** While basic error handling is implemented, the project prioritizes clarity over robustness. More comprehensive error handling would add complexity.
* **No Contextual Memory:** The agent does not retain context across interactions. This simplifies the project, but real-world agents would likely require some form of memory.

These tradeoffs were made to ensure the project remains accessible and understandable for beginners while still demonstrating the essential concepts of ReAct agents.

<!-- TOC --><a name="highlight-and-summary"></a>
## Highlight and Summary

This project provides a valuable introduction to building ReAct agents using LangChain and OpenAI. It demonstrates how to create custom tools, implement the ReAct pattern, and manage agent interactions. The custom output parser ensures reliable handling of LLM outputs. While the project is simple, it serves as a solid foundation for building more complex and sophisticated LLM-powered applications.

<!-- TOC --><a name="future-enhancements"></a>
## Future Enhancements

If this project were to be redone, several enhancements could be made:

* **Integration with More Tools:** Expand the agent's capabilities by integrating more tools, such as web search, database access, and API interactions.
* **Contextual Memory Implementation:** Implement a form of memory to allow the agent to retain context across multiple interactions.
* **Improved Error Handling and Logging:** Add more robust error handling and logging to improve stability and debugging.
* **Web Interface Development:** Create a web interface to provide a more interactive and user-friendly experience.
* **Agent Orchestration:** Explore agent orchestration, allowing multiple agents to coordinate and solve complex problems.
* **Advanced Prompt Engineering:** Experiment with more complex and nuanced prompts to improve the agent's reasoning and action execution.
* **Unit and Integration Testing:** Add comprehensive unit and integration tests to ensure the reliability and maintainability of the agent.
* **Dockerization:** Package the application in a Docker container for easier deployment and portability.
* **Configuration Management:** Implement a configuration management system to allow for easier customization and deployment.

<!-- TOC --><a name="prerequisites"></a>
## Prerequisites

Before running the project, ensure you have the following installed:

* Python 3.9+
* Poetry (recommended for dependency management) or pip
* OpenAI API key

<!-- TOC --><a name="setup"></a>
## Setup

1.  **Clone the Repository:**

    ```bash
    git clone https://github.com/junfanz1/react-langchain.git
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

<!-- TOC --><a name="code-explanation"></a>
## Code Explanation

<!-- TOC --><a name="mainpy"></a>
### `main.py`

* **Imports:** Imports necessary modules from LangChain, OpenAI, and other libraries.
* **`get_text_length` Tool:** Defines a custom tool that calculates the length of a given text.
* **`find_tool_by_name` Function:** Finds a tool by its name.
* **Prompt Template:** Creates a prompt template for the ReAct agent.
* **LLM Initialization:** Initializes the `ChatOpenAI` model with the OpenAI API key.
* **Agent Pipeline:** Constructs the agent pipeline, including the prompt, LLM, and custom output parser.
* **Agent Interaction Loop:** Implements a loop to handle agent actions and observations.
* **`ReActOutputParser` Class:** Defines a custom output parser to handle various agent responses, including actions and final answers.

<!-- TOC --><a name="requirementstxt"></a>
### `requirements.txt`

Lists the project's dependencies:

* `langchain`
* `langchain-openai`
* `langchain-community`
* `python-dotenv`

<!-- TOC --><a name="how-it-works"></a>
## How it Works

1.  The script initializes a ReAct agent with a custom tool (`get_text_length`).
2.  The agent receives a question: "What is the length of Dog in characters?".
3.  The agent uses the prompt template to generate a thought, action, and action input.
4.  The `ReActOutputParser` parses the agent's output.
5.  If the agent's output is an action, the script executes the tool and provides the observation back to the agent.
6.  If the agent's output is a final answer, the script prints the answer and exits.
7.  The process repeats until the agent provides a final answer.

<!-- TOC --><a name="custom-output-parser-reactoutputparser"></a>
## Custom Output Parser (`ReActOutputParser`)

The `ReActOutputParser` class is crucial for handling the agent's output. It uses regular expressions to parse the output and create either `AgentAction` or `AgentFinish` objects. This ensures that the script can correctly interpret the agent's responses.

<!-- TOC --><a name="future-improvements"></a>
## Future Improvements

* Add more tools to the agent.
* Implement error handling for tool execution.
* Improve the prompt template for better agent performance.
* Integrate with a web interface for a more interactive experience.
* Add more robust error handling.
* Add logging.
* Add unit tests.

<!-- TOC --><a name="contributing"></a>
## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue.

<!-- TOC --><a name="license"></a>
## License

This project is licensed under the MIT License.

## Reference

[Eden Marco: LangChain- Develop LLM powered applications with LangChain](https://www.udemy.com/course/langchain/?srsltid=AfmBOooPg0Xkc19q5W1430Dzq6MHGKWqHtq5a1WY4uUl9sQkrh_b_pej&couponCode=ST4MT240225B)
