# Relatório de Machine Learning: Oportuniza

**Disciplina:** Inteligência Artificial & Machine Learning
**Integrantes:** Alexia Ramalho (558385), Enzo Real (557943), Gustavo Pasquini (555454), Hellen Silva (559008) e Lorenzo Acquesta (557397)

## 1. Problema

A Oportuniza quer orientar jovens da Zona Sul de São Paulo para cursos técnicos gratuitos. Antes de recomendar cursos, é preciso entender o contexto de cada território. A pergunta de ML foi: **que perfil de vulnerabilidade e de acesso à formação técnica pública têm Capão Redondo, Campo Limpo e Jardim Ângela em comparação com os demais distritos?** Como nenhum distrito vem classificado, é um problema não supervisionado (agrupamento).

## 2. Dados

Tabela com os **96 distritos** de São Paulo, combinando indicadores do ObservaSampa (2022), o catálogo de ETECs do Centro Paula Souza (2026, 44 unidades) e a divisão distrital do GeoSampa (fontes e colunas em `dados/README.md`). O modelo usa 7 variáveis proporcionais ao tamanho do distrito: % de 0 a 17 anos, famílias em extrema pobreza e de baixa renda, domicílios em favelas, universalização da educação básica, ETECs e registros de cursos (os quatro indicadores de contagem, por 100 mil habitantes). Não havia valores ausentes. Ressalvas: os períodos são diferentes (2022 e 2026) e 58 dos 96 distritos não têm nenhuma ETEC, então as variáveis de oferta têm muitos zeros.

## 3. Modelo e preparação

Usamos **KMeans**, porque queremos formar grupos de distritos parecidos sem variável-alvo, precedido de **PCA**, porque os indicadores sociais são muito correlacionados e a informação repetida pesaria demais na distância. Separamos 75% dos distritos para treino (72) e 25% para teste (24). Padronização, PCA e KMeans foram ajustados só com o treino, e o teste apenas recebeu as transformações.

## 4. Treino e hiperparâmetros

Testamos **56 combinações** de três hiperparâmetros: componentes do PCA (2 a 5), número de grupos *k* (2 a 8) e inicialização (`k-means++` ou `random`). Cada uma foi avaliada por **validação cruzada de 5 folds dentro do treino**, usando a silhouette (de -1 a 1; quanto mais perto de 1, mais separados os grupos). A melhor foi **k = 2, 2 componentes, `k-means++`**, com silhouette de validação de 0,308. O k = 3 ficou praticamente empatado (0,306), então k = 2 foi escolhido por ser mais simples.

## 5. Resultados

| Conjunto | Silhouette |
|---|---|
| Treino | 0,316 |
| Validação cruzada | 0,308 |
| Teste | 0,320 |

Os valores são praticamente iguais, então **não há sinal de overfitting** (o teste tem só 24 distritos, o que limita essa evidência). Em 30 amostras com reposição, o ARI médio foi **0,947** (mínimo 0,763), indicando grupos estáveis. Uma silhouette perto de 0,3 mostra, porém, que os grupos se sobrepõem em parte.

Com os 96 distritos, o modelo formou dois grupos de 48:

| Média do grupo | Grupo 0 | Grupo 1 |
|---|---|---|
| População de 0 a 17 anos (%) | 25,6 | 18,5 |
| Famílias em extrema pobreza (por 100 mil) | 6.825 | 3.173 |
| Domicílios em favelas (por 100 mil) | 4.183 | 778 |
| ETECs (por 100 mil) | 0,17 | 0,79 |

Capão Redondo, Campo Limpo e Jardim Ângela ficaram no **grupo 0**, o mais vulnerável. Os dois primeiros não têm ETEC no catálogo e Jardim Ângela tem uma. As demais médias e os gráficos (validação cruzada, treino contra teste e distritos no plano do PCA) estão no notebook e em `ml/figuras/`.

## 6. Conclusão para o cidadão

Os distritos mais vulneráveis tendem a ter menos ETECs por habitante: o grupo 0 tem cerca de um quarto da oferta do grupo 1. Para um jovem do Capão Redondo, isso significa morar num tipo de distrito com muita demanda e pouca oferta de ETEC no próprio território. Isso **não quer dizer que ele não tenha acesso a cursos**: pode estudar em ETECs de outros distritos, e o modelo não conhece transporte, vagas, critérios de seleção nem outras opções gratuitas (SENAI, CEUs, ONGs). Para orientar parcerias, este diagnóstico deve ser combinado com uma lista validada de cursos e vagas.

**Limites:** dados de períodos diferentes, indicadores aproximados, muitos zeros nas variáveis de ETEC, separação moderada entre os grupos e teste pequeno. O KMeans mostra semelhanças, mas não prova que renda ou oferta de ensino causem algum resultado.
