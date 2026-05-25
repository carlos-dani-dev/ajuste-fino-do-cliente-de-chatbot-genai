# Chatbot Gemini CLI, Web e API

<p>
  Aplicação de chatbot com backend em FastAPI, persistência em SQLite e clientes para terminal e web.<br>
  O projeto aceita Gemini ou Ollama como provedor de LLM, com histórico de conversa, contagem simplificada de tokens e endpoints de consulta para depuração.
</p>

### A atualização mais recente

O backend foi atualizado para usar a API mais nova do SDK oficial da Google para Python, `google-genai`. Em vez do fluxo antigo baseado em configuração global, o cliente agora é criado com `genai.Client(...)` e a geração é feita por `client.models.generate_content(...)`. Isso deixa a integração mais generalista e alinhada com o SDK atual, além de preparar o projeto para evoluções futuras da API.

### Features principais

- Integração com Gemini via `google-genai`
- Suporte a Ollama como alternativa local
- API REST com FastAPI
- Persistência de conversas em SQLite via SQLAlchemy
- Cliente de terminal para interação rápida
- Cliente web para uso no navegador
- Histórico recente para contexto da resposta
- Estimativa de tokens por mensagem
- Health check e listagem de modelos disponíveis

### A estrutura de diretórios da aplicação 

```text
chatbot_cli_gemini/
├── backend/
│   ├── app/
│   │   ├── main.py
│   │   ├── gemini_client.py
│   │   ├── ollama_client.py
│   │   ├── database.py
│   │   ├── models.py
│   │   ├── schemas.py
│   │   ├── config.py
│   │   └── llm_client_interface.py
│   ├── list_models.py
│   ├── requirements.txt
│   └── .env
├── client/
├── chat-frontend/
├── start_backend.sh
├── start_client.sh
├── start_client.bat
└── start_web.sh
```

### Requisitos

- Python 3.13 ou superior
- `uv` para execução e gerenciamento do ambiente
- Uma chave de API do Gemini, se for usar o provedor `gemini`
- Um servidor Ollama local, se for usar o provedor `ollama`

### Configurações iniciais e instalação

<p>O arquivo `backend/.env` deve possuir uma configuração semelhante à seguinte:</p>

```bash
DATABASE_URL=sqlite:///./chatbot.db

# gemini | ollama
LLM_PROVIDER=gemini

GEMINI_API_KEY=sua_chave_gemini
GEMINI_MODEL=gemini-2.5-flash

OLLAMA_BASE_URL=http://localhost:11434
OLLAMA_MODEL=qwen3
OLLAMA_TIMEOUT=120
OLLAMA_TEMPERATURE=0.7
OLLAMA_NUM_PREDICT=2048
```

### Instalação

No diretório raiz do projeto:

```bash
uv sync
```

<p>Se preferir usar um ambiente virtual manualmente, instale as dependências de cada pasta com os respectivos `requirements.txt`.</p>

### Para executar a aplicação

No Linux ou WSL:

```bash
chmod +x start_backend.sh
chmod +x start_client.sh
chmod +x start_web.sh
```

Inicie o backend:

```bash
./start_backend.sh
```

Inicie o cliente de terminal:

```bash
./start_client.sh IP_DO_SERVIDOR
```

Inicie o cliente web:

```bash
./start_web.sh IP_DO_SERVIDOR
```

No Windows, use:

```bat
start_client.bat IP_DO_SERVIDOR
```

Também é possível subir o backend diretamente:

```bash
cd backend
uv run uvicorn app.main:app --host 0.0.0.0 --port 8000
```

### A principais rotas da aplicação

- `POST /chat` - envia uma mensagem e recebe a resposta do provedor configurado
- `GET /conversations/{session_id}` - recupera a conversa salva no banco
- `GET /health` - verifica se a API está ativa e qual provedor está em uso
- `GET /models` - lista modelos disponíveis conforme o provedor

### Observações importantes

<p>
  O histórico usado como contexto considera apenas as mensagens mais recentes da conversa. O banco registra as mensagens persistidas, mas a experiência do cliente continua sendo orientada por sessão.
</p>
<p>
  Se quiser adaptar o projeto para outro modelo Gemini, basta trocar a variável `GEMINI_MODEL` no arquivo `.env`.
</p>
