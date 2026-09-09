# 🌍 GeoKit: Ferramentas Utilitárias para Dados Geocientíficos

![Python Version](https://img.shields.io/badge/python-3.8%2B-blue.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-brightgreen.svg)

**GeoKit** é uma biblioteca Python e repositório de scripts criado para otimizar o processamento, limpeza e análise de dados geocientíficos. O projeto foca em utilitários para petrofísica, geoquímica, estratigrafia e aplicação de estatística robusta em dados de subsuperfície.

## 🚀 Funcionalidades Principais

O pacote está dividido em módulos focados em domínios específicos das geociências:

### 🛢️ Petrofísica (`geokit.petrophysics`)
* Leitura e parseamento rápido de arquivos `.las`.
* Agrupamento de ferramentas baseado em conhecimento de domínio (Domain-Knowledge-Based Tool Grouping).
* Tratamento e normalização de curvas de perfis de poço.

### 🧪 Geoquímica (`geokit.geochemistry`)
* Limpeza de dados e tratamento automatizado de Limites de Detecção (LOD).
* Rotinas de interpolação sistemática (ex: padronização de amostragem para steps de 15cm).

### 🪨 Estratigrafia (`geokit.stratigraphy`)
* Processamento simplificado de arquivos litológicos contendo pares de profundidade e códigos de fácies.
* Mapeamento numérico-categórico para modelos de Machine Learning.

### 📊 Estatística Robusta e Detecção de Anomalias (`geokit.stats`)
* Estimadores robustos para evitar o mascaramento de dados (Minimum Covariance Determinant - MCD, Median Absolute Deviation - MAD).
* Cálculo de Z-score modificado e Distância de Mahalanobis para identificação de outliers.

## ⚙️ Instalação

Clone este repositório para a sua máquina local:

```bash
git clone [https://github.com/](https://github.com/)[SEU-USUARIO]/[NOME-DO-REPOSITORIO].git
cd [NOME-DO-REPOSITORIO]
