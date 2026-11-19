# Dados

Arquivo usado no projeto: `base_distrital_oportuniza.csv`, com 96 linhas (uma por distrito de São Paulo) e 18 colunas.

## Fontes

| Fonte | O que foi usado | Referência / acesso |
|---|---|---|
| [ObservaSampa – Dados Abertos](https://dados-abertos-observasampa.prefeitura.sp.gov.br/Download) | População, famílias em extrema pobreza e baixa renda, domicílios em favelas, taxa de universalização da educação básica | Ano de referência 2022. Acesso em 18/11/2026 |
| [Centro Paula Souza – Onde Estudar ETECs](https://dados.cps.sp.gov.br/onde-estudar-etecs/) | Unidades de ETEC e cursos listados | Consultado em 2026. Lista filtrada para 44 unidades no município de São Paulo. Acesso em 19/11/2026 |
| [GeoSampa – divisão político-administrativa](https://metadados.geosampa.prefeitura.sp.gov.br/geonetwork/srv/api/records/af4dcf63-fe60-4066-8522-5ae2ac63fc5d) | Camada de distritos, usada para localizar cada ETEC no seu distrito | Acesso em 19/11/2026 |

## Colunas

| Coluna | Descrição |
|---|---|
| `distrito` | Nome do distrito |
| `ano_referencia` | Ano dos indicadores sociais (2022) |
| `populacao_total` | População total do distrito |
| `populacao_0_17` | População de 0 a 17 anos |
| `populacao_0_17_pct` | Percentual da população com 0 a 17 anos |
| `familias_extrema_pobreza` | Famílias em extrema pobreza |
| `familias_baixa_renda` | Famílias de baixa renda |
| `domicilios_favelas_estimados` | Domicílios em favelas (estimativa) |
| `taxa_universalizacao_educacao_basica_pct` | Taxa de universalização da educação básica (%). Pode passar de 100%, conforme a nota do indicador |
| `familias_extrema_pobreza_por_100mil_hab_proxy` | Famílias em extrema pobreza por 100 mil habitantes (aproximação) |
| `familias_baixa_renda_por_100mil_hab_proxy` | Famílias de baixa renda por 100 mil habitantes (aproximação) |
| `domicilios_favelas_por_100mil_hab_proxy` | Domicílios em favelas por 100 mil habitantes (aproximação) |
| `etecs_cps_qtd_2026` | Número de unidades ETEC no catálogo do distrito |
| `registros_cursos_cps_qtd_2026` | Número de registros de curso/modalidade no catálogo do distrito |
| `etecs_cps_por_100mil_hab_proxy` | ETECs por 100 mil habitantes (aproximação) |
| `registros_cursos_cps_por_100mil_hab_proxy` | Registros de cursos por 100 mil habitantes (aproximação) |
| `unidades_etec_cps_2026` | Nomes das unidades (texto, fica vazio onde não há ETEC) |
| `cursos_cps_listados_2026` | Nomes dos cursos (texto, fica vazio onde não há ETEC) |

As **sete variáveis usadas no modelo** são: `populacao_0_17_pct`, `familias_extrema_pobreza_por_100mil_hab_proxy`, `familias_baixa_renda_por_100mil_hab_proxy`, `domicilios_favelas_por_100mil_hab_proxy`, `taxa_universalizacao_educacao_basica_pct`, `etecs_cps_por_100mil_hab_proxy` e `registros_cursos_cps_por_100mil_hab_proxy`. As colunas de texto são só descritivas e não entram no modelo.

## Cuidados ao interpretar

- Os dados sociais são de 2022 e a oferta de ETEC é de 2026. Os campos `_por_100mil_hab_proxy` dividem dados de 2026 pela população de 2022, então misturam períodos e são aproximações.
- A oferta conta unidades e cursos que aparecem no catálogo, não vagas abertas, turmas ou matrículas.
- O recorte cobre só as ETECs do Centro Paula Souza. Não inclui CEUs, SENAI, ONGs nem outros cursos gratuitos da cidade.
- 58 dos 96 distritos não têm nenhuma ETEC no catálogo, por isso as variáveis de ETEC têm muitos zeros.
- Os nomes de unidades e cursos ficam vazios nesses distritos. Por isso o CSV é lido com `keep_default_na=False`.
