# Concessionaria de Automoveis

Sistema de gestao para concessionaria com backend em Go (Gin + GORM), frontends em React e servicos auxiliares em Python (e-mail, dashboard e nota fiscal).

## Visao geral

Este repositorio concentra varios modulos:

- API principal em Go para carros, funcionarios, compras, vendas e fluxo de cliente.
- Frontend principal em React + Vite para operacao do sistema.
- Frontend legado em React (Create React App).
- Frontend simples em JavaScript para CRUD de funcionarios.
- Servico Python para envio de e-mail de verificacao.
- Servico Python para geracao/envio de nota fiscal em PDF.
- Dashboard em Streamlit para analise de dados.

## Stack

- Go 1.24
- Gin
- GORM
- PostgreSQL (configurado no codigo atual)
- React + Vite
- Python (Flask, Streamlit)
- Docker / Docker Compose

## Estrutura principal

```text
.
|- app.go
|- config/
|- carro/
|- funcionario/
|- cliente/
|- compra/
|- venda/
|- frontend-react/
|- concessionaria/
|- frontend/
|- dashboard-python/
|- email-automatico-python/
|- nota-fiscal-python/
|- docker-compose.yml
|- Dockerfile
```

## API Go

### Endpoints principais

Base local esperada: http://127.0.0.1:5002

- Carros
  - GET /carros
  - POST /carros
  - GET /carros/:placa
  - PUT /carros/:placa
  - DELETE /carros/:placa
- Funcionarios
  - GET /funcionarios
  - POST /funcionarios
  - GET /funcionarios/:id
  - PUT /funcionarios/:id
  - DELETE /funcionarios/:id
- Compras
  - GET /compras/
  - POST /compras/
  - GET /compras/:id
  - PUT /compras/:id
  - DELETE /compras/:id
- Vendas
  - GET /vendas/
  - POST /vendas/
  - GET /vendas/:id
  - PUT /vendas/:id
  - DELETE /vendas/:id
- Cliente
  - POST /clientes/:id/reenviar-email
  - GET /verificar-email

### Rodar localmente

1. Instale Go 1.24+
2. Na raiz do projeto:

```bash
go mod tidy
go run app.go
```

3. A API deve subir em 127.0.0.1:5002 (conforme app.go atual).

## Frontend principal (Vite)

Pasta: frontend-react

```bash
cd frontend-react
npm install
npm run dev
```

Por padrao, o frontend usa a API em porta 5002 quando em localhost.

## Frontend legado (CRA)

Pasta: concessionaria

```bash
cd concessionaria
npm install
npm start
```

## Frontend simples (JS puro)

Pasta: frontend

Abra o HTML/arquivos estaticos via Live Server (ou servidor estatico). Esse frontend aponta para API na porta 5001 por padrao.

## Servicos Python

### 1) E-mail automatico

Pasta: email-automatico-python

```bash
cd email-automatico-python
python -m venv .venv
# Windows
.venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Servidor Flask: http://localhost:5000

Endpoint:

- POST /send-email

Necessita variaveis SMTP em arquivo .env (ex.: SMTP_SERVER, SMTP_PORT, SMTP_USER, SMTP_PASSWORD, SMTP_FROM).

### 2) Nota fiscal automatizada

Pasta: nota-fiscal-python

```bash
cd nota-fiscal-python
python -m venv .venv
# Windows
.venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Servidor Flask: http://localhost:5001

Endpoint:

- POST /gerar-nota-fiscal?venda_id={id}

### 3) Dashboard

Pasta: dashboard-python

```bash
cd dashboard-python
python -m venv .venv
# Windows
.venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

Dashboard: http://localhost:8501

## Docker

Existe Dockerfile para a API Go e docker-compose.yml no projeto.

Comandos basicos:

```bash
docker compose up --build
docker compose down -v
```

Observacao importante: o compose atual esta configurado com MySQL, enquanto a API Go atual conecta em PostgreSQL (Supabase) no codigo. Se for usar Docker local, ajuste a conexao de banco para ficar consistente.

## Estado atual e observacoes

- Existem variacoes de porta entre modulos (5001, 5002, 6000). Padronizar isso evita falhas de comunicacao.
- Algumas credenciais estao hardcoded em codigo Python/Go. Mover tudo para variaveis de ambiente.

## Sugestao rapida de padronizacao

- API Go: 5002
- Frontend React (Vite): 5173 (padrao)
- E-mail Flask: 5000
- Nota Fiscal Flask: 5001
- Dashboard Streamlit: 8501

