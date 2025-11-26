# MequiZap – Fluxo Inicial

Atendimento automatizado via WhatsApp para pedidos com retirada em SmartLockers do McDonald's, utilizando n8n, Evolution API e IA generativa.


## 📌 Visão Geral

O MequiZap automatiza o atendimento ao cliente pelo WhatsApp, guiando o usuário desde a primeira mensagem até a escolha do produto.
O fluxo recebe mensagens, processa com IA e envia respostas naturais e personalizadas.

Este documento descreve o fluxo inicial implementado no n8n.


## 🏗️ Arquitetura Resumida

Evolution API – Gateway para envio e recebimento de mensagens WhatsApp.
n8n – Orquestração do fluxo e integração entre componentes.
IA (Google Gemini) – Produz respostas humanizadas para atendimento.
Memória de Conversa – Mantém contexto usando o número do cliente.


## 🔄 Funcionamento do Fluxo

Recebimento da Mensagem
A Evolution API envia eventos para o webhook do n8n (/webhook/wpp).

*Extração dos Dados*
O fluxo captura:
Nome do cliente
Telefone
Mensagem enviada

Construção do Contexto
É montado um objeto JSON contendo:
``` json
{
  "Cliente": "<nome>",
  "Mensagem": "<mensagem>",
  "Método": "SmartLocker"
}
```


Processamento com IA
O Google Gemini responde com cordialidade, seguindo o prompt configurado para atendimento McDonald's.

Preparação da Resposta
O texto gerado é formatado e o número é ajustado para envio.

Envio ao WhatsApp
A resposta é enviada via Evolution API:

POST /message/sendText/n8n


## 🧠 Memória da Conversa

O fluxo utiliza um buffer de memória identificado pelo número do cliente, permitindo:

Continuidade natural da conversa
Personalização persistente
Respostas mais coerentes em interações longas


## 📦 Componentes do Fluxo

Webhook (entrada)

Set / Edit Fields – Limpeza e extração dos dados

AI Agent (LangChain) – Processo principal de atendimento

Google Gemini Chat Model – Modelo de linguagem

Memory Buffer – Histórico da conversa

HTTP Request – Envio de mensagens ao WhatsApp


## 🔐 Boas Práticas

Armazene credenciais em variáveis de ambiente

Proteja o webhook com token, IP allowlist ou proxy

Use HTTPS sempre que possível

Restrinja o acesso à Evolution API


## ▶️ Como Executar

Suba sua stack Docker:

docker compose up -d


Conecte o WhatsApp à Evolution API.

Importe o fluxo no n8n.

Ative o workflow.

Envie uma mensagem no WhatsApp para testar.