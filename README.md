# PlayStore Medallion Pipeline

## 📌 Sobre o Projeto

Este é um projeto estritamente **educacional** desenvolvido com o objetivo de consolidar conceitos fundamentais de Engenharia de Dados, simulando um cenário real de ingestão, governança e transformação de dados em larga escala.

O pipeline realiza o processo completo de **ETL (Extract, Transform, Load)** capturando avaliações de usuários na Google Play Store para transformá-las em *insights* analíticos de negócio (análise de sentimento e categorização de demandas).

## 🚀 Metodologia de Desenvolvimento e Evolução

O projeto foi construído e refinado em duas fases estratégicas para demonstrar a maturidade na modelagem e no ciclo de vida do desenvolvimento de dados:

1. **Fase 1: Script Conceitual e Perfilamento (Análise de Viabilidade)**
   - Desenvolvimento de um script inicial focado em entender a estrutura de resposta da API, tipos de dados originais (schema) e volumetria.
   - **Resultado:** Identificou-se que a análise de sentimento baseada apenas na nota numérica (1 a 5) era imprecisa para determinar a real dor do usuário, surgindo a necessidade de uma camada de processamento de linguagem natural.

2. **Fase 2: Script com Implementação Cognitiva e Otimização de LLM**
   - Refatoração completa do pipeline para integrar Inteligência Artificial Generativa (**Gemini 2.5 Flash**) no processo de enriquecimento de dados da camada Silver.
   - **Otimização por Lote (Batching):** Para evitar erros de limite de requisição (*Rate Limits* / Erro 429) comuns no processamento linha por linha, a arquitetura foi desenhada para agrupar múltiplos registros e enviá-los em uma única chamada unificada para a IA.
   - **Garantia de Esquema:** Utilização da biblioteca **Pydantic** para forçar o modelo de IA a retornar dados estritamente estruturados (JSON), garantindo a estabilidade do contrato de dados.

## 🏗️ Arquitetura do Data Lake e Boas Práticas

A estrutura do pipeline adota os padrões arquiteturais utilizados por grandes empresas de tecnologia, destacando-se:

- **Arquitetura Medalhão (Medallion Architecture):** Organização do fluxo de dados em três camadas de maturidade bem definidas no diretório `data_lake/`:
  - **Bronze (Raw):** Preservação do dado bruto em formato semiestruturado (JSON) vindo da API, garantindo a reprodutibilidade do pipeline sem reonerar a fonte original.
  - **Silver (Cleaned):** Limpeza de dados nulos, aplicação de regras de governança para conformidade com a **LGPD** (anonimização de dados sensíveis e PII) e enriquecimento inteligente via IA (extração de sentimento real e categoria do feedback).
  - **Gold (Business):** Agregações analíticas diárias prontas para o consumo de equipes de Business Intelligence (BI) e dashboards.
- **Armazenamento Otimizado (Parquet):** Utilização do formato colunar `.parquet` nas camadas Silver e Gold, visando alta compactação de disco e performance de leitura acelerada.
- **Idempotência:** O pipeline pode ser executado múltiplas vezes no mesmo dia sem gerar duplicidade de dados, pois realiza o cruzamento com o histórico armazenado e remove registros repetidos com base no identificador único `reviewId`.
- **Tratamento de Erros e Resiliência:** Código modularizado em funções isoladas com captura de exceções (`try/except`) e monitoramento profissional via biblioteca nativa de `logging`.

## 🛠️ Tecnologias Utilizadas

- **Linguagem Principal:** Python 3.10+
- **Ambiente:** Google Colab
- **Manipulação e Engenharia de Dados:** Pandas
- **Ingestão:** Google Play Scraper (API)
- **Formatos de Arquivos:** JSON (Bronze) e Apache Parquet (Silver/Gold)
- **Inteligência Artificial:** SDK Google GenAI (Gemini 2.5 Flash)
- **Validação de Contratos de Dados:** Pydantic (Structured Outputs)

## 📊 Estrutura de Armazenamento Temporário (Colab)

```text
📁 data_lake
├── 📁 bronze
│   └── 📄 reviews_raw_YYYY-MM-DD.json
├── 📁 silver
│   └── 📄 reviews_cleaned.parquet
└── 📁 gold
    └── 📄 daily_ai_analysis_summary.parquet
```
