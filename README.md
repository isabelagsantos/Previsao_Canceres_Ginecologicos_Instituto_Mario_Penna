# Previsão de Cânceres Ginecológicos — Instituto Mário Penna

Aplicação de métodos estatísticos de previsão a uma série temporal de casos mensais de **diagnóstico de alguns cânceres ginecológicos** (CID C51 a C54 e C56) de **janeiro/2015 a dezembro/2024 no Hospital Luxemburgo/Instituto Mário Penna**, com o objetivo de prever novos diagnósticos futuros na instituição.

> 🚧 **Status:** projeto em andamento.

---

## Sumário

- [Objetivo](#objetivo)
- [Dados](#dados)
- [Metodologia](#metodologia)
- [Estrutura do projeto](#estrutura-do-projeto)
- [Pré-requisitos e pacotes](#pré-requisitos-e-pacotes)
- [Como executar](#como-executar)
- [Etapas já desenvolvidas](#etapas-já-desenvolvidas)
- [Próximos passos](#próximos-passos)
- [Fontes e referências](#fontes-e-referências)
- [Créditos e uso de ferramentas](#créditos-e-uso-de-ferramentas)

---

## Objetivo

Aplicar métodos estatísticos de previsão de séries temporais aos casos mensais de diagnóstico de câncer ginecológico no Hospital Luxemburgo (Instituto Mário Penna), de modo a estimar a demanda futura de novos diagnósticos na instituição.

## Dados

**Painel-Oncologia – BRASIL**
*Casos por Diagnóstico Detalhado segundo Mês/Ano do diagnóstico*

| Característica | Descrição |
|---|---|
| **Diagnósticos (CID-10)** | C51 – Neoplasia maligna da vulva<br>C52 – Neoplasia maligna da vagina<br>C53 – Neoplasia maligna do colo do útero<br>C54 – Neoplasia maligna do corpo do útero<br>C55 – Neoplasia maligna do útero, porção não especificada<br>C56 – Neoplasia maligna do ovário<br>C57 – Neoplasia maligna de outros órgãos genitais femininos e dos não especificados<br>C58 – Neoplasia maligna da placenta |
| **Período** | 2015 a 2024 (120 observações mensais) |
| **Estabelecimento diagnóstico** | 2200457 – Hospital Luxemburgo |
| **Fontes** | Sistema de Informação Ambulatorial (SIA), por meio do Boletim de Produção Ambulatorial Individualizado (BPA-I) e da Autorização de Procedimento de Alta Complexidade (APAC); Sistema de Informação Hospitalar (SIH); Sistema de Informações de Câncer (SISCAN) |
| **Atualização dos dados** | 15/10/2026 |

> Os dados foram extraídos de C51 a C58, mas a análise de previsão concentra-se nos CIDs **C51 a C54 e C56**.
>
> Os CIDs **C55** (Neoplasia maligna do útero, porção não especificada), **C57** (Neoplasia maligna de outros órgãos genitais femininos e dos não especificados) e **C58** (Neoplasia maligna da placenta) possuem **menos de 15 observações cada**, o que torna impossível realizar qualquer previsão confiável com essa quantidade de dados. Por isso, ficam fora da etapa de previsão.

### Série utilizada

Série mensal tratada, com uma coluna de data (`mes_ano_diag`) e uma coluna de casos por tipo de câncer (por exemplo, `diag_vulva`). Arquivo de exemplo:

```
serie_casos_diagnostico_cancer_ginecologico_C51_a_C58_mensal_2015_2024_tratada.csv
```

## Metodologia

1. **Análise exploratória** da série (síntese numérica, boxplots, visualização temporal e sazonal).
2. **Decomposição clássica** (tendência, sazonalidade e resíduo).
3. **Autocorrelação (ACF) e autocorrelação parcial (PACF)**.
4. **Testes de estacionariedade** (ADF e KPSS) e determinação do número de diferenças (`ndiffs` / `nsdiffs`).
5. **Divisão treino/teste:** 80% para treino (96 observações) e 20% para teste (24 observações).
6. **Modelagem e previsão** (em andamento).

## Estrutura do projeto

```
.
├── README.md
├── dados/          # séries mensais tratadas (.csv)
├── codigos/        # análises em R / R Markdown
└── resultados/     # tabelas e gráficos gerados
```

## Pré-requisitos e pacotes

- [R](https://www.r-project.org/) e [RStudio](https://posit.co/download/rstudio-desktop/)

```r
install.packages(c("tidyverse", "knitr", "kableExtra",
                   "patchwork", "forecast", "tseries"))
```

| Pacote | Uso |
|---|---|
| `tidyverse` | Manipulação de dados e gráficos (`dplyr`, `ggplot2`, etc.) |
| `knitr` / `kableExtra` | Tabelas formatadas para o relatório |
| `patchwork` | Composição de painéis de gráficos |
| `forecast` | Modelagem, decomposição, ACF/PACF e previsão |
| `tseries` | Testes ADF e KPSS |

## Como executar

1. Clone o repositório:
   ```bash
   git clone https://github.com/isabelagsantos/Previsao_Canceres_Ginecologicos_Instituto_Mario_Penna.git
   ```
2. Abra o arquivo `.Rmd` no RStudio.
3. Atualize o caminho do arquivo de dados na leitura:
   ```r
   dados <- read.csv("caminho/para/serie_casos_diagnostico_cancer_ginecologico_C51_a_C58_mensal_2015_2024_tratada.csv")
   ```
4. Execute os blocos em ordem (ou use **Knit**).

## Etapas já desenvolvidas

**Bloco 0 – Análise exploratória** (exemplo: câncer de vulva, C51)

- Criação da série temporal: `ts(..., start = c(2015, 1), frequency = 12)`
- Divisão em treino (96 meses) e teste (24 meses)
- Síntese numérica: média, mediana, moda, amplitude, variância, desvio padrão, coeficiente de variação, quartis, IQR
- Detecção de outliers (cercas de Tukey e escore z)
- Gráficos: boxplots (geral, por ano e por mês), série temporal, gráfico sazonal, decomposição clássica
- ACF e PACF
- Testes de estacionariedade (ADF e KPSS) nas séries original e diferenciada

## Próximos passos

- [ ] Replicar a análise exploratória para C52, C53, C54 e C56
- [ ] Ajustar modelos de previsão (ex.: suavização exponencial, ARIMA/SARIMA)
- [ ] Avaliar acurácia no conjunto de teste (RMSE, MAE, MAPE)
- [ ] Gerar previsões futuras e intervalos de confiança
- [ ] Redigir conclusões e discussão

## Fontes e referências

- REIS, E. A.; REIS, I. A. (2002). *Análise Descritiva de Dados*. Relatório Técnico do Departamento de Estatística da UFMG. Disponível em: [www.est.ufmg.br](https://www.est.ufmg.br)
- Painel-Oncologia – Brasil (Ministério da Saúde), com dados do SIA (BPA-I e APAC), SIH e SISCAN.

## Créditos e uso de ferramentas

- **Claude:** auxílio no código.
- **DeepSeek:** padronização da documentação.
