# 09 — Consulta do CSV carregado para Iceberg

## Objetivo

Consultar no Impala a tabela criada a partir de um CSV local carregado para Iceberg.

## História de negócio

Um arquivo CSV recebido de uma área de negócio é incorporado ao Lakehouse e passa a ser consultável no Hue.

## Tabela esperada

```sql
iceberg_prod.governo_mg.beneficios_csv
```

## Atualizar metadados

```sql
INVALIDATE METADATA iceberg_prod.governo_mg.beneficios_csv;
```

## Consultar dados carregados

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios_csv
ORDER BY data_pagamento, id_cidadao;
```

## Validação de qualidade simples

```sql
SELECT
    COUNT(*) AS total_linhas,
    COUNT(DISTINCT id_cidadao) AS cidadaos_distintos,
    SUM(CASE WHEN data_pagamento IS NULL THEN 1 ELSE 0 END) AS datas_invalidas,
    SUM(CASE WHEN valor_beneficio IS NULL THEN 1 ELSE 0 END) AS valores_invalidos
FROM iceberg_prod.governo_mg.beneficios_csv;
```

## Agregação por status

```sql
SELECT
    status,
    COUNT(*) AS total,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM iceberg_prod.governo_mg.beneficios_csv
GROUP BY status
ORDER BY status;
```

## O que explicar

- Um CSV local foi promovido para uma tabela Iceberg.
- Depois da carga, o dado deixa de ser apenas um arquivo e vira uma tabela governável.
- O Impala passa a consultar a mesma tabela para analytics e BI.
