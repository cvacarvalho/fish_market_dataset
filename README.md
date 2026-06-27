
# 🐟 Fish Market SQL Analysis

Análise exploratória e estatística utilizando PostgreSQL, DBeaver e Power Query

## 📌 Sobre o Projeto

Este projeto tem como objetivo praticar análise de dados utilizando SQL em um ambiente realista. O dataset escolhido foi o **Fish Market Dataset (Kaggle)**, que contém medidas de diferentes espécies de peixes (peso, comprimento, altura).

O projeto envolve:

-   inspeção e validação dos dados no Power Query
    
-   criação de tabela no PostgreSQL
    
-   importação via DBeaver
    
-   consultas SQL exploratórias
    
-   análises estatísticas avançadas (correlação, métricas derivadas, z-score)
    
-   identificação de inconsistências e outliers
    
## 📂 Dataset

**Fonte:** Kaggle – Fish Market Dataset Contém informações sobre peso, comprimentos, altura e largura de peixes de diferentes espécies.

## 🛠️ Ferramentas Utilizadas

-   Excel (Power Query)
    
-   PostgreSQL
    
-   DBeaver
    
-   SQL
    

## 🔍 1. Inspeção Inicial (Power Query)

A inspeção inicial permitiu:

-   confirmar tipos de dados corretos
    
-   verificar ausência de valores nulos
   

## 2. Criação da Tabela no PostgreSQL
CREATE TABLE fish_market (
    id SERIAL PRIMARY KEY,
    species TEXT,
    weight NUMERIC,
    length1 NUMERIC,
    length2 NUMERIC,
    length3 NUMERIC,
    height NUMERIC,
    width NUMERIC
);

## 3. Importação dos Dados (DBeaver)

A importação foi realizada via:
-   botão direito na tabela → Import Data
-   seleção do arquivo CSV original
-   mapeamento automático das colunas
 
Validação após a importação:

sql

```
SELECT COUNT(*) FROM fish_market;
SELECT * FROM fish_market LIMIT 10;
```

## 📊 4. Análise Exploratória (SQL)

### Média de peso por espécie
sql

```
SELECT species, AVG(weight) AS avg_weight
FROM fish_market
GROUP BY species
ORDER BY avg_weight DESC;

```
<img width="447" height="505" alt="Print 1" src="https://github.com/user-attachments/assets/8f014e22-ab28-4930-a679-d217fde0e576" />

```

### Frequência de registros por espécie

sql

```
SELECT species, COUNT(*) AS total
FROM fish_market
GROUP BY species
ORDER BY total DESC;

```

### Top 10 peixes mais pesados

sql

```
SELECT *
FROM fish_market
ORDER BY weight DESC
LIMIT 10;

```

### Comprimento médio por espécie

sql

```
SELECT species, AVG(length1) AS avg_length
FROM fish_market
GROUP BY species
ORDER BY avg_length DESC;

```

## 📈 5. Análises Avançadas

### Correlação entre peso e comprimento

sql

```
SELECT 
    species,
    CORR(weight, length1) AS corr_weight_length
FROM fish_market
GROUP BY species
ORDER BY corr_weight_length DESC;

```

### Índice de volume aproximado

sql

```
SELECT 
    species,
    AVG(length1 * height * width) AS avg_volume_index,
    AVG(weight) AS avg_weight
FROM fish_market
GROUP BY species
ORDER BY avg_volume_index DESC;

```

### Detecção de outliers com z-score

sql

```
SELECT 
    species,
    weight,
    (weight - AVG(weight) OVER (PARTITION BY species)) /
    NULLIF(STDDEV(weight) OVER (PARTITION BY species), 0) AS z_score
FROM fish_market
ORDER BY ABS(
    (weight - AVG(weight) OVER (PARTITION BY species)) /
    NULLIF(STDDEV(weight) OVER (PARTITION BY species), 0)
) DESC
LIMIT 10;

```

## 🧠 6. Principais Insights

-   A maioria das espécies apresenta **correlação forte** entre peso e comprimento.
    
-   O índice de volume aproximado acompanha o peso médio, reforçando coerência dimensional.
    
-   O registro com **peso = 0,0** foi identificado como **outlier extremo**, indicando dado ausente mascarado como zero.
    
-   Funções janela e estatísticas permitiram identificar padrões e anomalias com precisão.
    

## 🏁 7. Conclusão

Este projeto demonstra:

-   domínio de ETL básico
    
-   criação e manipulação de tabelas no PostgreSQL
    
-   importação de dados via DBeaver
    
-   consultas SQL exploratórias e avançadas
    
-   aplicação de estatística (correlação, z-score)
    
-   interpretação analítica e identificação de inconsistências
