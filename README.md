# Checkpoint 2 — Machine Learning Aplicado à Energia

Projeto acadêmico que aplica técnicas de classificação e regressão a dados do setor energético, incluindo fontes de geração elétrica, radiação solar e estabilidade da rede.

## Integrantes

| Nome | RM |
|---|---|
| Guilherme Figueira Velloso | 568827 |
| José Augusto Ribeiro Freire Manfrinato | 571151 |
| Lais da Silva Dias | 569943 |
| João Augusto Poloniato Telles | 571443 |
| Thiago Soalheiro Diamantino | 569316 |
| Kauan Damasceno de Lima | 573727 |

## Objetivos

O projeto está dividido em três análises:

1. **Classificação de fontes de energia:** identificar empreendimentos solares, eólicos e hidráulicos usando potência e localização.
2. **Estimativa da radiação solar:** prever a radiação solar em Petrolina (PE) a partir de condições meteorológicas e hora local.
3. **Estabilidade da rede elétrica:** comparar duas configurações de Regressão Linear para prever a variável `stab`.

## Tecnologias utilizadas

- Python
- Pandas e NumPy
- Matplotlib e Seaborn
- Scikit-learn
- Jupyter Notebook e Google Colab
- Bibliotecas nativas: `json`, `urllib` e `csv`

## Notebooks e dados

| Arquivo | Conteúdo |
|---|---|
| `Checkpoint2_SERS.ipynb` | Classificação dos empreendimentos da ANEEL e regressão da radiação solar |
| `RESOLVIDO_AULA_07_Regressão_Linear_com_Dados_de_Energia.ipynb` | Comparação de modelos de Regressão Linear para estabilidade da rede |
| `aneel_classificacao_orange.csv` | Dados de classificação exportados pelo notebook |
| `meteo_regressao_orange.csv` | Dados meteorológicos exportados pelo notebook |

## 1. Classificação de fontes de energia — ANEEL

### Dados e preparação

São utilizados dados do **Sistema de Informações de Geração da ANEEL (SIGA)**, consultados pela API pública.

A execução registrada no notebook apresenta **3.876 empreendimentos**:

| Fonte | Quantidade |
|---|---:|
| Hidráulica | 1.476 |
| Solar | 1.200 |
| Eólica | 1.200 |

As entradas são `potencia_kw`, `latitude` e `longitude`. O alvo é `fonte`, com as classes Solar, Eólica e Hidráulica.

As categorias `UHE`, `PCH` e `CGH` são agrupadas como Hidráulica. Nomes, códigos e siglas que identificam diretamente a fonte não são utilizados como entradas.

A divisão é estratificada, com **80% para treino**, **20% para teste** e `random_state=42`. Para o KNN, a padronização é ajustada somente nos dados de treino.

### Resultados

| Algoritmo | Acurácia | Precisão macro | Recall macro | F1 macro |
|---|---:|---:|---:|---:|
| Árvore de Decisão | 0,9613 | 0,9614 | 0,9610 | 0,9612 |
| KNN | 0,9652 | 0,9663 | 0,9636 | 0,9648 |
| Random Forest | **0,9755** | **0,9769** | **0,9741** | **0,9753** |

O **Random Forest** apresentou o melhor desempenho nas quatro métricas, com acurácia de **97,55%**.

A amostra possui limites de consulta por tipo de empreendimento e não representa a participação de cada fonte na matriz energética brasileira. A data de coleta não está registrada no notebook.

## 2. Estimativa da radiação solar — Open-Meteo

### Dados e preparação

São utilizados dados históricos do **Open-Meteo** para Petrolina (PE):

- **Coordenadas:** latitude `-9.39` e longitude `-40.50`.
- **Período:** 01/04/2025 a 30/06/2025.
- **Fuso horário:** `America/Recife`.
- **Horários selecionados:** das 7h às 17h.
- **Registros válidos:** 1.001.

As entradas são temperatura, umidade relativa, cobertura de nuvens, velocidade do vento e hora local.

O alvo é `radiacao_w_m2`, correspondente à radiação solar global horizontal média da hora anterior, em **W/m²**.

A divisão preserva a ordem temporal: **800 registros iniciais para treino** e **201 registros finais para teste**, sem embaralhamento. As transformações são ajustadas somente no treino.

### Resultados

| Algoritmo | MAE (W/m²) | MSE ((W/m²)²) | R² |
|---|---:|---:|---:|
| Regressão Linear | 145,205 | 30.034,201 | 0,360 |
| Random Forest | **66,123** | **7.214,689** | **0,846** |
| Gradient Boosting | 68,180 | 7.642,268 | 0,837 |

O **Random Forest** apresentou o menor MAE, o menor MSE e o maior R². A variável `hora` teve a maior importância calculada pelo modelo, seguida por temperatura e umidade.

Os dados são estimativas históricas de modelos/reanálise. A estimativa de radiação não corresponde diretamente à energia elétrica produzida por painéis, que também depende de área, eficiência, orientação, temperatura e perdas do sistema.

## 3. Estabilidade da rede elétrica

### Dados e preparação

O conjunto **Electrical Grid Stability** contém **10.000 registros e 14 colunas**, sem valores ausentes na análise registrada.

O alvo dos modelos é a variável numérica `stab`. A coluna categórica `stabf` não é utilizada como entrada.

São comparadas duas configurações de Regressão Linear:

| Modelo | Variáveis utilizadas |
|---|---|
| Modelo 1 | `g3`, `g2`, `tau2`, `g1` e `tau3`: cinco maiores correlações absolutas com `stab` |
| Modelo 2 | `tau1`, `tau2`, `tau3`, `tau4`, `g1`, `g2`, `g3` e `g4` |

Ambos utilizam **80% dos dados para treino**, **20% para teste** e `random_state=42`, sendo avaliados nos mesmos registros.

### Resultados

| Modelo | R² | MAE | MSE |
|---|---:|---:|---:|
| Modelo 1 | 0,401770 | 0,023310 | 0,000811 |
| Modelo 2 | **0,645229** | **0,017553** | **0,000481** |

O **Modelo 2** apresentou melhor desempenho nas três métricas, reduzindo o MAE em aproximadamente **25%** e o MSE em aproximadamente **41%**.

A seleção por correlação foi feita sobre o conjunto completo, seguindo a atividade. Em uma avaliação rigorosa de generalização, essa seleção deve utilizar somente os dados de treino.


## Fontes dos dados

- [ANEEL — SIGA](https://dadosabertos.aneel.gov.br/dataset/siga-sistema-de-informacoes-de-geracao-da-aneel)
- [Open-Meteo — API de dados históricos](https://open-meteo.com/en/docs/historical-weather-api)
- [Electrical Grid Stability — CSV utilizado na atividade](https://raw.githubusercontent.com/prof-atritiack/CP2-ML-SERS/refs/heads/main/Data_for_UCI_named.csv)

## Conclusão

Nos experimentos registrados, o Random Forest apresentou o melhor desempenho na classificação das fontes de energia e na estimativa da radiação solar.

Na análise de estabilidade da rede, utilizar todas as variáveis `tau` e `g` produziu resultados melhores do que selecionar somente as cinco maiores correlações individuais.

As análises mostram a importância da seleção de atributos, da separação adequada entre treino e teste e da comparação de modelos por múltiplas métricas.
