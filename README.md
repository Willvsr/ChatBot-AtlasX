# 🤖 Atlas - Assistente de Logística AtlasX

Este repositório contém a solução desenvolvida para o desafio de atendimento automatizado da **AtlasX Logística Integrada**. O bot foi construído na plataforma Blip utilizando uma arquitetura modular e integração com IA Generativa.

## 🏗️ Estrutura Modular
O projeto foi desenhado seguindo as boas práticas da Blip, dividido em blocos lógicos para facilitar a manutenção e escalabilidade:
* [cite_start]**Boas Vindas (BV):** Recepção e conformidade com LGPD[cite: 4].
* [cite_start]**Principal (P):** Menu de decisão inicial[cite: 7].
* [cite_start]**Identificação e Validação (IV):** Motor de consulta de documentos (CPF/CNPJ) via API[cite: 16].
* [cite_start]**Cadastro (C):** Fluxo de coleta de dados assistido por IA[cite: 57].
* [cite_start]**Apoio:** Módulos de Transbordo, Inatividade, Pesquisa CSAT e Cascata de Validação[cite: 21, 56, 103, 89].

## 🧠 Decisões Arquiteturais
* [cite_start]**IA Generativa (Groq - Llama 3):** Utilizada no módulo de cadastro para extração de entidades (nome, e-mail, endereço) de forma fluida[cite: 61].
* [cite_start]**Resiliência e Retry Policy:** Implementação de até 3 retentativas automáticas em chamadas de API antes do erro persistente[cite: 49].
* [cite_start]**Tratamento de Erros:** "Cascata de Validação" para lidar com inputs não suportados (áudio, imagens) e falhas de integração[cite: 88].
* [cite_start]**Modelagem de Estados:** Encerramento cíclico que reconduz o utilizador ao início, garantindo prontidão para novos atendimentos[cite: 93].

---

## 📥 Como Importar o Projeto (Passo a Passo)

### 1. Criação do Roteador
1. No portal Blip, crie um novo contato inteligente do tipo **Roteador**.
2. Nomeie como: `Router - AtlasX`.

### 2. Configuração de Recursos (Resources)
Acesse o roteador -> **Configurações (...)** -> **Conteúdos** -> **Recursos**. Adicione:
* **Chave:** `BotConfiguration` | **Tipo:** JSON | **Valor:** Conteúdo de `BotConfiguration.json`.
* **Chave:** `functionGetMenu` | **Tipo:** Texto | **Valor:** Conteúdo de `functionGetMenu.txt`.
* **Chave:** `ServiceScheduleRules` | **Tipo:** JSON | **Valor:** Conteúdo de `ServiceScheduleRules.json`.

### 3. Geração de Chaves e Connection URL
Para que a orquestração funcione, você deve preencher o arquivo `BotConfiguration.json` com os dados abaixo antes de salvá-lo nos Recursos:
1. **Chave do Roteador:** Em `Router - AtlasX` -> **Configurações** -> **Chaves de acesso** -> Gerar Nova Chave.
2. **Connection URL:** Em `Router - AtlasX` -> **Configurações** -> **Informações de conexão** -> Copie a "URL para enviar comandos".
3. **Chave do Transbordo:** No bot `Transbordo - AtlasX` -> **Configurações** -> **Chaves de acesso** -> Gerar Nova Chave.

> **Dica:** Utilize um editor de código (como VS Code) para preencher o JSON e garantir que a sintaxe esteja correta.

### 4. Criação dos Bots Subordinados
Crie **9 bots** do tipo Builder. Utilize os nomes abaixo (o bot de validação deve ser criado sem espaços devido ao limite de caracteres):
* `Boas Vindas - AtlasX`
* `Principal - AtlasX`
* `IdentificaoeValidacao - AtlasX`
* `Cadastro - AtlasX`
* `Algo Mais - AtlasX`
* `Transbordo - AtlasX`
* `Pesquisa CSAT - AtlasX`
* `Cascata de Validacao - AtlasX`
* `Finalizacao - AtlasX`

### 5. Configuração e Publicação Individual
Para cada um dos 9 bots:
1. Acesse o **Builder** -> **Configurações** (engrenagem).
2. Ative as chaves: **Tracking automático** e **Utilizar contexto do roteador**.
3. Vá em **Versões** -> **Carregar fluxo** -> Selecione o arquivo JSON correspondente.
4. Clique no ícone de foguete (**Publicar fluxo**) no menu lateral esquerdo para ativar.

### 6. Conexão dos Serviços no Roteador
No `Router - AtlasX`, acesse **Serviços** e conecte os bots seguindo esta tabela:

| Serviço | Chatbot Correspondente | Configuração / Expiração |
| :--- | :--- | :--- |
| **boasVindas** | `Boas Vindas - AtlasX` | **Chatbot Principal** |
| **principal** | `Principal - AtlasX` | 86400 segundos |
| **identificacaoValidacao** | `IdentificaoeValidacao - AtlasX` | 86400 segundos |
| **cadastro** | `Cadastro - AtlasX` | 86400 segundos |
| **algoMais** | `Algo Mais - AtlasX` | 86400 segundos |
| **transbordo** | `Transbordo - AtlasX` | **Sem redirecionamento automático** |
| **pesquisa** | `Pesquisa CSAT - AtlasX` | 86400 segundos |
| **cascataValidacao** | `Cascata de Validacao - AtlasX` | 86400 segundos |
| **finalizacao** | `Finalizacao - AtlasX` | 86400 segundos |

---

## 🧪 Cenários de Teste
O projeto foi validado através de **19 cenários críticos**, garantindo a cobertura de:
* [cite_start]Fluxos de sucesso em Cadastro e Consulta[cite: 32, 68].
* [cite_start]Tratamento de documentos inválidos e clientes não encontrados[cite: 25, 42].
* [cite_start]Recuperação de erros de API e inatividade[cite: 51, 56].
* [cite_start]Transbordo humano com pesquisa de satisfação pós-atendimento[cite: 103].

[cite_start]*O detalhamento completo pode ser encontrado no arquivo `Cenários de Teste.docx` na pasta `/docs` [cite: 1-104].*

## 🚀 Como Testar
1. No **Router - AtlasX**, acesse **Canais** -> **Blip Chat**.
2. Na aba **Instalação**, clique no link em: *"Seu chatbot está disponível aqui"*.
