🤖 Atlas - Assistente de Logística AtlasX
Este repositório contém a solução desenvolvida para o desafio de atendimento automatizado da AtlasX Logística Integrada. O bot foi construído na plataforma Blip utilizando uma arquitetura modular e integração com IA Generativa.

🏗️ Estrutura Modular
O projeto foi desenhado seguindo as boas práticas da Blip, dividido em blocos lógicos:
Boas Vindas (BV): Recepção e conformidade com LGPD.
Principal (P): Menu de decisão inicial.
Identificação e Validação (IV): Motor de consulta de documentos (CPF/CNPJ) via API.
Cadastro (C): Fluxo de coleta de dados assistido por IA.
Apoio: Módulos de Transbordo, Inatividade, Pesquisa CSAT (pós-humano) e Cascata de Validação.

🧠 Decisões Arquiteturais

IA Generativa (Groq - Llama 3): Utilizada para extração de entidades (endereço, nome, e-mail) de forma fluida, garantindo uma UX superior à coleta tradicional.
Resiliência e Retry Policy: Implementação de até 3 retentativas automáticas em chamadas de API antes do redirecionamento para erro persistente.
Tratamento de Erros: "Cascata de Validação" para lidar com inputs não suportados (áudio, imagens) e falhas de integração.
Modelagem de Estados: Encerramento cíclico que reconduz o usuário ao início do fluxo, garantindo que o bot esteja sempre pronto para uma nova interação.

📥 Como Importar o Projeto (Passo a Passo)
Para garantir que o Atlas funcione com todas as suas funcionalidades (IA, menus dinâmicos e horários), siga rigorosamente a ordem abaixo:

1. Criação do Roteador
No portal Blip, crie um novo contato inteligente do tipo Roteador.

Nomeie como: Router - AtlasX.

2. Configuração de Recursos (Resources)
Antes de importar o fluxo, você precisa configurar os "motores" do bot.

Acesse o seu roteador e clique no ícone de três pontos (...) no menu superior.

Selecione Conteúdos e, no menu lateral esquerdo, clique em Recursos.

Adicione os 3 recursos abaixo exatamente com as mesmas chaves:

Chave: BotConfiguration

Tipo: JSON

Valor: (Copie e cole o conteúdo do arquivo BotConfiguration.json disponível na pasta /json/resources deste repositório).

Chave: functionGetMenu

Tipo: Texto

Valor: (Copie e cole o script disponível no arquivo functionGetMenu.txt).

Chave: ServiceScheduleRules

Tipo: JSON

Valor: (Copie e cole as regras de horário contidas em ServiceScheduleRules.json).

3. Criação dos Bots Subordinados
Crie 9 bots do tipo "Builder" dentro do seu roteador. Para que as transferências de estado funcionem, utilize exatamente os nomes abaixo:


Boas Vindas - AtlasX 


Principal - AtlasX 


IdentificaoeValidacao - AtlasX 


Cadastro - AtlasX 


Algo Mais - AtlasX 


Transbordo - AtlasX 


Pesquisa CSAT - AtlasX 


Cascata de Validacao - AtlasX 


Finalizacao - AtlasX 

4. Importação do Fluxo (Tutorial para cada Bot)
Para cada um dos 9 bots criados, realize o seguinte procedimento:

Acesse o Builder do bot.

Clique no ícone de Configurações (engrenagem no canto esquerdo da página).

No menu lateral que será aberto à direita, ative as chaves:


Tracking automático 


Utilizar contexto do roteador 

Ainda no menu lateral, clique na aba Versões.

Clique em Carregar fluxo e selecione o arquivo JSON correspondente ao bot (ex: boas_vindas.json).

Clique em Sim na janela de confirmação de sobreposição.

Importante: Clique no botão Publicar fluxo no canto esquerdo (ícone de foguete) para salvar as alterações.

5. Conexão dos Serviços ao Roteador (Router - AtlasX)
Esta etapa interliga os módulos e define o Boas Vindas como a porta de entrada.

Acesse o Router - AtlasX e clique em Serviços no menu superior.

Clique em Adicionar um serviço para conectar cada bot individualmente.

O nome de cada serviço deve ser preenchido exatamente conforme a lista abaixo para garantir que os redirecionamentos de estado funcionem:

| Serviço | Chatbot Correspondente | Configuração / Expiração |
| :--- | :--- | :--- |
| **boasVindas** | `Boas Vindas - AtlasX` | [cite_start]**Chatbot Principal** (Porta de entrada/LGPD) [cite: 1] |
| **principal** | `Principal - AtlasX` | 86400 segundos |
| **identificacaoValidacao** | `Identificacao e Validacao - AtlasX` | 86400 segundos |
| **cadastro** | `Cadastro - AtlasX` | 86400 segundos |
| **algoMais** | `Algo Mais - AtlasX` | 86400 segundos |
| **transbordo** | `Transbordo - AtlasX` | **Sem redirecionamento automático** |
| **pesquisa** | `Pesquisa CSAT - AtlasX` | 86400 segundos |
| **cascataValidacao** | `Cascata de Validacao - AtlasX` | 86400 segundos |
| **finalizacao** | `Finalizacao - AtlasX` | 86400 segundos |

> **Nota importante:** O serviço `transbordo` deve ser configurado obrigatoriamente sem o redirecionamento automático para preservar a integridade da fila de atendimento humano.

Após concluir todas as configurações acima, você pode testar a jornada completa do Atlas seguindo estes passos:

No menu superior do seu Router - AtlasX, acesse a aba Canais.

Clique em Blip Chat.

Vá até a aba Instalação.

Clique no link embutido na frase: "Seu chatbot está disponível aqui".

Interaja com o bot para validar os fluxos de Boas-Vindas, Identificação, Consulta/Cadastro e Transbordo.
