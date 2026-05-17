# PlayStore Medallion Pipeline

## 📌 Sobre o Projeto

Este é um projeto estritamente educacional desenvolvido com o objetivo de consolidar conceitos fundamentais de Engenharia de Dados, simulando um cenário real de ingestão, governança e transformação de dados em larga escala.

O pipeline realiza o processo completo de ETL (Extract, Transform, Load) capturando avaliações de usuários na Google Play Store para transformá-las em insights analíticos de negócio (análise de sentimento e engajamento).

## 🏗️ Arquitetura e Boas Práticas Aplicadas

A principal meta deste projeto foi aplicar os padrões arquiteturais utilizados por grandes empresas de tecnologia, destacando-se:

- Arquitetura Medalhão (Medallion Architecture): Organização do fluxo de dados em três camadas de maturidade bem definidas:
  - Bronze (Raw): Preservação do dado bruto em formato semiestruturado (JSON) vindo da API, garantindo a reprodutibilidade do pipeline.
  - Silver (Cleaned): Limpeza de dados nulos, aplicação de regras de governança (anonimização de dados pessoais para conformidade com a LGPD) e enriquecimento de dados (feature engineering).
  - Gold (Business): Agregações analíticas prontas para o consumo de equipes de Business Intelligence (BI).
- Armazenamento Otimizado (Parquet): Utilização do formato colunar .parquet nas camadas Silver e Gold, visando alta compactação de disco e performance de leitura.

## 🛠️ Tecnologias Utilizadas

- Linguagem: Python
- Ambiente: Google Colab
- Manipulação de Dados: Pandas
- Extração: Google Play Scraper (API)
- Formato de Arquivos: JSON e Apache Parquet
