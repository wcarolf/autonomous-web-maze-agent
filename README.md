# autonomous-web-maze-agent
Autonomous AI Agent built in Python using a ReAct loop, Function Calling, and OpenRouter API to navigate web pages, parse HTML with BeautifulSoup, and solve a multi-step online maze with domain guardrails and error-handling recovery.

# Autonomous Web Maze Solver Agent

> **Autonomous AI Agent** built in Python on **Google Colab** using a **ReAct (Reasoning + Acting)** loop, **Function Calling**, and the **OpenRouter API** to crawl web pages, parse HTML with BeautifulSoup, and solve a multi-step online maze (`https://maze.edwarddonner.com`) with domain guardrails and automated error recovery.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1oo4GJg_RPw-NOrLufhBOiuy3SixUi1Eh?usp=sharing)

*[Leia a documentação em Português](#pt-br)*

---
*(#ENGLISH-VERSION)*
## Architecture & AI Engineering Highlights

Rather than relying on high-level agent frameworks that hide the underlying mechanics, this notebook builds a complete **Agent Harness** from scratch using the OpenAI Python SDK pointed at OpenRouter's free model router (`openrouter/free`):

* **ReAct Control Loop (`run_agent`):** Manages conversation state and memory (`messages`), alternating between LLM reasoning (`assistant`) and environment observation (`role: "tool"`) until the agent discovers the exit message.
* **Function Calling / Tool Use (`read_web_page_json`):** Defines a structured JSON schema describing the web-scraping tool and its required `url` parameter, enabling the LLM to autonomously decide when and where to navigate next without ever seeing the underlying Python code.
* **Domain Guardrails:** Enforces URL validation inside `read_web_page` (`url.startswith("https://maze.edwarddonner.com")`), preventing the agent from navigating outside the target maze environment.
* **Fault Tolerance & Self-Correction (`handle_tool_call`):** Wraps tool execution in a `try/except` block that catches HTTP or parsing errors and returns them as structured tool feedback, allowing the LLM to recover and choose an alternative path without crashing the runtime.
* **Loop Prevention:** Caps execution at `max_steps=25` to prevent infinite loops or runaway API calls.

<a name="pt-br"></a>

# Versão em Português
Agente de IA Autônomo desenvolvido em Python no Google Colab utilizando arquitetura ReAct (Raciocínio + Ação), Function Calling e a API da OpenRouter para navegar por páginas web, extrair texto limpo de HTML e resolver um labirinto online (https://maze.edwarddonner.com) até encontrar a mensagem final.

Arquitetura e Destaques de Engenharia de IA
Em vez de utilizar frameworks prontos que abstraem o funcionamento interno, este notebook constrói um Agent Harness completo diretamente sobre o SDK da OpenAI conectado ao roteador gratuito da OpenRouter (openrouter/free):

Loop de Orquestração ReAct (run_agent): Gerencia o histórico e o estado da conversa (messages), alternando entre o raciocínio do modelo (assistant) e o retorno das ferramentas (role: "tool") até atingir a condição de parada.

Function Calling (read_web_page_json): Define um schema JSON com o nome, a descrição e os parâmetros obrigatórios (url) da ferramenta de leitura web, permitindo que a LLM decida qual link acessar a cada passo sem enxergar o código Python.

Guardrails de Domínio: Trava de segurança em read_web_page que bloqueia qualquer requisição fora de https://maze.edwarddonner.com.

Tolerância a Falhas (handle_tool_call): Uso de try/except para capturar falhas de requisição ou URLs inválidas e devolvê-las como feedback para a própria LLM, permitindo que o agente corrija a rota sozinho sem interromper a execução.

Prevenção de Loop Infinito: Limite máximo de 25 passos (max_steps=25) por execução.

> Como Executar no Google Colab 
Não é necessário instalar nada no computador:

Clique no selo Open in Colab no topo desta página,

Crie uma chave de API gratuita na OpenRouter.

No menu lateral esquerdo do Google Colab, clique no ícone de Chave (🔑 Secrets).

Crie um segredo chamado OPENROUTER_API_KEY, cole sua chave (sk-or-v1-...) e ative a opção Acesso ao notebook.

Execute todas as células (Ambiente de execução > Executar tudo) para validar a chave e acompanhar o agente resolvendo o labirinto passo a passo.
---

## How the Agent Loop Works - ## Como o Agente Funciona

```text
[User Task] ──▶ [LLM Reasoning (OpenRouter)]
                       │
            ┌──────────┴──────────┐
            ▼                     ▼
     (Needs Web Page)      (Found Exit / Answer)
            │                     │
            ▼                     ▼
  [Tool Call JSON: url]    [Final Output & Stop]
            │
            ▼
  [Python Harness: read_web_page()]
   ├─ Domain Guardrail Check
   ├─ Requests + BeautifulSoup HTML Parsing
   └─ Try/Except Error Packaging
            │
            ▼
  [Append role="tool" to Messages] ──▶ (Back to LLM)

autonomous-web-maze-agent/
├── maze_agent_notebook.ipynb   # Annotated Google Colab notebook with step-by-step validation and agent loop
└── README.md                   # Project documentation

> How to Run in Google Colab
No local installation is required. You can run this agent directly in your browser:

Click the Open in Colab badge at the top of this README (or open maze_agent_notebook.ipynb and click Open in Colab).

Create a free API key at OpenRouter.

In Google Colab, open the Secrets panel (the key icon 🔑 in the left sidebar).

Add a new secret named OPENROUTER_API_KEY, paste your key (sk-or-v1-...), and toggle Notebook access ON.

Run all cells (Runtime > Run all) to validate the API connection and watch the agent navigate the maze step-by-step.

Acknowledgments
Developed as a hands-on AI Engineering portfolio study inspired by Ed Donner's agentic workflow lab and interactive web maze challenge.

===================================================================================================================================================================================================
