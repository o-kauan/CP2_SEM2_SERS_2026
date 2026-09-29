# Checkpoint 2 — Regressão Linear e Estabilidade da Rede Elétrica

Projeto acadêmico de Machine Learning para construir e comparar dois modelos de **Regressão Linear**, utilizando o conjunto de dados **Electrical Grid Stability**.

O objetivo é prever a variável numérica `stab` e avaliar como a seleção das variáveis de entrada influencia o desempenho dos modelos.

## Integrantes

| Nome | RM |
|---|---|
| Guilherme Figueira Velloso | 568827 |
| José Augusto Ribeiro Freire Manfrinato | 571151 |
| Lais da Silva Dias | 569943 |
| João Augusto Poloniato Telles | 571443 |
| Thiago Soalheiro Diamantino | 569316 |
| Kauan Damasceno de Lima | 573727 |

## Tecnologias utilizadas

- Python
- Pandas e NumPy
- Matplotlib e Seaborn
- Scikit-learn
- Jupyter Notebook / Google Colab

## Dataset

O conjunto de dados contém **10.000 registros e 14 colunas**, sem valores ausentes na análise realizada.

As colunas estão organizadas em:

| Colunas | Descrição |
|---|---|
| `tau1` a `tau4` | Tempos de reação |
| `p1` a `p4` | Potências de produção ou consumo |
| `g1` a `g4` | Coeficientes de elasticidade de preço |
| `stab` | Variável numérica de estabilidade utilizada como alvo |
| `stabf` | Classificação categórica de estabilidade, não utilizada nos modelos |

O CSV é carregado diretamente no notebook:

[Dados utilizados na atividade](https://raw.githubusercontent.com/prof-atritiack/CP2-ML-SERS/refs/heads/main/Data_for_UCI_named.csv)

## Etapas do projeto

1. Carregamento e exploração inicial dos dados.
2. Verificação dos tipos de dados, valores ausentes e estatísticas descritivas.
3. Construção da matriz de correlação e do mapa de calor.
4. Seleção das cinco variáveis com maior correlação absoluta com `stab`.
5. Visualização das relações por meio de gráficos de dispersão.
6. Separação dos dados em treino e teste.
7. Treinamento dos dois modelos de Regressão Linear.
8. Comparação das métricas e análise dos resultados.

## Modelos avaliados

### Modelo 1 — Cinco maiores correlações

Utiliza as cinco variáveis com maior correlação absoluta com `stab`, excluindo a própria variável alvo:

| Variável | Correlação com `stab` |
|---|---:|
| `g3` | 0,3082 |
| `g2` | 0,2936 |
| `tau2` | 0,2910 |
| `g1` | 0,2828 |
| `tau3` | 0,2807 |

### Modelo 2 — Todas as variáveis `tau` e `g`

Utiliza oito variáveis:

`tau1`, `tau2`, `tau3`, `tau4`, `g1`, `g2`, `g3` e `g4`.

Nos dois modelos, os dados foram divididos em **80% para treinamento e 20% para teste**, utilizando `random_state=42`. Assim, ambos foram avaliados nos mesmos 2.000 registros de teste.

## Resultados

| Modelo | R² | MAE | MSE |
|---|---:|---:|---:|
| Modelo 1 — Cinco maiores correlações | 0,401770 | 0,023310 | 0,000811 |
| Modelo 2 — Todas as variáveis `tau` e `g` | **0,645229** | **0,017553** | **0,000481** |

- **R²:** quanto maior, melhor o desempenho em relação à previsão pela média.
- **MAE:** quanto menor, menor o erro absoluto médio.
- **MSE:** quanto menor, menor o erro quadrático médio, com maior penalização dos erros de maior magnitude.

O **Modelo 2 apresentou o melhor desempenho nas três métricas**. Em comparação ao Modelo 1, reduziu o MAE em aproximadamente **25%** e o MSE em aproximadamente **41%**.

Os resultados indicam que selecionar variáveis apenas pela correlação individual pode deixar de fora informações úteis para a previsão.

## Como executar

### Google Colab

1. Acesse o [Google Colab](https://colab.research.google.com/).
2. Abra o notebook `.ipynb` do projeto por upload ou pela opção GitHub.
3. Execute todas as células em ordem.

### Ambiente local

Instale as dependências:

```bash
pip install pandas numpy matplotlib seaborn scikit-learn notebook
```

Inicie o Jupyter Notebook:

```bash
jupyter notebook
```

Abra o notebook do projeto e execute as células sequencialmente.

> É necessário acesso à internet para carregar o CSV pela URL utilizada no notebook.

## Considerações finais

Entre as configurações avaliadas, o modelo com todas as variáveis `tau` e `g` foi a melhor opção para prever `stab`, explicando aproximadamente **64,5% da variação observada no conjunto de teste**.

A seleção por correlação foi realizada sobre o conjunto completo, conforme a sequência proposta na atividade. Em uma avaliação com separação rigorosa entre treino e teste, essa seleção deve ser feita apenas com os dados de treinamento.

Projeto desenvolvido para o **Checkpoint 2 — 2º semestre de 2026**.
