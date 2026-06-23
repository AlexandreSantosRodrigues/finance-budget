<h1 align="center"> 
  📊 Corporate Finance: Budget vs Actual Analysis & 2024 Forecast 
</h1>

<p align="center">
  <a href="https://www.python.org/">
    <img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" alt="Python">
  </a>
  <a href="https://pandas.pydata.org/">
    <img src="https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white" alt="Pandas">
  </a>
  <a href="https://numpy.org/">
    <img src="https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white" alt="NumPy">
  </a>
  <a href="https://matplotlib.org/">
    <img src="https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=python&logoColor=white" alt="Matplotlib">
  </a>
  <a href="https://kaggle.com/">
    <img src="https://img.shields.io/badge/Kaggle-20BEFF?style=for-the-badge&logo=Kaggle&logoColor=white" alt="Kaggle">
  </a>
</p>

> **Resumo Executivo:** Uma *pipeline* analítica de ponta a ponta (*End-to-End*) desenhada para resolver inconsistências de dados financeiros (Data Quality), modelar novos atributos de negócio (Feature Engineering) e projetar o comportamento orçamental corporativo do próximo ano fiscal com recurso a algoritmos preditivos.

---

## 📑 Índice
1. [Visão Geral do Projeto](#-visão-geral-do-projeto)
2. [Arquitetura e Pipeline de Dados](#-arquitetura-e-pipeline-de-dados-etl--eda)
3. [Dicionário de Dados](#-dicionário-de-dados)
4. [Estrutura do Repositório](#-estrutura-do-repositório)
5. [Como Executar Localmente](#-como-executar-localmente)
6. [Próximos Passos (Roadmap)](#-próximos-passos-roadmap)

---

## 📌 Visão Geral do Projeto
Este projeto processa um conjunto de dados sintético de **10.010 registos transacionais financeiros** que documentam despesas departamentais ocorridas entre Janeiro de 2021 e Dezembro de 2023. O objetivo central é fornecer à equipa de controlo de gestão ferramentas estatísticas e visuais para avaliar desvios de orçamento (*Budget Variances*) e otimizar o planeamento financeiro.

---

## ⚙️ Arquitetura e Pipeline de Dados (ETL & EDA)

A solução foi estruturada em 4 módulos sequenciais e escaláveis:

### 🛠️ 1. Data Wrangling & Quality Assurance
Tratamento rigoroso do *dataset* bruto para garantir a integridade dos cálculos:
- **Deduplicação Determinística:** Identificação e eliminação de 10 registos integralmente duplicados (`df.drop_duplicates`).
- **Imputação de Valores Ausentes (*Missing Values*):**
  - **Dados Numéricos:** Imputação via **Mediana** robusta (mitigação do peso de *outliers*).
  - **Dados Categóricos:** Imputação via **Moda** estatística.
  - **Séries Temporais (`Date`):** Aplicação de algoritmo *Forward Fill* (`ffill`) para respeitar a continuidade cronológica.
  - **Chaves Primárias (`Transaction ID`):** Rotulagem categórica de nulidades como `ID_Nao_Informado`.

### 🧠 2. Feature Engineering
Geração de novas dimensões de análise essenciais para os relatórios executivos:
- **Decomposição *Datetime*:** Extração paramétrica de `Year` e `Month_Name`.
- **Cálculo de Variância:** Criação da métrica absoluta (`Budget` - `Actual`).
- **Cálculo de Variância Relativa:** Rácio percentual de desvio orçamental, com tratamento vetorial via `np.where` para contornar exceções matemáticas (divisão por zero).

### 📈 3. Exploratory Data Analysis (EDA)
Criação de visualizações de elevado impacto baseadas na gramática de gráficos do `Seaborn`:
- Agregação macro (`Bar Plots`) de Orçamento vs. Realizado por departamento.
- Análise de composição (`Donut Charts`) das modalidades de pagamento.
- Identificação de gargalos (Top 3 Departamentos) através de medidas de diferença absoluta.
- Análise de tendências cronológicas via `Line Plots`.

### 🤖 4. Modelagem Preditiva (Forecast 2024)
- **Método:** Regressão Linear Simples via Mínimos Quadrados Ordinários (OLS approximation usando `np.polyfit`).
- **Execução:** O algoritmo itera sobre cada departamento de forma isolada, extrai os coeficientes de inclinação e interceção da série temporal (2021-2023), e projeta cenários de gastos ($) estritamente não-negativos para 2024.
- **Output:** Transformação dos dados (*Melt*) para comparar, num gráfico otimizado, a *baseline* real de 2023 contra a projeção do modelo para 2024.

---

## 📖 Dicionário de Dados

| Coluna | Tipo de Dado (Pandas) | Descrição do Atributo |
| :--- | :--- | :--- |
| `Date` | `datetime64[ns]` | Data exata em que a despesa foi processada. |
| `Department` | `object` | Unidade de negócio (e.g., Sales, IT, HR). |
| `Category` | `object` | Classificação da despesa (e.g., Salaries, Infrastructure). |
| `Region` | `object` | Zona geográfica da operação. |
| `Budget Amount` | `float64` | Montante orçamentado (planeamento financeiro inicial). |
| `Actual Amount` | `float64` | Montante financeiramente executado (gasto real). |
| `Variance` | `float64` | Diferença absoluta calculada (*Feature gerada*). |
| `Variance_Pct` | `float64` | Desvio percentual relativo (*Feature gerada*). |

---

## 📂 Estrutura do Repositório

```text
📦 finance-budget-analysis
 ┣ 📂 data
 ┃ ┗ 📜 Budget_vs_Actual_Data.xlsx    # Dataset bruto (input)
 ┣ 📂 notebooks
 ┃ ┗ 📓 finance_eda_forecast.ipynb    # Código principal (Jupyter Notebook)
 ┣ 📂 outputs
 ┃ ┗ 📜 forecast_2024.csv             # Exportação das previsões
 ┗ 📜 README.md                       # Documentação técnica do projeto
