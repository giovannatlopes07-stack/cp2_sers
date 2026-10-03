# Projeto de Aprendizado de Máquina: Classificação de Fontes & Regressão de Radiação Solar

Este repositório contém as soluções para duas tarefas práticas de ciência de dados usando dados reais do cenário energético e meteorológico brasileiro.

---

## 1. Objetivo do Projeto
O objetivo deste projeto é desenvolver modelos preditivos robustos aplicados ao setor de energia:
1. **Tarefa 1 (Classificação):** Categorizar o tipo de fonte de geração de energia de empreendimentos brasileiros (`Solar`, `Eólica` ou `Hidráulica`) com base na capacidade de geração cadastrada (`potencia_kw`) e na sua localização geográfica (`latitude` e `longitude`).
2. **Tarefa 2 (Regressão):** Estimar a radiação solar instantânea incidente de ondas curtas ($W/m^2$) utilizando variáveis meteorológicas locais (temperatura, umidade relativa, cobertura de nuvens, velocidade do vento e hora do dia).

---

## 2. Fontes e Período dos Dados

* **Dados de Classificação (ANEEL):** Obtidos de forma programática via API de Dados Abertos da Agência Nacional de Energia Elétrica (ANEEL). Representam outorgas ativas registradas e atualizadas até o momento da consulta.
* **Dados Meteorológicos (Open-Meteo):** Obtidos a partir da API pública de dados históricos Open-Meteo para a região de Petrolina-PE (latitude -9.39, longitude -40.50), abrangendo o período de **01 de abril de 2025 a 30 de junho de 2025** (segundo trimestre).

---

## 3. Resumo dos Resultados Obtidos

### Tarefa 1: Classificação de Fontes de Geração (ANEEL)
Foram comparados os algoritmos KNN, Regressão Logística e Random Forest com divisão estratificada (80% treino / 20% teste):
* **Random Forest Classifier (Melhor Modelo):** Acurácia de **97,55%** e F1-Score (Macro) de **97,53%**. Demonstrou excelente capacidade de separar as usinas pelas coordenadas e potência.
* **K-Nearest Neighbors (KNN):** Acurácia de **96,65%**, se beneficiando da forte proximidade geográfica de plantas similares.
* **Regressão Logística:** Acurácia de **82,35%**, confirmando a alta não linearidade das fronteiras de decisão.

### Tarefa 2: Regressão de Radiação Solar ($W/m^2$)
Foram avaliados três algoritmos de regressão em uma divisão temporal pura cronológica (800 amostras iniciais para treino / 201 para teste):

| Algoritmo | MAE ($W/m^2$) | MSE ($(W/m^2)^2$) | $R^2$ (Coef. Determinação) |
| :--- | :---: | :---: | :---: |
| **Regressão Linear** | 145.20 | 30034.20 | 0.3598 |
| **Decision Tree Regressor** | 90.28 | 14473.81 | 0.6915 |
| **Random Forest Regressor (Melhor)** | **66.71** | **7278.12** | **0.8449** |

* **Destaque:** O **Random Forest Regressor** conseguiu modelar com precisão de **84,49%** ($R^2$) as variações bruscas diárias de radiação solar, enquanto o modelo linear falhou devido às características complexas do movimento solar e da cobertura de nuvens.

---

## 4. Instruções para Executar o Notebook

Para executar o notebook de ponta a ponta sem erros, siga os passos abaixo:
1. **Dependências:** Certifique-se de ter as seguintes bibliotecas Python instaladas em seu ambiente Jupyter ou Google Colab:
   ```bash
   pip install pandas numpy scikit-learn seaborn matplotlib
   ```
2. **Execução das Células:** Execute as células sequencialmente de cima para baixo. 
   * As primeiras células realizarão as chamadas diretas às APIs públicas da ANEEL e do Open-Meteo para extrair e gerar os arquivos CSV locais (`aneel_classificacao_orange.csv` e `meteo_regressao_orange.csv`).
   * Não é necessário fazer download externo de arquivos; o próprio script cria e armazena os conjuntos de dados no diretório de trabalho.
3. **Divisão de Dados:** O notebook aplica a divisão aleatória estratificada para a Tarefa 1 e divisão puramente sequencial temporal para a Tarefa 2. Não modifique as sementes aleatórias (`random_state`) para manter a reprodutibilidade dos resultados.
