# Opcional
## Integração de Script em Python com BigQuery
Um script em Python pode ser usado para extrair dados de transações e enviá-los ao BigQuery utilizando a biblioteca google-cloud-bigquery. Aqui está um exemplo básico:
```python
from google.cloud import bigquery

# Autenticação com chave de serviço (arquivo JSON)
client = bigquery.Client.from_service_account_json("service-account-key.json")

# Dados de exemplo a serem enviados para o BigQuery
data = [
    {"transaction_id": "1", "user_id": "101", "amount": 150.00, "type": "deposit"},
    {"transaction_id": "2", "user_id": "102", "amount": 200.00, "type": "payment"}
]

# Configurando o dataset e tabela no BigQuery
dataset_id = "seu_dataset"
table_id = "transacoes"

# Criar referência para a tabela
table_ref = client.dataset(dataset_id).table(table_id)
table = client.get_table(table_ref)

# Inserir os dados
errors = client.insert_rows_json(table, data)

if errors:
    print(f"Erros ao inserir: {errors}")
else:
    print("Dados enviados com sucesso!")

```
- Pré-requisitos:
    - Criar um dataset e uma tabela no BigQuery.
    - Configurar uma conta de serviço e obter o arquivo JSON para autenticação.

## Dashboard para Visualização de Dados
### **Aqui está como criar com Metabase:**
1. Conectar ao BigQuery:
    - No Metabase, configure uma nova fonte de dados com as credenciais do BigQuery.
2. Criar Dashboards:
    - Exemplos de visualizações:
        - Gráfico de barras: Volume de transações por tipo (depósitos, pagamentos).
        - Gráfico de linha: Saldo total dos usuários ao longo do tempo.
        - Tabela: Lista de transações recentes com detalhes.
3. Publicar o Dashboard:
    - Compartilhe o link do dashboard com a equipe para monitoramento em tempo real.

### **Aqui está como criar com Power BI:**

**Conecte ao BigQuery**
1. No Power BI Desktop:
    - Vá em Home > Get Data > Google BigQuery.
    - Faça login com suas credenciais do Google Cloud.
    - Navegue até o dataset e a tabela de transações no BigQuery.
    - Carregue os dados ou use o modo DirectQuery para consultas em tempo real.

**Conecte via Arquivo CSV (opção alternativa para simplificação):**
1. Exportar os dados do BigQuery:
    - No console do BigQuery, execute uma consulta para exportar dados de exemplo.
        ```sql
        SELECT transaction_id, user_id, amount, type, transaction_date
        FROM seu_dataset.transacoes
        ```
    - Baixe o resultado como CSV.
2. Importar no Power BI:
    - Clique em Home > Get Data > Text/CSV.
    - Carregue o arquivo exportado.

**Criação do Dashboard**
1. Transformar os Dados (Editor Power Query):
    - Certifique-se de que colunas como `transaction_date` estejam no formato de data/hora.
    - Renomeie as colunas para facilitar a leitura (ex.: `transaction_id` → "ID Transação").
2. Criação de Gráficos:
    - Volume de Transações por Tipo:
        - Insira um gráfico de barras.
        - Eixo X: type (tipo de transação: depósito ou pagamento).
        - Eixo Y: Soma de amount (valor total).
    - Saldo por Usuário:
        - Insira uma tabela.
        - Colunas: user_id (ID do usuário) e soma de amount.
    - Transações ao Longo do Tempo:
        - Insira um gráfico de linha.
        - Eixo X: transaction_date (data da transação).
        - Eixo Y: Soma de amount.
3. Filtros:
    - Adicione um filtro para permitir a seleção de um único usuário ou intervalo de datas.

**Publicação**
1. Publique o dashboard no Power BI Service:
    - lique em File > Publish > Publish to Power BI Service.
    - Escolha o workspace desejado.
2. Compartilhamento:
    - No Power BI Service, configure permissões para outros usuários acessarem o dashboard.
    - Gere um link para compartilhamento ou adicione a um workspace compartilhado.
3. Exemplo de Dashboard
    - O dashboard final incluiria:
        - Um gráfico de barras mostrando o volume de transações por tipo (depósitos e pagamentos).
        - Uma tabela com o saldo total de cada usuário.
        - Um gráfico de linha mostrando a evolução do volume transacional ao longo do tempo.
        - Filtros interativos para usuários e datas.
## Pipeline Básica de CI
Uma pipeline básica de CI pode ser configurada com GitHub Actions para testes e builds. Aqui está um exemplo de configuração no arquivo .github/workflows/ci.yml:
```yaml
name: CI Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout código
      uses: actions/checkout@v3

    - name: Configurar Node.js
      uses: actions/setup-node@v3
      with:
        node-version: 18

    - name: Instalar dependências
      run: npm install

    - name: Executar testes
      run: npm run test

    - name: Build do projeto
      run: npm run build

  deploy:
    runs-on: ubuntu-latest
    needs: build

    steps:
    - name: Deploy em ambiente de staging
      run: echo "Deploy para staging realizado com sucesso!"
```
- Explicação:
    - A pipeline é acionada em push ou pull requests na branch main.
    - Passos incluem checkout do código, instalação de dependências, execução de testes, e build do projeto.
    - Um deploy fictício é simulado para fins de demonstração.

## Organização com Kanban
Para organizar o trabalho, um board no **Jira** ou **Trello** pode ser usado com as seguintes colunas:

1. **Backlog:** Tarefas futuras, como implementar novos endpoints ou otimizar integrações.
2. **In Refine:** Tarefas que estão sendo refinados pela equipe para seguir para a etapa de desenvolvimento.
3. **Waiting Dev:** Itens a serem feitos que já podem ser desenvolvidos pe.
4. **In Progress:** Tarefas em desenvolvimento.
5. **In PullRequest:** Tarefas que já foram desenvolvidas e que precisam ser avaliadas por pelo menos 2 membros da equipe.
6. **In CodeReveiw:** Tarefas que foram avaliadas no PullRequest e que necessitam de ajustes.
7. **Waiting QA:** Tarefas que já tiveram as aprovações na etapa de PullRequest e que já podem ser avaliadas e testadas pelos QAs da equipe.
8. **In QA:** Tarefas que estão sendo avaliadas e testadas pelos QAs.
9. **Waiting PROD:** Tarefas que já foram avaliadas e testadas e já estão prontas para subir em produção.
10. **Pilote:** Tarefas que já estão em produção em seus momentos iniciais e que estão sendo feito os acompanhamentos para validação do funcionamento correto em produção.
11. **Done:** Tarefas concluídas que já passaram por todos os passos anteriores.

Também é comum definir limites mínimos e máximos de tarefas em cada etapa(coluna) de acordo como o tamanho da equipe para garantir a fluidez do projeto, além de definir para cada tarefa qual o responsável, data de inicio, data prevista de encerramento, prioridade e pontos(pontos que defini a complexidade da tarefa. Ex. fibonacci)