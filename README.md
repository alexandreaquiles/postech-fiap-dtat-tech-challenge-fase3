# TECH CHALLENGE - Fase 3 - Pós Tech Data Analytics - FIAP

O projeto consistia em atuar como Expert em Data Analytics para um grande hospital e entender como foi o comportamento da população na época da pandemia da COVID-19 e quais indicadores seriam importantes para o planejamento, caso haja um novo surto da doença.

Conforme especificações do projeto, buscamos a base de dados da Pesquisa Nacional por Amostra de Domicílios - [PNAD COVID-19](https://basedosdados.org/dataset/br-ibge-pnad-covid) do IBGE. Organizamos os dados a fim de atender o solicitado, conforme anexo 1, prezando pelas seguintes premissas:

1. Utilização de no máximo 20 questionamentos realizados na pesquisa;  
2. Utilizar 3 meses para construção da solução;  
3. Caracterização dos sintomas clínicos da população;  
4. Comportamento da população na época da COVID-19, e   
5. Características econômicas da Sociedade.

## Organização do Banco de Dados

Os dados da PNAD COVID-19 foram organizados em torno de três categorias principais: sintomas clínicos da população, comportamento durante a pandemia,e características econômicas. 

Utilizamos os dados dos meses de setembro, outubro e novembro de 2020, atendo-se às perguntas que permitiram entender o impacto da pandemia na saúde, comportamento e condições econômicas da população.

A estruturação do banco foi feita utilizando as seguintes etapas:

1. Colab  
  * Limpeza dos dados: Tratamento de valores nulos nas colunas relacionadas aos sintomas clínicos, condições econômicas e comportamento, substituindo-os por valores como "Não informado" ou "Não", conforme a lógica de interpretação dos dados,   
  * Matriz de correlação: Algumas variáveis categóricas foram codificadas para permitir criar uma matriz de correlação (usando o Cramér's V).  
  * Seleção de variáveis mais relevantes: Para garantir uma análise direcionada e evitar sobrecarga de dados, selecionamos as perguntas mais eficazes com menor quantidade de dados faltantes e maior relevância clínica, econômica e comportamental.  
  * Exportação dos dados em csv  
2. Power BI  
  * Importação dos dados em csv, e   
  * Criação de visualizações e filtros que permitiram uma segunda análise quanto aos grande números e a correlação das categorias selecionadas, conforme anexo 02\.

## Perguntas Selecionadas e Justificativas

As perguntas selecionadas focaram em três pilares:

- Sintomas Clínicos da População
- Comportamento da População
- Características Econômicas da Sociedade

### Sintomas Clínicos da População

1. **Febre**  
2. **Tosse**  
3. **Perda de cheiro e sabor**  
4. **Dificuldade de respirar**  
5. **Dor de cabeça**  
   **Por que essas perguntas?**: Os sintomas selecionados estão fortemente correlacionados entre si e representam os principais sintomas da COVID-19. São essenciais para determinar a gravidade da doença e orientar a necessidade de cuidados médicos imediatos.

### Comportamento da População

1. **Restringiu contato com outras pessoas?**  
2. **Estava em home office?**  
   **Por que essas perguntas?**: A compreensão do comportamento da população em termos de distanciamento social e trabalho remoto é fundamental para entender as medidas preventivas adotadas e o impacto sobre a disseminação do vírus.

### Características Econômicas da Sociedade

1. **Trabalhou ou fez bico?**  
2. **Estava afastado do trabalho?**  
3. **Recebeu auxílio emergencial?**  
4. **Contribuição ao INSS**  
   **Por que essas perguntas?**: As perguntas sobre a situação econômica da população nos fornecem uma visão sobre a vulnerabilidade social durante a pandemia. A relação entre trabalho informal, afastamento e o recebimento de auxílios governamentais reflete o impacto da crise sobre a subsistência e a estabilidade econômica.

## Análise: Principais Ações que o Hospital Deve Tomar

Com base nos dados analisados e nas correlações entre sintomas clínicos, comportamento e condições econômicas, o hospital deve se preparar para enfrentar um possível novo surto da COVID-19 por meio das seguintes ações:

* **Aumento da Capacidade de Internação e UTI**: Dada a correlação forte entre sintomas como febre, dificuldade respiratória e perda de olfato, o hospital deve garantir que haja leitos suficientes para pacientes com sintomas graves.  
* **Triagem Avançada de Sintomas**: A implementação de triagem automatizada para identificar múltiplos sintomas simultâneos (febre, tosse, etc.) permitirá o atendimento mais rápido de casos críticos e a separação dos pacientes com sintomas leves.  
* **Foco no Atendimento a Grupos Vulneráveis**: Pessoas que trabalham informalmente ou estão afastadas do trabalho devem ser priorizadas, pois possuem maior vulnerabilidade econômica e podem ter dificuldades em acessar o sistema de saúde.  
* **Educação e Prevenção**: Campanhas de conscientização sobre distanciamento social e práticas preventivas são fundamentais para reduzir a disseminação do vírus, mesmo com a fraca correlação observada entre sintomas e comportamento preventivo.  
* **Telemedicina**: Deve ser ampliado o uso de teleconsultas para monitorar sintomas leves e orientar a população sem sobrecarregar os recursos hospitalares.

## Anexo 01 - Dados do SQL

Os dados da pesquisa PNAD COVID-19 de três meses (setembro, outubro e novembro de 2020), foram estruturados em código SQL no BigQuery com base nas perguntas que possuíam dados mais eficazes e com menor quantidade de nulos.

Microdados da PNAD-COVID.

Website: [https://basedosdados.org/dataset/br-ibge-pnad-covid](https://basedosdados.org/dataset/br-ibge-pnad-covid)

Consulta (Query):

```sql
SELECT  

  a.ano,

  a.mes,

  PARSE\_DATE('%Y-%m-%d', CONCAT(CAST(ano AS STRING), '-', LPAD(CAST(mes AS STRING), 2, '0'), '-01')) AS data\_entrevista,

  \-- Características econômicas da sociedade

  a.sigla\_uf AS uf,

  b.valor AS sit\_domicilio,  \-- Situação do domicílio

  a.a002 AS idade,  \-- Mantém como numérico

  c.valor AS sexo,

  d.valor AS raca,

  e.valor AS escolaridade,

  n.valor AS aposentadoria\_pensao,  \-- Recebimento de aposentadoria ou pensão

  o.valor AS auxilio\_emergencial,   \-- Recebimento de auxílio emergencial

  p.valor AS seguro\_desemprego,     \-- Recebimento de seguro-desemprego

  q.valor AS trabalhou\_bico,        \-- Trabalhou ou fez bico

  r.valor AS afastado\_trabalho,     \-- Estava afastado do trabalho

  s.valor AS contribuicao\_inss,     \-- Contribuição para o INSS

  \-- Sintomas clínicos da população

  f.valor AS perda\_cheiro\_sabor,    \-- Perda de cheiro ou de sabor

  g.valor AS febre,                 \-- Febre

  h.valor AS tosse,                 \-- Tosse

  i.valor AS dif\_respirar,          \-- Dificuldade para respirar

  j.valor AS dor\_de\_cabeca,         \-- Dor de cabeça

  k.valor AS est\_de\_saude,          \-- Busca por estabelecimento de saúde

  l.valor AS internacao,            \-- Internação por COVID-19

 

  \-- Comportamento da população

  t.valor AS plano\_saude,           \-- Possui plano de saúde

  u.valor AS restringiu\_contato,    \-- Restringiu contato com outras pessoas

  v.valor AS home\_office            \-- Estava em home office

FROM basedosdados.br\_ibge\_pnad\_covid.microdados AS a

\-- JOINs para as variáveis de características econômicas

LEFT JOIN basedosdados.br\_ibge\_pnad\_covid.dicionario AS b

ON a.v1022 \= b.chave AND b.nome\_coluna \= 'v1022'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS c

ON a.a003 \= c.chave AND c.nome\_coluna \= 'a003'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS d

ON a.a004 \= d.chave AND d.nome\_coluna \= 'a004'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS e

ON a.a005 \= e.chave AND e.nome\_coluna \= 'a005'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS n

ON a.d0011 \= n.chave AND n.nome\_coluna \= 'd0011'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS o

ON a.d0051 \= o.chave AND o.nome\_coluna \= 'd0051'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS p

ON a.d0061 \= p.chave AND p.nome\_coluna \= 'd0061'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS q

ON a.c001 \= q.chave AND q.nome\_coluna \= 'c001'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS r

ON a.c002 \= r.chave AND r.nome\_coluna \= 'c002'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS s

ON a.c014 \= s.chave AND s.nome\_coluna \= 'c014'

\-- JOINs para as variáveis de sintomas clínicos

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS f

ON a.b00111 \= f.chave AND f.nome\_coluna \= 'b00111'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS g

ON a.b0011 \= g.chave AND g.nome\_coluna \= 'b0011'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS h

ON a.b0012 \= h.chave AND h.nome\_coluna \= 'b0012'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS i

ON a.b0014 \= i.chave AND i.nome\_coluna \= 'b0014'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS j

ON a.b0015 \= j.chave AND j.nome\_coluna \= 'b0015'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS k

ON a.b002 \= k.chave AND k.nome\_coluna \= 'b002'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS l

ON a.b005 \= l.chave AND l.nome\_coluna \= 'b005'

\-- JOINs para as variáveis de comportamento

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS t

ON a.b007 \= t.chave AND t.nome\_coluna \= 'b007'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS u

ON a.b011 \= u.chave AND u.nome\_coluna \= 'b011'

LEFT JOIN \`basedosdados.br\_ibge\_pnad\_covid.dicionario\` AS v

ON a.c013 \= v.chave AND v.nome\_coluna \= 'c013'

WHERE PARSE\_DATE('%Y-%m-%d', CONCAT(CAST(ano AS STRING), '-', LPAD(CAST(mes AS STRING), 2, '0'), '-01'))

      IN ('2020-11-01', '2020-10-01', '2020-09-01')

ORDER BY data\_entrevista;
```

## Anexo 02 - Visualizações em Power BI

![][imagens/powerbi-1.jpeg]

![][imagens/powerbi-2.jpeg]

![][imagens/powerbi-3.jpeg]

![][imagens/powerbi-4.jpeg]

![][imagens/powerbi-5.jpeg]
