# 🤖 BIA: Agente Financeiro Inteligente (Powered by n8n)

![n8n](https://img.shields.io/badge/Orquestrador-n8n-FF6D5A?style=for-the-badge&logo=n8n&logoColor=white)
![AI](https://img.shields.io/badge/IA-GPT--4-000000?style=for-the-badge&logo=openai&logoColor=white)
![Status](https://img.shields.io/badge/Status-Conclu%C3%ADdo-success?style=for-the-badge)

## 📌 Contexto do Projeto

Os assistentes virtuais no setor financeiro estão evoluindo de simples chatbots reativos para **agentes inteligentes e proativos**. Este repositório contém a entrega do desafio "BIA do Futuro" da DIO. 

A solução desenvolvida é a **BIA (Bank Intelligence Assistant)**, um agente financeiro orquestrado não por scripts complexos, mas através de **Agentic Workflows no n8n**. A BIA utiliza IA Generativa para:

- **Antecipar necessidades:** Lendo históricos de transações antes mesmo do usuário pedir.
- **Personalizar:** Cruzando o saldo disponível com o perfil de risco do cliente.
- **Garantir segurança (Zero Alucinação):** Usando o n8n para forçar o LLM a consumir apenas o catálogo oficial de produtos em formato JSON.

---

## 🚀 O Que Foi Entregue

A documentação completa do funcionamento, lógica e testes da BIA está dividida nos seguintes arquivos:

### 1. Documentação do Agente
Definição do problema, persona, tom de voz e a arquitetura visual do fluxo de dados rodando no n8n.
📄 **Acesse:** [`docs/01-documentacao-agente.md`](./docs/01-documentacao-agente.md)

### 2. Base de Conhecimento
Como o agente acessa e consome os dados simulados do banco (transações, perfil e produtos) via *Tools* do n8n.
📄 **Acesse:** [`docs/02-base-conhecimento.md`](./docs/02-base-conhecimento.md)

### 3. Engenharia de Prompts
O *System Prompt* rigoroso que dita as regras de negócio e os cenários de *Few-Shot Prompting* para lidar com tentativas de fraude ou perguntas fora do escopo.
📄 **Acesse:** [`docs/03-prompts.md`](./docs/03-prompts.md)

### 4. Avaliação e Métricas
Como o agente foi testado utilizando os logs de execução do n8n para garantir aderência ao perfil do cliente.
📄 **Acesse:** [`docs/04-metricas.md`](./docs/04-metricas.md)

### 5. Pitch de Apresentação
Roteiro de apresentação e demonstração da solução rodando em tempo real.
📄 **Acesse:** [`docs/05-pitch.md`](./docs/05-pitch.md)

---

## 🛠️ Aplicação Funcional (O Código)

Diferente de soluções baseadas em código puro (como Streamlit/Gradio), este projeto utiliza uma arquitetura **Low-Code**. O cérebro do agente está exportado no formato JSON, pronto para ser importado em qualquer instância do n8n.

📁 **Workflow do n8n:** [`src/bia_workflow.json`](./src/bia_workflow.json)

*Para testar: Basta copiar o conteúdo do JSON e colar em um canvas vazio do n8n.*

---

## ⚙️ Ferramentas Utilizadas

| Categoria | Ferramenta Escolhida e Motivo |
|-----------|-------------------------------|
| **Orquestração** | **[n8n](https://n8n.io/)**: Escolhido pela sua capacidade nativa de criar *Advanced AI Agents* com uso de ferramentas (Tools) e memória de forma visual e segura. |
| **LLM (Cérebro)** | **OpenAI (GPT-4o)**: Conectado via nó nativo no n8n, configurado com temperatura baixa (0.1) para foco analítico. |
| **Integração/Chat** | **n8n Chat Trigger**: Interface de testes nativa para simular a conversa com o usuário sem necessidade de desenvolver um front-end do zero. |

---

## 📂 Estrutura do Repositório

```text
📁 lab-agente-financeiro/
│
├── 📄 README.md                        # Este documento
│
├── 📁 data/                            # Dados mockados consumidos pela IA
│   ├── historico_atendimento.csv       
│   ├── perfil_investidor.json          
│   ├── produtos_financeiros.json       
│   └── transacoes.csv                  
│
├── 📁 docs/                            # Documentação técnica do projeto
│   ├── 01-documentacao-agente.md       
│   ├── 02-base-conhecimento.md         
│   ├── 03-prompts.md                   
│   ├── 04-metricas.md                  
│   └── 05-pitch.md                     
│
└── 📁 src/                             # Código da aplicação
    └── bia_workflow.json               # Fluxo exportado do n8n
