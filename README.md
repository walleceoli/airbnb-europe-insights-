# Airbnb-Europe-Insights 

## Descrição

Neste projeto, realizo uma **análise exploratória** de dados sobre Airbnbs em cidades europeias. A base de dados está distribuída em **múltiplos arquivos**, organizados por cidade e separados entre dias de semana e fins de semana.

O objetivo do projeto é extrair informações e insights que respondam a questões como:
* Variação de preços entre diferentes cidades
* Diferenças de preços entre dias de semana e fins de semana
* Relação entre preços e distância até capitais
* Impacto do tipo do imóvel, tamanho, número de quartos e capacidade de acomodação
* Identificação dos imóveis mais acessíveis e seus padrões
* Detecção de imóveis que fogem da média ou apresentam comportamentos atípicos

A partir dessas análises, busca-se obter uma visão macro de uma base de dados rica e realista, obtida a partir do **Kaggle**.

## Organização e Preparação dos Dados

Como mencionado, os dados estavam divididos em diferentes arquivos, todos no formato **CSV**. Como o objetivo é realizar uma análise **macro** desses conjuntos de dados, optei por unir todos os arquivos, mantendo separados apenas os dados de **dias de semana** e **fins de semana**, a fim de analisar e comparar as diferenças entre essas duas categorias.

Os arquivos foram consolidados utilizando **Python**, com as bibliotecas **Pandas**, **glob** e **os**. O pandas foi utilizado para importar e manipular os dados, enquanto glob e os foram usados para acessar automaticamente os arquivos a partir de uma pasta contendo todos os CSVs, evitando a necessidade de selecionar cada arquivo individualmente.

Logo em seguida, iniciei o *tratamento* dos dados. Foram verificadas as informações dos DataFrames, incluindo tanto os tipos das colunas quanto as *estatísticas descritivas*. Também foi checada a presença de valores *nulos* e *duplicados*.

Além disso, analisei os *valores únicos* de algumas colunas a fim de compreender melhor a base de dados. A partir dessa análise, foi possível identificar que as colunas representam:

* realSum → preço do Airbnb (variável alvo)

* origem → cidade

* room_type → tipo de acomodação

* room_shared, room_private → flags de tipo de acomodação

* person_capacity, bedrooms → tamanho do imóvel

* dist, metro_dist → localização

* attr_index, rest_index → atratividade urbana

* ratings → qualidade do imóvel

* host_is_superhost, multi, biz → perfil do anfitrião

Foi removida a coluna Unnamed: 0, que havia sido criada automaticamente durante o processo de união dos DataFrames ao salvar o índice gerado pelo pandas.

## Verificando  e corrigindo tipos de dados

Durante a verificação dos tipos de dados, foi identificado que algumas colunas estavam com tipos **inadequados**.

A coluna person_capacity, que indica a quantidade de pessoas suportada pelo imóvel, estava representada como **float**, o que não faz sentido, sendo então convertida para um tipo numérico **inteiro**.

A coluna cleanliness_rating apresenta apenas valores discretos entre 2.0 e 10.0. Por se tratar de valores sem casas decimais reais, optou-se por representá-la como número **inteiro**.

Já a coluna guest_satisfaction_overall representa uma pontuação agregada em uma escala de 0 a 100. Apesar de ser possível convertê-la para inteiro, ela foi mantida como variável numérica contínua float para preservar sua interpretação estatística.

## Analisando a coluna realSum

A análise teve início a partir de estatísticas descritivas, com foco inicial na base de dados referente aos **dias úteis**. Foram calculadas métricas como **média**, **mediana**, **desvio padrão** e **coeficiente de variação**.

A partir dessas métricas, foi possível identificar que algumas cidades apresentam um alto nível de **volatilidade** nos preços. Cidades como Atenas, Viena e Londres exibem valores elevados de coeficiente de variação — sendo Atenas o caso mais **extremo**, com um valor próximo de **2,5**. Esse resultado indica a presença de imóveis que fogem significativamente do padrão ou da média, criando **outliers** que distorcem a média em direção a imóveis de **alto valor**.

Para confirmar e aprofundar essa análise, foi calculado o **intervalo interquartil (IQR)**, uma vez que métricas baseadas no primeiro e terceiro quartis tendem a **reduzir a influência de valores extremos** que distorcem a média.

Com isso, foi possível confirmar que as cidades citadas anteriormente realmente apresentam **outliers** muito **acima da média**. Em Atenas, por exemplo, a mediana é de 127.715, enquanto um único imóvel apresenta valor superior a **17.500**, influenciando fortemente as métricas baseadas na média. Esse comportamento também é observado em **Viena, Londres e Paris**.

Em **contrapartida**, cidades como **Roma, Lisboa e Budapeste** apresentam distribuições de preços mais **homogêneas**, com médias mais representativas e **menor interferência de outliers**.

## Tecnologias Utilizadas

As seguintes tecnologias e bibliotecas foram utilizadas ao longo do projeto:

- **Python** → linguagem principal do projeto
- **pandas** → manipulação, limpeza e análise dos dados
- **numpy** → operações numéricas e suporte a cálculos estatísticos
- **matplotlib** → visualização de dados estática
- **plotly** → visualizações interativas
- **streamlit** → criação de dashboards interativos
- **os** → manipulação de diretórios e caminhos de arquivos
- **glob** → leitura automatizada de múltiplos arquivos CSV
