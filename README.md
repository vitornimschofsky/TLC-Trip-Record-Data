# Case Técnico: Data Architecture - iFood

## 1. Introdução

Este repositório contém a solução técnica para o case de Data Architect da iFood. O objetivo é demonstrar habilidades em Engenharia de Dados, desenvolvendo um pipeline para ingestão, transformação e análise de dados de corridas de táxi de Nova York.

A solução foi construída utilizando uma arquitetura de Data Lake com o padrão Medallion, garantindo que os dados sejam organizados, limpos e otimizados para consumo.

## 2. Arquitetura da Solução

![Diagrama da Arquitetura](images/diagrama_arquitetura.jpeg)

A arquitetura da solução segue um modelo de pipeline de dados moderno, com três camadas principais:

* **Ingestão Serverless (AWS Lambda):** Uma função Lambda automatiza a ingestão dos dados de origem, baixando os arquivos Parquet e salvando-os em um bucket S3.
* **Processamento e Modelagem (Databricks):** Três notebooks PySpark são executados no Databricks para processar e refinar os dados nas camadas Bronze, Silver e Gold.
* **Armazenamento (Amazon S3):** O Amazon S3 é utilizado como o Data Lake para armazenar os dados em todas as três camadas, aproveitando sua escalabilidade e baixo custo.

### Visão Geral do Pipeline

`AWS Lambda (Ingestão) -> S3 (Camada Bronze) -> Databricks (Processamento) -> S3 (Camadas Silver & Gold)`

## 3. Justificativa das Escolhas Técnicas

As seguintes escolhas técnicas foram feitas para atender aos critérios de avaliação do case, como Qualidade do Código, Criatividade e Justificativa:

* **AWS Free Tier (S3 & Lambda):** Utilizado para demonstrar a criação de uma solução de ponta a ponta na nuvem de forma gratuita e escalável.
* **Databricks Community Edition:** Escolhido como a plataforma de processamento distribuído (usando PySpark) por sua facilidade de uso com o formato Delta Lake e suporte a SQL.
* **Arquitetura Medallion:** Implementada para garantir que o pipeline seja organizado, manutenível e que o dado tenha uma clara rastreabilidade e governança.
* **Formato de Dados `Delta Lake`:** Utilizado nas camadas Silver e Gold por oferecer as propriedades ACID e otimizações de performance, o que é ideal para tabelas de BI.
* **Modelagem Fato e Dimensão:** A camada Gold foi modelada em um `star schema` simplificado para otimizar as consultas analíticas e facilitar o consumo dos dados por ferramentas de BI.

## 4. Estrutura do Repositório

O repositório está organizado conforme solicitado no case técnico:

* `ifood-case/`
    * `src/`: Contém o código da função Lambda (`lambda_function.py`) e o script para empacotar as dependências (`create_package.py`).
    * `analysis/`: Contém os notebooks do Databricks (`01-Bronze`, `02-Silver`, `03-Gold`).
    * `README.md`: Este arquivo, com a documentação do projeto.
    * `requirements.txt`: Lista das dependências Python (`boto3`, `requests`).

## 5. Instruções de Execução

Para executar o pipeline, siga os passos abaixo:

1.  **Configuração na AWS:**
    * Crie um bucket S3 (ex: `ifood-case-data-lake-vitor`).
    * Crie um IAM Role para o Lambda com permissão de escrita no S3.
    * Substitua o `s3_bucket_name` no código do Lambda pelo nome do seu bucket.
    * Crie um pacote ZIP com o código e as dependências e faça o upload para a sua função Lambda.
2.  **Execução do Pipeline:**
    * No Databricks, crie os notebooks e cole o código correspondente em cada um.
    * Substitua as credenciais (`YOUR_ACCESS_KEY_ID`, `YOUR_SECRET_ACCESS_KEY`) e o nome do bucket nos notebooks.
    * Execute o notebook **`01-Ingestao_Bronze_Layer`**. Ele irá ler os dados brutos e salvá-los no S3.
    * Execute o notebook **`02-Processamento_Silver_Layer`**. Ele irá limpar e padronizar os dados.
    * Execute o notebook **`03-Processamento_Gold_Layer`**. Ele irá criar as tabelas de fato e dimensão e rodar as análises.

## 6. Resultados da Análise

As seguintes consultas foram executadas na camada Gold para responder às perguntas do case técnico.

### Média de valor total por mês para yellow táxis:
A média de valor total recebido por mês para táxis amarelos é:

| year | month | media_valor_total_mensal |
|:-----|:------|:-------------------------|
| 2023 | 1     |             26.91        |
| 2023 | 2     |             26.79        |
| 2023 | 3     |             27.69        |
| 2023 | 4     |             28.17        |
| 2023 | 5     |             28.86        |


### Média de passageiros por hora em maio para todos os táxis:
A média de passageiros por hora em maio, considerando todos os táxis, é:

| hora_do_dia | media_passageiros |
|:------------|:------------------|
|      0      |             1.36  |
|      1      |             1.37  | 
|      2      |             1.38  |
|      3      |             1.36  |
|      4      |             1.25  |
|      5      |             1.17  |
|      6      |             1.17  |
|      7      |             1.19  |
|      8      |              1.2  |
|      9      |             1.24  |
|     10      |             1.28  |
|     11      |              1.3  |
|     12      |             1.31  |
|     13      |             1.32  |
|     14      |             1.32  |
|     15      |             1.34  |
|     16      |             1.33  |
|     17      |             1.33  |
|     18      |             1.32  |
|     19      |             1.34  |
|     20      |             1.35  |
|     21      |             1.37  |
|     22      |             1.37  |
|     23      |             1.36  |

