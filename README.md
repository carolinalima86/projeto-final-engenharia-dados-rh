# 📊 Projeto Final — Trilha Engenharia de Dados | Data Girls

Este projeto foi desenvolvido como parte da trilha de **Engenharia de Dados** do programa **Data Girls**, com foco em aplicar os conhecimentos de **ETL**, **tratamento de dados**, **armazenamento em nuvem (AWS S3)** e **conceitos de orquestração de pipeline com simulação de agendamento**.

## 🎯 Objetivo

Analisar dados de **recursos humanos** (employee attrition) e criar um pipeline de dados completo que:

- Limpa e transforma os dados
- Salva os dados processados
- Realiza upload para o **AWS S3**
- **Simula a execução agendada do processo, demonstrando a automação de pipeline.**
## 🗂️ Estrutura do Projeto
├── ProjetofinalTrilhaEngenhariaDados_DataGirls.ipynb  # Notebook principal do projeto

├── data/

│   └── WA_Fn-UseC_-HR-Employee-Attrition.csv          # Dataset original

└── README.md

## 🧰 Tecnologias e Ferramentas

- **Python**
- **Pandas** e **NumPy**
- **Google Colab**
- **AWS S3** com `boto3`
- **Ferramentas Conceituais de Orquestração (Airflow)**
## 🔄 Pipeline ETL

Este pipeline segue as etapas de Extração, Transformação e Carga (ETL) para preparar os dados de RH.

### 1. **Extração**
- **Fonte:** O dataset original foi importado de um arquivo CSV, `WA_Fn-UseC_-HR-Employee-Attrition.csv`, que está armazenado localmente no repositório na pasta `data/`.

### 2. **Transformação**
Esta etapa foca na limpeza, padronização e enriquecimento dos dados para torná-los prontos para análise. As principais transformações incluem:

* **Renomeação de Colunas:** Nomes das colunas traduzidos para português para melhor legibilidade e consistência (ex: 'Age' para 'idade', 'Department' para 'departamento').
* **Tradução de Valores Categóricos:** Valores em colunas como 'Attrition', 'Gender', 'MaritalStatus', 'BusinessTravel', 'EducationField', 'OverTime' e 'JobRole' foram traduzidos para português (ex: 'Yes' para 'Sim', 'Male' para 'Masculino').
* **Normalização de Escalas:** Colunas como 'Education', 'EnvironmentSatisfaction', 'JobInvolvement', 'JobSatisfaction', 'PerformanceRating', 'RelationshipSatisfaction' e 'WorkLifeBalance' tiveram seus valores numéricos mapeados para descrições textuais correspondentes (ex: 1: 'Baixa', 4: 'Excelente').
* **Criação de Novas Features:**
    * `faixa_etaria`: Criada a partir da coluna 'idade', categorizando os funcionários em faixas etárias específicas (ex: '18-25', '26-35').
    * `desligamento_flag`: Coluna binária (0 ou 1) que indica se o funcionário se desligou (1 para 'Sim', 0 para 'Não').
    * `faixa_salarial`: Criada através da categorização da 'renda_mensal' em quartis (Q1_Baixa, Q2_Media, Q3_Alta, Q4_Muito_Alta).
* **One-Hot Encoding:** Variáveis categóricas restantes foram convertidas em representações numéricas binárias, utilizando `pd.get_dummies()`, para serem compatíveis com modelos de Machine Learning e análises quantitativas.

### 3. **Carga**
- O dataset transformado é salvo em formato **Parquet** por sua eficiência em armazenamento e leitura, ideal para uso em Data Lakes.
- O arquivo Parquet é então carregado para um bucket no **AWS S3**. Para simular o agendamento, o nome do arquivo no S3 inclui a data da execução, permitindo o versionamento dos dados (`funcionarios_transformados_YYYYMMDD.parquet`).

### 4. **Automação e Simulação de Agendamento**
Para cumprir o requisito de "execução agendada e simulada", o pipeline ETL foi encapsulado em uma função Python única (`executar_pipeline_etl`). A automação foi simulada da seguinte forma no Google Colab:

* A função `executar_pipeline_etl` é chamada repetidamente dentro de um loop.
* Cada chamada simula uma execução em um "dia" diferente, passando a data simulada como parâmetro.
* Um pequeno atraso (`time.sleep()`) foi adicionado entre as execuções para visualização, simulando o intervalo de agendamento.
* Como resultado, cada execução gera um novo arquivo Parquet no S3 com a data da simulação no nome (ex: `s3://datagirlsetl1008/projeto_rh/funcionarios_transformados_20250724.parquet`), demonstrando o versionamento e a capacidade de reexecução agendada.

Em um ambiente de produção real, esta orquestração seria gerenciada por ferramentas dedicadas como o **Apache Airflow**, que permitiriam agendamento complexo, monitoramento robusto e recuperação de falhas.

## 📁 Dataset

O dataset utilizado é de domínio público e contém informações como:

- Nível educacional
- Cargo
- Salário
- Satisfação no trabalho
- Equilíbrio entre vida pessoal e profissional
- Status de desligamento

## ☁️ AWS S3

Bucket criado: `datagirlsetl1008`
Arquivos enviados: `projeto_rh/funcionarios_transformados_YYYYMMDD.parquet` (múltiplos arquivos, um para cada execução simulada).
> A conexão foi feita utilizando `boto3` com credenciais IAM específicas para acesso programático.

## 🚀 Como Rodar o Projeto

1.  Clone este repositório.
2.  Abra e execute o notebook `ProjetofinalTrilhaEngenhariaDados_DataGirls.ipynb` no Google Colab.
3.  Insira suas credenciais AWS quando solicitado.
4.  Execute a célula que define a função `executar_pipeline_etl`.
5.  Execute a célula que contém o loop de simulação de agendamento.
6.  Verifique os arquivos Parquet gerados em seu bucket S3 (eles terão a data no nome, como `funcionarios_transformados_20250724.parquet`).
## 👩‍💻 Autora

**Carolina Cavalcante Lima**
Economista em transição de carreira para TI, com foco em **Engenharia de Dados** e **Cloud Computing**.

