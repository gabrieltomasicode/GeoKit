# 🌍 GeoKit

O **GeoKit** é um conjunto de notebooks para apoiar fluxos de trabalho em geociências, com foco em:
- tratamento e padronização de dados geoquímicos;
- integração de informações litológicas com perfis LAS;
- visualização de perfis de poço em formatos estáticos e interativos.

Os notebooks foram estruturados para uso prático em ambiente de projeto, com etapas de ingestão de dados, validações, processamento e exportação.

---

## 📁 Estrutura de pastas

```text
GeoKit/
├── README.md
├── LimpezaETratamento/
│   ├── Geochem_Grid_Resampler.ipynb
│   ├── GeochemLOD_Processor.ipynb
│   └── LithoLAS_Integrator.ipynb
└── Vizualizers/
    ├── Scatter_to_well_log.ipynb
    └── visualizador_perfis_poco_oop.ipynb
```

### Descrição das pastas

- **LimpezaETratamento/**  
  Concentra notebooks de preparação de dados: regularização de amostragem, tratamento de censura (LOD) e integração litologia + LAS.

- **Vizualizers/**  
  Contém notebooks voltados à análise visual de dados de poço, incluindo dashboards interativos e visualização em múltiplas trilhas.

---

## 🧩 Descrição completa dos notebooks

### 1) `LimpezaETratamento/Geochem_Grid_Resampler.ipynb`
**Objetivo:** regularizar dados geoquímicos em um grid vertical fixo (padrão de 15 cm).  
**O que faz:**
- carrega planilha de entrada (`.xlsx`) e identifica automaticamente colunas de poço e profundidade;
- aplica validações para evitar travamentos e falhas de processamento (colunas obrigatórias, profundidade nula, profundidade não numérica);
- resolve profundidades duplicadas por regra de agregação (média para variáveis numéricas e primeiro valor para categóricas);
- cria grid contínuo por poço e interpola por vizinho mais próximo (`merge_asof`) com tolerância numérica ajustada;
- preenche lacunas preservando tipo de dado (`-9999` para numéricas e `Sem_Dado` para categóricas);
- exporta o resultado final em novo arquivo Excel regularizado.

**Resultado esperado:** base padronizada em malha regular, pronta para comparação entre poços, modelagem e controle de qualidade.

---

### 2) `LimpezaETratamento/GeochemLOD_Processor.ipynb`
**Objetivo:** tratar dados geoquímicos com valores abaixo do limite de detecção (LOD).  
**O que faz:**
- realiza leitura segura do Excel e limpeza inicial dos nomes de colunas;
- identifica valores censurados (ex.: `<0,5`, `<20`, `<LD`, `<LOD`);
- calcula proporção de censura por coluna para decidir entre descarte ou manutenção;
- remove colunas com censura acima do limite definido (30%);
- aplica imputação dinâmica para colunas elegíveis (66% do LOD para baixa censura e 33% para censura intermediária);
- converte os dados para formato numérico quando necessário;
- exporta arquivo tratado com aba principal de dados e aba de auditoria (`Log_Remocao`) com elementos descartados.

**Resultado esperado:** dataset geoquímico consistente para análises estatísticas, com rastreabilidade das colunas removidas.

---

### 3) `LimpezaETratamento/LithoLAS_Integrator.ipynb`
**Objetivo:** integrar informações litológicas (CSV) a arquivos LAS por poço e profundidade.  
**O que faz:**
- instala e importa dependências necessárias (ex.: `lasio`);
- lê arquivos LAS (base completa e eletrofácies) e cria estrutura de pareamento por nome-base do poço;
- lê tabelas litológicas em CSV e associa cada tabela ao LAS correspondente;
- converte classes litológicas para valores numéricos por mapeamento de fácies;
- preenche a curva de fácies por intervalo de topo/base usando máscaras booleanas robustas;
- adiciona/atualiza curva `AF` no LAS e exporta arquivos processados com sufixo de saída.

**Resultado esperado:** arquivos LAS enriquecidos com curva litológica integrada para interpretação geológica e correlação de perfis.

---

### 4) `Vizualizers/Scatter_to_well_log.ipynb`
**Objetivo:** conectar seleção em crossplot com destaque em perfil de poço (well log).  
**O que faz:**
- carrega múltiplos arquivos LAS para DataFrames;
- solicita ao usuário o poço e as curvas de interesse para os eixos do crossplot;
- limpa valores nulos/sentinela para manter consistência entre pontos e profundidade;
- cria painel interativo com Plotly (`scatter` + perfil de poço em subplots);
- habilita seleção Lasso/Box no crossplot e projeta os pontos selecionados no perfil;
- inclui botão para limpeza de seleção e atualização visual em tempo real.

**Resultado esperado:** análise exploratória rápida de relações entre curvas e posicionamento em profundidade.

---

### 5) `Vizualizers/visualizador_perfis_poco_oop.ipynb`
**Objetivo:** plotagem de perfis de poço com arquitetura orientada a objetos e dois motores gráficos.  
**O que faz:**
- oferece modo **estático** (Matplotlib) e **interativo** (Plotly);
- encapsula leitura e preparo de dados LAS na classe `WellData`;
- define camada base de plotagem (`BasePlotter`) e implementações específicas para cada motor;
- permite uso de layout padrão (multitrilhas prontas) ou layout customizado (logs, cores, estilos, preenchimento e limites por trilha);
- trata convenções comuns de poço (ex.: `DEPT` → `DEPTH`) e ausência de logs com mensagens de aviso.

**Resultado esperado:** visualização técnica flexível de perfis, adequada tanto para inspeção rápida quanto para montagem customizada de painéis.

---

## 🚀 Como executar

### Ambiente recomendado
- **Google Colab** (recomendado)
- Python 3
- Dependências instaladas dentro do próprio notebook (`pip install`, quando necessário)

### Fluxo sugerido
1. Abrir o notebook desejado no Colab.
2. Montar o Google Drive (quando solicitado no notebook).
3. Ajustar os caminhos de entrada/saída para as suas pastas.
4. Executar as células na ordem.
5. Validar os arquivos exportados.

---

## ✅ Quando usar cada notebook

- Use **Geochem_Grid_Resampler** quando precisar padronizar amostragem por profundidade.
- Use **GeochemLOD_Processor** quando houver censura por limite de detecção nos dados geoquímicos.
- Use **LithoLAS_Integrator** quando precisar incorporar litologia em perfis LAS.
- Use **Scatter_to_well_log** para investigar relações entre curvas e localização em profundidade.
- Use **visualizador_perfis_poco_oop** para montar perfis completos com layout técnico estático ou interativo.
