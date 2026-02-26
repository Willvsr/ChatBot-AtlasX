# 🤖 Atlas - Assistente de Logística AtlasX

Este repositório contém a solução desenvolvida para o desafio de atendimento automatizado da **AtlasX Logística Integrada**. O bot foi construído na plataforma Blip utilizando uma arquitetura modular e integração com IA Generativa.

Figma do projeto: https://www.figma.com/design/nBsnzRl35WjF88dRZU4Odz/AtlasX-Log%C3%ADstica-Integrada?node-id=0-1&t=SggnIv8b8aG9PBad-1

## 🏗️ Estrutura Modular
O projeto foi desenhado seguindo as boas práticas da Blip, dividido em blocos lógicos para facilitar a manutenção e escalabilidade:
* **Boas Vindas (BV):** Recepção e conformidade com LGPD.
* **Principal (P):** Menu de decisão inicial.
* **Identificação e Validação (IV):** Motor de consulta de documentos (CPF/CNPJ) via API.
* **Cadastro (C):** Fluxo de coleta de dados assistido por IA.
* **Apoio:** Módulos de Transbordo, Inatividade, Pesquisa CSAT e Cascata de Validação.

## 🧠 Decisões Arquiteturais
* **IA Generativa (Groq - Llama 3):** Utilizada no módulo de cadastro para extração de entidades (nome, e-mail, endereço) de forma fluida.
* **Resiliência e Retry Policy:** Implementação de até 3 retentativas automáticas em chamadas de API antes do erro persistente.
* **Tratamento de Erros:** "Cascata de Validação" para lidar com inputs não suportados (áudio, imagens) e falhas de integração.
* **Modelagem de Estados:** Encerramento cíclico que reconduz o utilizador ao início, garantindo prontidão para novos atendimentos.

---

## 📥 Como Importar o Projeto (Passo a Passo)

### 1. Criação do Roteador
1. No portal Blip, crie um novo contato inteligente do tipo **Roteador**.
2. Nomeie como: `Router - AtlasX`.

### 2. Configuração de Recursos (Resources)
Acesse o roteador -> **Configurações (...)** -> **Conteúdos** -> **Recursos**. Adicione os 3 recursos abaixo:
* **Chave:** `BotConfiguration` | **Tipo:** JSON | **Valor:** Conteúdo de `BotConfiguration.json`.
* **Chave:** `functionGetMenu` | **Tipo:** Texto | **Valor:** Conteúdo de `functionGetMenu.txt`.
* **Chave:** `ServiceScheduleRules` | **Tipo:** JSON | **Valor:** Conteúdo de `ServiceScheduleRules.json`.

### 3. Geração de Chaves e Connection URL
Para que a comunicação entre os bots funcione, você deve preencher o arquivo `BotConfiguration.json` com os dados coletados abaixo antes de salvá-lo nos Recursos:

1. **Chave do Roteador:** No roteador, acesse **Configurações** -> **Chaves de acesso** -> Gerar Nova Chave.
2. **Connection URL:** No roteador, acesse **Configurações** -> **Informações de conexão** -> Copie a "URL para enviar comandos".
3. **Chave do Transbordo:** No bot de transbordo, acesse **Configurações** -> **Chaves de acesso** -> Gerar Nova Chave.

> **Dica:** Recomenda-se utilizar o **VS Code** para editar o JSON e garantir que os valores das chaves e da URL estejam corretos antes de colar no Blip.

### 4. Criação dos Bots Subordinados
Crie **9 bots** do tipo Builder. **Nota importante:** Devido a limites de caracteres na criação, crie o bot de validação com o nome `IdentificaoeValidacao - AtlasX` (sem espaços). Você pode adicionar os espaços no nome exibido após a criação.
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
2. Ative: **Tracking automático** e **Utilizar contexto do roteador**.
3. Vá em **Versões** -> **Carregar fluxo** -> Selecione o arquivo JSON correspondente.
4. Clique no ícone de foguete (**Publicar fluxo**) no menu lateral esquerdo.

### 6. Conexão dos Serviços no Roteador
No `Router - AtlasX`, acesse **Serviços** e conecte os bots exatamente com estes nomes de serviço:

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

> **Nota importante:** O serviço `transbordo` deve ser configurado obrigatoriamente sem o redirecionamento automático para preservar a integridade da fila de atendimento humano.
---

## 🧪 Cenários de Teste
O projeto foi validado através de **19 cenários críticos**:
* Fluxos de sucesso em Cadastro e Consulta.
* Tratamento de documentos inválidos e clientes não encontrados.
* Recuperação de erros de API e inatividade.
* Transbordo humano com pesquisa de satisfação pós-atendimento.

*O detalhamento completo pode ser encontrado no arquivo `Cenários de Teste.pdf` na pasta `/docs`.*

## 🚀 Como Testar
1. No **Router - AtlasX**, acesse **Canais** -> **Blip Chat**.
2. Na aba **Instalação**, clique no link em: *"Seu chatbot está disponível aqui"*.
