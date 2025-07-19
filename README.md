# AI Local Environment

This project provides a flexible and customizable local AI environment running in Docker.

- The main functionality is to allow you to interact with local and external API models using the same Web interface.  
- It also allows you to integrare with MCP providers to augment the AI models with your own data.
- In future interactions, I plan to add additional related sercices such as N8N using the same underlying structure.

## Tools

  -   **[Ollama](https://ollama.com/)**: Ollama is used to run large language models locally. Models vary in ability and sizes and can be found at [Ollama Models](https://ollama.com/search). Running models is hightly hardware dependent and works best on a machine with a GPU with lots of memory or a Modern Mac with a M chip due to it's unified memory architecture. This currently includes qwen2.5:7b-instruct-q4_K_M which works well enough for MCP usage.                                                    
  -   **[LiteLLM](https://www.litellm.ai/)**:                 LiteLLM provides a unified API interface to over 100          LLMs. It acts as a proxy, allowing you to switch             between different models without changing your code.  In this case, Open Web UI is configured to connect to LiteLLM to allow it access to external models                                        
  -   **[Open WebUI](httpss://www.openwebui.com/)**:           A user-friendly, extensible, and customizable web interface for interacting with LLMs. The UI is similar chatGPT but supports various features like RAG, model management, and multi-user support.                          
     

## Overview

The project is structured into the following main components:

-   **Shared Services Docker Compose**: A `docker-compose-shared.yaml` file for defining services that are common across multiple stacks. Currently it defines a docker bridge network which all services run in

-   **Stacks**: Pre-configured Docker Compose stacks for different services.
    -   `ai-localllm`: includes Ollama.  This is it's own stack as some users may want use external LLMs exclusively or outside of docker on a more powerful machine.
    -   `ai-webui`: includes Open Web UI and Lite LLM 
   
-   **Configuration**: Centralized configuration management.
    -   `config/sample.env`: A sample environment file to customize your setup.
    -   `config/litellm/sample-litellm_config.yaml`: LiteLLM config file. Sample includes OpenAI models such as GPT 4.O but can be configued to add many others such as Gemini, Antrophic, Hugging Face Models  More information of support providers & models can be found on the [LiteLLM documentation](https://docs.litellm.ai/docs/) is vast 
    

## Getting Started

### Prerequisites

-   a machine that has docker and docker compose installed.

### Installation

#### 1.  **Clone the repository:**

    ```bash
    git clone https://github.com/dcarlini/ai-local-env
    cd ai-local-env
    ```

#### 2.  **Configure your environment:**

##### Copy the sample environment file and customize it to your needs:

    ```bash
    cd config
    cp sample.env .env
    ```

##### Modify `.env` setting necessary environment variables 
    
Most values provided work out of the box but you will need to set the following variables at
    
- OPENAI_API_KEY - This is the api key for your [OpenAI](https://openai.com/) API Developer Account
- LITELLM_MASTER_KEY - This is a generated api key to secure your instance of LiteLLM
- Optionally you can provide other keys to connect to other services if you config such models in LiteLLM config
    - GEMINI_API_KEY
    - ANTHROPIC_API_KEY

If you prefer not to provide keys in a environment file set the variables on the command line on your docker host or for better security connect to key store.  For simiplicity, instructions are not provided here. 

##### Copy the sample litellm config file and customize it to your needs:

    ```bash
    cd litellm
    cp sample-litellm_config.yaml litellm_config.yaml
    ```

##### Customize LiteLLM Config
The sample works out of the box and gives you access to two GPT models assuming you have a working OpenAPI Developer Account.  

The file can be blank if no external access is needed.  

Exact configuration is outside the scope of this document but is documented at
- [LiteLLM Model Management](https://docs.litellm.ai/docs/proxy/model_management)
- [LiteLLM Providers](https://docs.litellm.ai/docs/providers)

## Usage

The docker compose files are split into stacks. This allows you to pick and choose which services to run.   

You can run it all or piecemeal. The shared docker compose is required to be run first and sets up a shared network so that the services can interact.

### Run it all

```bash
docker-compose -f docker-compose.yml -f stacks/ai-localllm/docker-compose.yml -f stacks/ai-mcpo/docker_compose.yml -f stacks/ai-webui/docker_compose.yml up -d
```

### Run the shared components

```bash
docker-compose up -d
```

### Run stacks
To run a specific stack, use the `docker-compose` command with the appropriate compose file.

#### Run the Local LLM Stack

```bash
docker-compose -f stacks/ai-localllm/docker-compose.yml up -d
```

#### Run the MCP Stack

```bash
docker-compose -f stacks/ai-mcpo/docker-compose.yml up -d
```

#### Run the Web UI Stack

```bash
docker-compose -f stacks/ai-webui/docker-compose.yml up -d
```

## Access URLs

Once the services are running, you can access them at the following URLs:

-   **Open WebUI**: [http://localhost:3000](http://localhost:3000)
-   **LiteLLM API**: [http://localhost:4000](http://localhost:4000)
-   **Ollama API**: [http://localhost:11434](http://localhost:11434)

Replace localhost with the server IP if running on a different machine 

On your first login to Open WebUI you will need to setup a admin user. Simily fill out the form providing a name, email and password. 

## Related Links
- [Open WebUI Documentation](https://docs.openwebui.com/)
- [LiteLLM documentation](https://docs.litellm.ai/docs/)
- [Ollama Models](https://ollama.com/search)
- [MCP Servers GitHub](https://github.com/modelcontextprotocol/servers) 
