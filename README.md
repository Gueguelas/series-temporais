# Atividade de Séries Temporais

Estudo didático de séries temporais: carregamento, limpeza, classificação de estacionariedade e modelagem.

## Bases utilizadas

- **Preço de casas** (Property Sales) — preço médio mensal de imóveis vendidos, 2007 a 2019. Fonte: Kaggle (htagholdings/property-sales).
- **Consumo de energia** (Electric Power) — potência ativa global média mensal de uma residência, dez/2006 a nov/2010. Fonte: Kaggle (uciml/electric-power-consumption-data-set).
- **Vendas Amazon** — receita total mensal agregada, 2022 a 2023. Fonte: Kaggle (aliiihussain/amazon-sales-dataset).
- **Vitórias do Brasil** — total de vitórias por ano da seleção brasileira, 1914 a 2023. Fonte: Kaggle (azminetoushikwasi/brazil-all-international-matches-19142023).

## Tratamento

Todas as séries passaram por:
1. Remoção de valores nulos
2. Conversão de datas para datetime
3. Reamostragem mensal (média para energia e imóveis, soma para Amazon)
4. Normalização — MinMax (0 a 1) para Amazon, energia e Brasil; Z-score para imóveis

A série do Brasil foi construída a partir do campo `Result`, contando vitórias (`W`) agrupadas por ano.

## Análise de estacionariedade

Foram usados dois testes em cada série:
- **ADF** (Augmented Dickey-Fuller) — H0: a série tem raiz unitária (não estacionária). Se p < 0.05, rejeita H0.
- **KPSS** — H0: a série é estacionária. Se p < 0.05, rejeita H0.

A combinação dos dois dá mais segurança na conclusão.

## Modelagem

Foi usado o modelo **AutoReg** (autoregressivo) nas séries estacionárias. O processo:

1. Split treino/teste (85/15)
2. Busca do melhor lag (1 a 30) minimizando MSE no teste
3. Treino com o lag escolhido
4. Avaliação com MAE e RMSE
5. Avaliação de resíduos com Ljung Box
