🇧🇷

# Pipeline de Tratamento de Dados Geoquímicos

Este repositório contém um notebook desenvolvido para automatizar o pré-processamento de dados geoquímicos. O script foi projetado para operar no ambiente Google Colab, garantindo a padronização das planilhas e o tratamento estatístico adequado dos Limites de Detecção (LOD).

## 🚀 Objetivo

O foco principal do notebook é limpar, transformar e imputar valores numéricos em dados brutos que apresentam o marcador de censura (o símbolo `<`). A metodologia estatística assegura que as leituras não sejam distorcidas durante análises exploratórias e detecção de anomalias subsequentes.

## 🛠️ Tecnologias e Bibliotecas

O pipeline utiliza as seguintes dependências em Python:
*   **Pandas:** Leitura, manipulação e exportação dos DataFrames.
*   **NumPy:** Suporte às operações numéricas.
*   **Re (Regex):** Extração dinâmica de valores numéricos formatados como texto, agora com suporte avançado para detecção híbrida (com ou sem números, identificando automaticamente separadores de vírgula ou ponto).
*   **Google Colab (Drive):** Montagem do sistema de arquivos para acesso direto aos dados em nuvem.

## ⚙️ Funcionalidades e Regras de Negócio

O notebook está estruturado em funções modulares que aplicam as seguintes regras de negócio:

1.  **Limpeza de Cabeçalhos:** Remoção de espaços em branco (whitespaces) invisíveis nos nomes das colunas, evitando erros de indexação.
2.  **Avaliação Estatística de Censura:** Mapeamento da proporção de dados censurados (abaixo do LOD) por elemento/coluna.
3.  **Descarte Condicional:** Colunas (elementos) que apresentem uma proporção de valores abaixo do LOD **superior a 30%** são automaticamente descartadas do conjunto final.
4.  **Imputação Dinâmica Híbrida (LOD):**
    O script avalia o conteúdo que acompanha a marcação de censura para definir a ação correta:
    *   **Quando há valor numérico (ex: `<0,5`, `<20.0`):** O script extrai o número utilizando Regex.
        *   Para colunas com **menos de 10%** de censura: Valores censurados são substituídos por **66%** do valor numérico do LOD.
        *   Para colunas com censura **entre 10% e 30%**: Valores censurados são substituídos por **33%** do valor numérico do LOD.
    *   **Quando NÃO há valor numérico (ex: `<LD`, `<LOD`):** A conversão para frações do LOD é ignorada. Os valores censurados são transformados de forma segura em dados ausentes (`NaN`), permitindo que a coluna continue operante em cálculos matemáticos.
5.  **Conversão de Tipos:** Garantia de que todo o conjunto resultante possua formato puramente numérico (float).
6.  **Log de Auditoria:** Geração de uma aba secundária no Excel resultante, documentando todos os elementos químicos descartados pelas regras de censura.

## 💻 Como Utilizar no Google Colab

1.  Faça o upload deste Notebook (`.ipynb`) para o seu Google Drive e abra-o via Google Colab.
2.  Execute a célula de importação. O Colab solicitará permissão para montar o seu Google Drive. Conceda a autorização.
3.  Certifique-se de que os seus arquivos de entrada (em formato `.xlsx`) estejam disponíveis no seu Google Drive.
4.  Desça até o final do notebook e ajuste o caminho do arquivo na função de execução.
    *   Exemplo: `executar_pipeline_geoquimico("/content/drive/MyDrive/Seu_Caminho/Seu_Arquivo.xlsx")`
5.  Execute todas as células. O arquivo resultante (ex: `resultado_geoquimico_tratado.xlsx`) será gerado no ambiente do Colab contendo as abas de dados tratados e o log de remoção.

## 🗂️ Estrutura de Arquivos Resultantes

O pipeline consolida os resultados em um novo arquivo Excel `.xlsx` com as seguintes abas:
*   `Dados_Tratados`: Contém a matriz de dados limpa, imputada e convertida.
*   `Log_Remocao`: Tabela de auditoria listando as colunas eliminadas pelo critério de 30% de censura.


---

🇺🇸

# Geochemical Data Processing Pipeline

This repository contains a notebook developed to automate the preprocessing of geochemical data. The script is designed to operate in the Google Colab environment, ensuring worksheet standardization and the appropriate statistical treatment of Limits of Detection (LOD).

## 🚀 Objective

The main focus of the notebook is to clean, transform, and impute numerical values into raw data featuring the censorship marker (the `<` symbol). The statistical methodology ensures that readings are not distorted during subsequent exploratory analysis and anomaly detection.

## 🛠️ Technologies and Libraries

The pipeline utilizes the following dependencies in Python:
*   **Pandas:** Reading, manipulation, and exporting of DataFrames.
*   **NumPy:** Support for numerical operations.
*   **Re (Regex):** Dynamic extraction of text-formatted numerical values, now featuring advanced support for hybrid detection (with or without numbers, automatically identifying comma or dot separators).
*   **Google Colab (Drive):** File system mounting for direct access to cloud data.

## ⚙️ Features and Business Rules

The notebook is structured into modular functions that apply the following business rules:

1.  **Header Cleaning:** Removal of invisible whitespaces in column names, preventing indexing errors.
2.  **Statistical Censorship Evaluation:** Mapping the proportion of censored data (below LOD) per element/column.
3.  **Conditional Discard:** Columns (elements) presenting a proportion of values below the LOD **greater than 30%** are automatically discarded from the final dataset.
4.  **Hybrid Dynamic Imputation (LOD):**
    The script evaluates the content accompanying the censorship marker to determine the correct action:
    *   **When a numerical value is present (e.g., `<0.5`, `<20.0`):** The script extracts the number using Regex.
        *   For columns with **less than 10%** censorship: Censored values are replaced with **66%** of the LOD numerical value.
        *   For columns with censorship **between 10% and 30%**: Censored values are replaced with **33%** of the LOD numerical value.
    *   **When NO numerical value is present (e.g., `<LD`, `<LOD`):** Conversion to LOD fractions is bypassed. Censored values are safely transformed into missing data (`NaN`), allowing the column to remain operational for mathematical calculations.
5.  **Type Conversion:** Ensuring the entire resulting dataset possesses a purely numerical format (float).
6.  **Audit Log:** Generation of a secondary tab in the resulting Excel file, documenting all chemical elements discarded due to censorship rules.

## 💻 How to Use in Google Colab

1.  Upload this Notebook (`.ipynb`) to your Google Drive and open it via Google Colab.
2.  Run the import cell. Colab will request permission to mount your Google Drive. Grant authorization.
3.  Ensure your input files (`.xlsx` format) are available in your Google Drive.
4.  Scroll to the bottom of the notebook and adjust the file path in the execution function.
    *   Example: `executar_pipeline_geoquimico("/content/drive/MyDrive/Your_Path/Your_File.xlsx")`
5.  Run all cells. The resulting file (e.g., `resultado_geoquimico_tratado.xlsx`) will be generated in the Colab environment containing the treated data and removal log tabs.

## 🗂️ Resulting File Structure

The pipeline consolidates the results into a new `.xlsx` Excel file with the following tabs:
*   `Dados_Tratados`: Contains the cleaned, imputed, and converted data matrix.
*   `Log_Remocao`: Audit table listing columns eliminated by the 30% censorship criterion.
