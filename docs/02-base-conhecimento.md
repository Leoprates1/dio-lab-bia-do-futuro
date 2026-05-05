# Base de Conhecimento (Knowledge Base)

## Dados Utilizados

Para garantir respostas precisas e livres de alucinações, a BIA foi desenvolvida para não depender exclusivamente do treino nativo do LLM. Em vez disso, ela consulta ficheiros locais mockados (simulados) que representam a base de dados de um banco. 

Os ficheiros utilizados encontram-se na pasta `data/` do projeto original:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Fornecer contexto sobre as interações passadas do cliente, garantindo um atendimento mais empático (ex: saber se o cliente teve um problema recente). |
| `perfil_investidor.json` | JSON | Identificar a tolerância ao risco do cliente (Conservador, Moderado, Arrojado) para personalizar as recomendações financeiras. |
| `produtos_financeiros.json` | JSON | Funciona como o catálogo exclusivo de produtos. O agente está bloqueado para sugerir apenas o que consta nesta lista. |
| `transacoes.csv` | CSV | Analisar o fluxo de caixa, identificar padrões de gastos excessivos (ex: delivery) e calcular o saldo disponível para investimentos. |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças (como séries temporais de ações ou dados abertos de Open Banking), desde que sejam adequados ao contexto de orquestração do n8n.

---

## Adaptações nos Dados

Neste projeto, mantivemos os dados estruturais propostos pela DIO para garantir a compatibilidade do desafio. No entanto, no cenário real orquestrado pelo **n8n**, a grande adaptação não ocorre no ficheiro em si, mas na forma como ele é lido:
* Os ficheiros CSV foram padronizados com cabeçalhos claros (`data`, `descricao`, `valor`, `categoria`) para facilitar o *parsing* (leitura) feito pelos nós do n8n antes de serem entregues ao LLM.

---

## Estratégia de Integração

A arquitetura do agente tira partido do conceito de **Agentic Workflows** (Fluxos Agênticos) construídos no n8n.

### Como os dados são carregados?
A BIA acede à base de conhecimento de forma ativa utilizando a funcionalidade de **Tools (Ferramentas)** do n8n.
* Não injetamos todos os dados de uma vez na conversa. O nó *Advanced AI Agent* do n8n está conectado a ferramentas auxiliares (ex: `Read File Node` ou integrações com Google Sheets).
* Quando o utilizador pergunta *"Quanto eu gastei com comida?"*, o LLM decide, de forma autónoma, acionar a ferramenta que lê o `transacoes.csv`, processa a resposta e devolve ao utilizador.

### Como os dados são usados no prompt?
Os dados de contexto fixo (como o Perfil do Investidor) são injetados dinamicamente no **System Prompt** logo no início da sessão. Os dados variáveis (como transações e produtos) são consultados sob demanda (RAG - *Retrieval-Augmented Generation* / Tool Calling).

---

## Exemplo de Contexto Montado

Quando a BIA recebe a mensagem do cliente, o fluxo do n8n compila invisivelmente as informações essenciais. Este é um exemplo do que o LLM vê nos bastidores antes de gerar a resposta:

```text
[INFORMAÇÕES DE SISTEMA INJETADAS]
Dados do Cliente:
- Nome: João Silva
- Perfil de Risco: Moderado
- Saldo disponível para investimento: R$ 5.000,00
- Nota de Atendimento Anterior: Cliente reportou lentidão no app (seja extra cordial).

Últimas transações (Novembro):
- 01/11: Supermercado - R$ 450,00
- 03/11: App de Transporte - R$ 120,00
- 04/11: Restaurante Fast Food - R$ 85,00
- 05/11: Streaming - R$ 55,00
...

Ação Requerida: O cliente perguntou "O que faço com o dinheiro que sobrou?". 
Lembre-se de verificar os gastos com transporte e fast food antes de sugerir produtos do catálogo 'produtos_financeiros.json'.
