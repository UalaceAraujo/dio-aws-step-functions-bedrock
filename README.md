# ⚡ Orquestração Serverless com AWS Step Functions e Amazon Bedrock

Repositório desenvolvido como parte do desafio prático do bootcamp da **DIO (Digital Innovation One)**, com foco em orquestração de microsserviços serverless e IA Generativa na AWS.

---

## 🎯 Objetivo do Projeto

Criar uma máquina de estados no **AWS Step Functions** para orquestrar chamadas a modelos fundacionais (LLMs) gerenciados pelo **Amazon Bedrock**, implementando fluxos de decisão, controle de estado e tratamento de falhas (*Catch/Retry*).

---

## 🛠️ Serviços AWS Explorados

| Serviço | Tipo | Aplicação |
| :--- | :--- | :--- |
| **AWS Step Functions** | Orquestrador Serverless | Gerenciamento de estados, paralelismo e controle de fluxo declarativo |
| **Amazon Bedrock** | IA Generativa | Acesso seguro e serverless a Foundation Models (LLMs) via API unificada |
| **Amazon States Language (ASL)** | Especificação JSON | Estruturação de regras e transições de estado no arquivo `workflow.json` |

---

## 🔄 Fluxo de Execução da Máquina de Estados

1. **`PrepararPrompt`:** Estado do tipo `Pass` que estrutura o payload inicial e os hiperparâmetros (temperatura e limite de tokens).
2. **`InvocarAmazonBedrock`:** Estado do tipo `Task` que executa a invocação direta do modelo (`arn:aws:states:::bedrock:invokeModel`).
3. **`Catch / TratamentoDeErro`:** Captura exceções em tempo de execução e direciona para um fluxo seguro de fallback.
4. **`FormatarResposta`:** Estado final que extrai o texto gerado pela LLM e padroniza a saída do fluxo.

---

## 💡 Insights e Aprendizados

- **Redução de Glue Code:** O Step Functions permite integrar serviços AWS nativamente sem necessidade de criar e manter funções Lambda adicionais apenas para repassar dados.
- **Resiliência e Observabilidade:** Tratamento automático de retentativas (*Retries*) e erros (*Catch*), com histórico visual detalhado de cada execução.
- **Arquitetura 100% Serverless:** Cobrança por transições de estado e volume de tokens, garantindo alta escalabilidade com custo sob demanda (*pay-as-you-go*).

---

## 👤 Autor

Desenvolvido por **Ualace Araújo**.

- **GitHub:** [UalaceAraujo](https://github.com/UalaceAraujo)
- **LinkedIn:** [Ualace Araújo](https://www.linkedin.com/in/ualacearaujo)
