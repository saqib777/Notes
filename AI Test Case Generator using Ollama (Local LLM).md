

# AI Test Case Generator using Ollama (Local LLM)

## Introduction

This project is a fully functional AI-powered test case generator that converts simple human-readable feature descriptions into structured test steps. Instead of relying on paid cloud APIs, the system uses a locally running large language model through Ollama, making it completely free, private, and controllable.

At its core, this system bridges the gap between manual testing effort and automation by allowing a tester or developer to simply describe a feature in plain English and instantly receive meaningful test steps. This is especially useful in fast-paced development environments where writing test cases manually can become repetitive and time-consuming.

---

## Problem Statement

In traditional software testing workflows, creating test cases is often a manual process that requires careful thinking, consistency, and domain understanding. This leads to several challenges such as inconsistent quality, duplication of effort, and slower delivery cycles.

The goal of this project was to design a system that can take a simple input like:

"User login with email and password"

and automatically generate relevant test steps such as navigating to the login page, entering credentials, and validating behavior. The idea is not just automation, but intelligent assistance powered by AI.

---

## System Architecture

The system follows a clean and modular pipeline where each component has a clearly defined responsibility. The flow begins at the command-line interface, moves through prompt construction, interacts with the local LLM via an API, processes the response, and finally outputs structured test cases.

The pipeline works as follows:

CLI → Prompt Builder → LLM Client → Ollama → Response → Parser → Output

This separation of concerns makes the system easy to debug, extend, and maintain.

---

## Project Structure

The project is organized in a way that reflects real-world engineering practices. Each folder serves a specific purpose and contributes to the overall pipeline.

* The `cli` module acts as the entry point where the user provides input.
* The `ai_engine` module contains all logic related to AI interaction, including prompt building, API communication, and parsing.
* The `test_generator` module orchestrates the entire workflow by connecting all components.
* The `outputs` folder can be used to store generated results if needed.

This modular design ensures that changes in one part of the system do not break others, which is a key principle in scalable systems.

---

## Core Components Explained

### CLI Layer

The CLI is the user-facing part of the system. It accepts a feature description as input and triggers the test generation process. This is intentionally kept simple so that the focus remains on the AI pipeline.

A typical command looks like this:

python -m cli.main "User login with email and password"

This makes the tool lightweight and easy to use without requiring a UI.

---

### Generator Module

The generator acts as the orchestrator of the entire system. It takes the input from the CLI, builds a prompt, sends it to the LLM, and then processes the response.

It essentially connects all the moving parts together, ensuring smooth data flow between components. This layer is crucial because it defines how the system behaves end-to-end.

---

### Prompt Builder

The prompt builder is one of the most important parts of the system. It determines how the model understands the request and what kind of output it produces.

A well-designed prompt ensures that:

* The output is structured
* The response is consistent
* The model avoids unnecessary verbosity

This is where prompt engineering plays a critical role. Even small changes in prompt design can significantly affect the output quality.

---

### LLM Client (Ollama Integration)

This is the core engine of the system where the actual AI interaction happens. Instead of calling an external API like OpenAI, the system sends HTTP requests to a locally running Ollama server.

The API endpoint used is:

http://localhost:11434/api/generate

The request includes the model name, prompt, and configuration options such as temperature and response length.

One of the most critical configurations here is setting the response format to JSON. Without this, the model may return plain text, which breaks the parser and causes errors in the pipeline.

The use of debug logs in this module helped identify issues such as timeouts, server errors, and malformed responses during development.

---

### Parser

The parser is responsible for converting the raw output from the LLM into a usable structure. Since LLMs can sometimes return inconsistent or unexpected formats, this layer ensures that the final output is clean and usable.

It also handles errors gracefully, which is important for building a robust system.

---

## Ollama Setup and Usage

Ollama allows you to run large language models locally on your machine. This eliminates the need for API keys and usage costs.

The setup process involves installing Ollama, pulling a model such as gemma3:4b, and running it locally. Once running, the model listens on a local port and can be accessed via HTTP requests.

This project uses lightweight models like gemma3:4b because they are faster and more stable for local environments compared to heavier models.

---

## Challenges Faced and Solutions

During development, several real-world issues were encountered and resolved.

One major issue was GitHub blocking pushes due to exposed API keys. This was fixed by removing sensitive files and properly configuring the .gitignore file.

Another issue was dependency errors such as missing Python modules. Installing the required libraries resolved these problems.

There were also multiple runtime issues such as timeouts and server errors from Ollama. These were addressed by increasing request timeouts, ensuring the server was running, and correcting model names.

A critical debugging moment came when the API call itself was accidentally removed from the code. This caused silent failures and highlighted the importance of careful code structure and logging.

---

## Final Working Flow

The system now works seamlessly from input to output.

A user provides a feature description through the CLI. The system builds a structured prompt and sends it to the local LLM. The LLM generates a response, which is then parsed and displayed as structured test steps.

This entire process happens locally without any external dependencies, making it efficient and cost-free.

---

## Key Learnings

This project covers multiple important concepts relevant to an SDET role.

It demonstrates how to integrate AI into real systems, how to design modular architectures, and how to debug complex issues involving APIs, networking, and model behavior.

It also highlights the importance of secure development practices, especially when dealing with API keys and version control.

Perhaps most importantly, it shows how local AI models can be effectively used as an alternative to cloud-based solutions.

---

## Future Improvements

There are several ways this project can be extended further.

The output can be enhanced to include structured test case formats with IDs, priorities, and expected results. Export options such as CSV or Excel can be added for integration with testing tools.

A web-based UI can make the tool more accessible, and CI/CD integration can allow automatic test generation during development workflows.

Exploring different models can also improve performance and output quality.

---

## Conclusion

This project successfully demonstrates how AI can be used to automate test case generation in a practical and scalable way. By leveraging Ollama and local models, it eliminates cost barriers while maintaining flexibility and control.

What started as a simple idea has evolved into a complete system that reflects real-world engineering practices. This is not just a learning exercise but a strong foundation for building advanced AI-driven testing tools in the future.
