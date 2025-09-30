#  AWS Step Functions - Orquestração Serverless

Desafio do Bootcamp DIO - Documentação sobre workflows automatizados com AWS Step Functions
Este repositório documenta meus estudos sobre AWS Step Functions, explorando a criação de workflows visuais e orquestração de serviços AWS de forma low-code.

## O que são AWS Step Functions?
AWS Step Functions é um serviço de orquestração serverless que permite coordenar múltiplos serviços AWS em workflows visuais. É uma solução low-code onde você desenha o fluxo de trabalho visualmente, sem precisar escrever código complexo de integração.

### Como Funciona?
Você define uma State Machine (Máquina de Estados) que é basicamente um diagrama de fluxo onde:

- Cada caixa representa uma ação (invocar Lambda, enviar SNS, consultar DynamoDB, etc.)
- As setas representam a ordem de execução
- Você adiciona lógica de decisão, loops e tratamento de erros visualmente

### Principais Tipos de Estados

| Estado | Função | Exemplo de Uso |
|--------|--------|----------------|
| **Task** | Executa uma tarefa específica | Invocar um Lambda, chamar uma API |
| **Choice** | Decisão condicional (if/else) | Verificar se valor > 100 |
| **Parallel** | Executa múltiplas tarefas simultaneamente | Processar vários arquivos ao mesmo tempo |
| **Wait** | Pausa a execução por um tempo | Aguardar 5 minutos antes de continuar |
| **Pass** | Passa dados sem processamento | Transformar formato dos dados |
| **Succeed/Fail** | Finaliza o workflow | Marcar conclusão ou erro |
| **Map** | Itera sobre uma lista | Processar cada item de um array |

### Benefícios das Step Functions

 1. Visual e Low-Code
- Interface gráfica para desenhar workflows
- Não precisa código para conectar serviços
- Facilita entendimento por toda equipe (devs e não-devs)

 2. Gerenciamento de Erros Automático
- Retry: Tenta novamente automaticamente em caso de falha
- Catch: Captura erros e direciona para fluxo alternativo
- Histórico completo de todas as execuções

3. Escalabilidade e Confiabilidade
- Escala automaticamente sem configuração
- AWS gerencia toda infraestrutura
- Garantia de execução (não perde estados)

4. Custo-Benefício
- Paga apenas pelas transições de estado executadas
- Sem servidor rodando 24/7
- Reduz drasticamente código "glue" entre serviços

 5. Integração Nativa
= Mais de 200 serviços AWS integrados
= Lambda, DynamoDB, SNS, SQS, ECS, API Gateway, etc.
= Chamadas diretas sem código intermediário

------------------------------------------------------------------------------------------------------------------------------
### Exemplo Prático: Sistema de Processamento de Pedidos
Pensei num workflow que simula o fluxo completo de processamento de um pedido em um e-commerce:

Descrição do Fluxo

1. INÍCIO (Input com dados do pedido)
    ↓
2. ValidarPedido (Lambda Task)
   • Verifica se os dados do pedido estão corretos
   • Valida: ID, valor, cliente, itens
    ↓
3. PedidoValido? (Choice State)
   • Se VÁLIDO → continua o fluxo
   • Se INVÁLIDO → pula para notificação de erro
    ↓
4. ProcessarPagamento (Lambda Task com Retry)
   • Tenta processar o pagamento
   • Configurado com 3 tentativas automáticas
   • Se falhar após 3 tentativas → Catch captura o erro
    ↓
5. NotificarSucesso OU NotificarErro (Lambda Task)
   • Envia notificação conforme resultado
   • Pode ser email, SMS, webhook, etc.
    ↓
6. FIM (Workflow completo)
   
Componentes:
3 Lambda Functions: ValidarPedido, ProcessarPagamento, EnviarNotificacao
1 Choice State: Decisão condicional sobre validade do pedido
1 Retry Policy: 3 tentativas com backoff exponencial
1 Catch Block: Tratamento de erro no pagamento
------------------------------------------------------------------------------------------------------------------------------

## Por que usar Step Functions?
 ### Sem Step Functions:
Lambda1 → código para chamar Lambda2
       → código para retry
       → código para tratar erro
       → código para chamar Lambda3
Você precisa escrever toda lógica de orquestração dentro dos Lambdas.

### Com Step Functions:
Lambda1 → Lambda2 → Lambda3
A orquestração é visual e declarativa. Os Lambdas fazem apenas sua função específica.



## Casos de Uso Reais
Step Functions são ideais para:

- Processamento de pedidos/pagamentos (como meu exemplo)
- ETL e pipelines de dados (extrair, transformar, carregar)
- Workflows de aprovação (documentos, despesas, etc)
- Processamento de mídia (converter vídeos, gerar thumbnails)
- Machine Learning pipelines (treinar, validar, deploy)
- Orquestração de microserviços (coordenar múltiplas APIs)


**Feito por**
**Pietra Almeida**

### Contatos
<div> 
    <a href = "mailto:costapietra@gmail.com"><img loading="lazy" src="https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white" target="_blank"></a>
    <a href="https://www.linkedin.com/in/almeidapietra" target="_blank"><img loading="lazy" src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a>   
</div>
