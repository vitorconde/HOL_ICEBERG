# 08 — Validação de carga incremental feita via Spark MERGE

## Objetivo

Validar no Impala os efeitos do `MERGE INTO` executado no notebook Spark/Livy3.

## História de negócio

Um sistema de origem envia atualizações incrementais de benefícios. O Spark aplica o merge, e o Impala consome o resultado consolidado.

## Atualizar metadados

```sql
INVALIDATE METADATA iceberg_prod.governo_mg.beneficios;
INVALIDATE METADATA iceberg_prod.governo_mg.beneficios_staging;
```

## Ver registros da tabela staging

```sql
SELECT *
FROM iceberg_prod.governo_mg.beneficios_staging
ORDER BY id_cidadao;
```

## Ver resultado consolidado

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios
WHERE id_cidadao IN (100, 200)
ORDER BY id_cidadao;
```

## Comparação analítica após merge

```sql
SELECT
    status,
    COUNT(*) AS total_registros,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM iceberg_prod.governo_mg.beneficios
GROUP BY status
ORDER BY status;
```

## O que explicar

- O Spark executa a carga incremental.
- O Impala lê o resultado final sem cópia.
- Esse é o padrão Lakehouse multi-engine: cada engine faz o que faz melhor.
