# Orquestração Serverless com AWS Step Functions e Amazon Bedrock

Projeto desenvolvido para demonstrar a orquestração declarativa de fluxos de Inteligência Artificial Generativa em ambiente serverless na Amazon Web Services (AWS), integrando pipelines de execução orientados a eventos e modelos fundacionais.

## Visão Geral da Arquitetura

O projeto consiste na implementação de uma Máquina de Estados (*State Machine*) gerenciada pelo **AWS Step Functions**, responsável por estruturar a esteira de inferência de Large Language Models (LLMs) via **Amazon Bedrock**. A solução adota integração de serviço nativa (*Direct SDK Integration*), eliminando a necessidade de funções AWS Lambda intermediárias (*glue code*) e assegurando baixa latência, rastreabilidade e governança de dados.

## Serviços e Especificações Técnicas

| Componente | Categoria | Finalidade no Projeto |
| :--- | :--- | :--- |
| **AWS Step Functions** | Orquestração Serverless | Controle de fluxo, transição determinística de estados e auditoria visual |
| **Amazon Bedrock** | IA Generativa Gerenciada | Consumo unificado e padronizado de Foundation Models (FMs) sob demanda |
| **Amazon States Language (ASL)** | Especificação JSON-based | Declaração determinística das regras de negócio e topologia do fluxo |
| **AWS IAM** | Segurança e Identidade | Políticas de privilégio mínimo para execução do Step Functions sobre o Bedrock |

## Topologia e Fluxo de Execução

A definição do workflow (`workflow.json`) implementa os seguintes estados:

1. **`PrepararPrompt` (`Pass`):** Formata o payload de entrada, injeta o contexto da requisição e parametriza hiperparâmetros de inferência (como `temperature`, `top_p` e `max_tokens`).
2. **`InvocarAmazonBedrock` (`Task`):** Executa a chamada síncrona diretamente à API do Bedrock utilizando o recurso integrado `arn:aws:states:::bedrock:invokeModel`.
3. **`TratamentoDeExcecoes` (`Catch/Retry`):** Implementa políticas de *backoff* exponencial para lidar com limites de taxa (*throttling*) e redirecionamento para estados de contingência em caso de indisponibilidade transitória.
4. **`FormatarResposta` (`Pass`):** Realiza a filtragem JSONPath na resposta bruta do modelo, estruturando o output final consolidado.

## Padrões de Projeto e Benefícios

- **Eliminação de Código Intermediário (*No-Glue-Code Architecture*):** A integração nativa do Step Functions simplifica a manutenção ao conectar diretamente o motor de orquestração à API do Bedrock.
- **Resiliência e Tolerância a Falhas:** Definição declarativa de tratamento de erros com políticas avançadas de repetição automática sem poluição da lógica de negócio.
- **Observabilidade Granular:** Auditoria de execução estado por estado via console AWS e integração nativa com o Amazon CloudWatch.
- **Eficiência Operacional e Custos:** Modelo serverless puro, cobrado estritamente pelo número de transições de estado executadas e pelos tokens processados pela LLM.

## Estrutura do Repositório

```text
├── workflow.json         # Definição da máquina de estados em Amazon States Language (ASL)
├── sample-payload.json   # Carga útil de exemplo para execução e testes de inferência
└── README.md             # Documentação técnica e arquitetural da solução
