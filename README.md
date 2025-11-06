# News API

API de notícias desenvolvida com FastAPI e Supabase.

## 🚀 Como rodar o projeto localmente

1. Instale as dependências:

```bash
pip install -r requirements.txt
```

2. Inicie o servidor:

```bash
uvicorn main:app --reload --port 8000
```

A API estará disponível em `http://localhost:8000`

## ☁️ Deploy (Render / Heroku / outras plataformas)

Plataformas como Render e Heroku expõem a porta da aplicação através da variável de ambiente `PORT`. Se o comando de start usar uma string literal (por exemplo `SPORT` sem `$`), o uvicorn receberá essa string e falhará com "Invalid value for '--port'".

Abaixo está um passo-a-passo específico para o Render, seguido por alternativas e exemplos.

### Deploy no Render (passo a passo)

1. No painel do Render, clique em "New" → "Web Service" e conecte seu repositório.
2. Em "Environment", escolha "Python".
3. Em "Build Command" informe:

```bash
pip install -r requirements.txt
```

4. Em "Start Command" informe (recomendado):

```bash
./start.sh
```

Alternativa: usar diretamente o uvicorn com a porta da plataforma:

```bash
uvicorn main:app --host 0.0.0.0 --port $PORT
```

5. Adicione as variáveis de ambiente (Settings → Environment) obrigatórias:

- `SUPABASE_URL` — URL do seu projeto Supabase
- `SUPABASE_ANON_KEY` — chave anônima (anon key)
- `TABLE_NEWS` — (opcional) nome da tabela (padrão: `news`)

6. Configure a Health Check (opcional, recomendado):

- Health check path: `/health`

7. Deploy. Se houver erro `Invalid value for '--port'` verifique o comando de start na dashboard — deve conter `$PORT` (com cifrão) ou usar `./start.sh`.

### Arquivos de apoio incluídos

- `start.sh`: script simples que usa `PORT` com fallback para `8000`.
- `Procfile`: compatível com plataformas que usam Procfile — contém `web: ./start.sh`.
- `render.yaml`: exemplo de configuração (opcional) para deploy automático.

### Exemplos rápidos

Procfile (incluso):

```
web: ./start.sh
```

Exemplo mínimo `render.yaml` (opcional):

```yaml
services:
	- type: web
		name: news-api
		branch: main
		buildCommand: pip install -r requirements.txt
		startCommand: ./start.sh
		envVars:
			- key: SUPABASE_URL
			- key: SUPABASE_ANON_KEY
			- key: TABLE_NEWS
```

### Dicas rápidas de troubleshooting

- Se o log mostrar `Error: Invalid value for '--port': 'SPORT' is not a valid integer.`, corrija o comando de start para usar `$PORT` (com `$`).
- Verifique se as variáveis de ambiente estão definidas no painel do Render (Settings → Environment).
- Use o endpoint `/health` para configurar health checks e garantir que o serviço esteja saudável.
