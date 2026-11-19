# Oportuniza

Projeto da disciplina **Inteligência Artificial & Machine Learning**.

Agrupamos os 96 distritos de São Paulo com PCA + KMeans, usando indicadores de vulnerabilidade social e a oferta de ETECs do Centro Paula Souza, para entender o perfil de Capão Redondo, Campo Limpo e Jardim Ângela.

## Integrantes

| RM | Nome |
|---|---|
| 558385 | Alexia Ramalho |
| 557943 | Enzo Real |
| 555454 | Gustavo Pasquini |
| 559008 | Hellen Silva |
| 557397 | Lorenzo Acquesta |

## Estrutura

```
ml/                  notebook executado e figuras
dados/               base utilizada e documentação (README.md)
requirements.txt     bibliotecas necessárias
RELATORIO_ML.md      relatório do problema à conclusão
```

## Como rodar

No Google Colab: abra `ml/Oportuniza_PCA_KMeans.ipynb`, execute as células na ordem e envie `dados/base_distrital_oportuniza.csv` quando pedido.

Localmente:

```
pip install -r requirements.txt
```

e abra o notebook dentro da pasta `ml/` (ele encontra o CSV em `../dados/`).
