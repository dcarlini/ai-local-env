# AI Local Environment

This project provides a flexible and customizable environment for running local AI models using Docker Compose. It includes configurations for running local LLMs and a web UI for interacting with them.

## Overview

This project provides a flexible and customizable environment for running local AI models using Docker Compose. It includes configurations for running local LLMs and a web UI for interacting with them.

### Tools Used

-   **[Ollama](https://ollama.com/)**: Ollama is used to run large language models locally. It allows you to pull, run, and manage LLMs like Llama 2, Code Llama, and others on your own machine.
-   **[LiteLLM](https://www.litellm.ai/)**: LiteLLM provides a unified interface to over 100 LLMs. It acts as a proxy, allowing you to switch between different models (local or remote) without changing your code.
-   **[Open WebUI](httpss://www.openwebui.com/)**: A user-friendly, extensible, and customizable web interface for interacting with local and remote LLMs. It supports various features like RAG, model management, and multi-user support.

-   **Stacks**: Pre-configured Docker Compose stacks for different AI services.
    -   `ai-localllm`: A stack for running local Large Language Models (LLMs) using Ollama.
    -   `ai-webui`: A stack for deploying a web-based UI (Open WebUI) to interact with the LLMs, along with LiteLLM for proxying requests.
-   **Configuration**: Centralized configuration management.
    -   `config/litellm`: Configuration for LiteLLM, which allows for a consistent interface to various LLMs.
    -   `config/sample.env`: A sample environment file to customize your setup.
-   **Shared Services**: A `docker-compose-shared.yaml` file for defining services that are common across multiple stacks, such as a shared network.

## Getting Started

### Prerequisites

-   Docker
-   Docker Compose

### Installation

1.  **Clone the repository:**

    ```bash
    git clone <repository-url>
    cd ai-local-env
    ```

2.  **Configure your environment:**

    Copy the sample environment file and customize it to your needs:

    ```bash
    cp config/sample.env .env
    ```

    Update the `.env` file with your desired settings. This includes setting your OpenAI API key (if you plan to use OpenAI models) and generating a master key for LiteLLM.

## Usage

To run a specific stack, use the `docker-compose` command with the appropriate compose file.

### Running the Local LLM Stack

This stack runs Ollama and pulls a default model.

```bash
docker-compose -f stacks/ai-localllm/compose.yaml up -d
```

### Running the Web UI Stack

This stack runs the Open WebUI and LiteLLM.

```bash
docker-compose -f stacks/ai-webui/compose.yaml up -d
```

### Running the Full Stack

To run all services together, you can combine the compose files. This is the recommended way to run the project.

```bash
docker-compose -f docker-compose-shared.yaml -f stacks/ai-localllm/compose.yaml -f stacks/ai-webui/compose.yaml up -d
```

After running the full stack, you can access the Open WebUI at `http://localhost:3000`.

## Configuration

The configuration for the different services is located in the `config` directory.

-   **LiteLLM**: The `config/litellm/litellm_config.yaml` file is used to configure the LiteLLM proxy. You can add or remove models from this file to customize which LLMs are available through the proxy.
-   **Environment Variables**: The `.env` file is used to configure the services. The following variables are available:
    -   `OPENAI_API_KEY`: Your OpenAI API key.
    -   `LITELLM_MASTER_KEY`: A master key for LiteLLM to secure API access.
    -   `OLLAMA_BASE_URL`: The URL for the Ollama service.
    -   `LITELLM_BASE_URL`: The URL for the LiteLLM service.
    -   `OPENAI_API_BASE_URLS`: The base URL for the OpenAI API, which is set to the LiteLLM proxy.
    -   `OPENAI_API_KEYS`: The API keys for the OpenAI API, which is set to the LiteLLM master key.

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue.
