# Dados do COVID-19 no Brasil em 2020 - BigQuery-Colab
Análise exploratória dos casos e óbitos de COVID-19 no Brasil, focada em 2020 e nível UF (estados), usando Google BigQuery (dataset público) e Python no Google Colab.
Objetivo: praticar extração via SQL com pandas-gbq, tratamento/EDA em pandas e responder a 4 perguntas de negócio com agrupamentos e visualizações.

## Dataset

Origem: Google Cloud Public Datasets

Tabela: bigquery-public-data.covid19_open_data.covid19_open_data

Granularidade usada:

Brasil (nacional): subregion1_name IS NULL

Estados (UF): REGEXP_CONTAINS(location_key, '^BR_[A-Z]{2}$') e subregion2_name IS NULL

Período analisado: 2020-01-01 a 2020-12-31

Observação: quando new_confirmed/new_deceased faltam, calculamos diários como diferença dos acumulados, truncando negativos (revisões).

## Requisitos / Como reproduzir

Google Colab (recomendado)

Um Projeto GCP (ex.: prime-pod-472415-d7) com a BigQuery API habilitada

No notebook:

Autentique: from google.colab import auth; auth.authenticate_user()

Instale: !pip install pandas-gbq --quiet

Defina:

PROJECT_ID = "seu-project-id"

TABLE = "bigquery-public-data.covid19_open_data.covid19_open_data"

## Perguntas de negócio

Quais estados tiveram mais casos em 2020? (ranking por UF)

Como evoluíram os casos mês a mês no Brasil em 2020? (tendência nacional)

Qual foi a letalidade (CFR) por estado em 2020? (óbitos_2020 / casos_2020)

No mês de pico nacional em 2020, quais estados mais contribuíram? (composição do pico)

## Resultados & Conclusões 

- **Pico nacional:** **12-2020**, com **1.340.095** casos.
- **Top estados do ano:** **São Paulo (1.465.311)**, **Minas Gerais (544.629)**, **Bahia (494.223)** — **32,6%** do total.
- **CFR (óbitos/casos):** **Rio de Janeiro (5,86%)**, **Pernambuco (4,34%)**.
- **Mês de pico (2020-12):** os 2 estados com mais casos somaram **26,6%** do total.
- **Limitações:** possíveis revisões nos acumulados; sem taxa per capita nesta versão.
- **Próximos passos:** normalizar por 100 mil hab.; analisar positividade se `new_tested` existir; comparar 2020 vs 2021.


## Metodologia

Extração: pandas_gbq.read_gbq() com SQL padrão; filtragem por país/UF/ano.

Tratamento:

Conversão de tipos; remoção de duplicatas por (date, location_key)

Fallback diário via diff() dos acumulados (cumulative_*) com clip(lower=0)

Análises: groupby/agg, agregação mensal para tendência nacional, ranking por UF, CFR por UF, e composição no mês de pico.

Visualizações: matplotlib (linhas/barras).

START = "2020-01-01", END = "2020-12-31"

## Fonte

BigQuery — Google Cloud Public Datasets: bigquery-public-data.covid19_open_data.covid19_open_data

Licenças/termos conforme a página oficial do dataset no Google Cloud.

## Notas

Este projeto é parte do exercício final do curso EBAC – Análise de Dados (prática com BigQuery + Colab).
