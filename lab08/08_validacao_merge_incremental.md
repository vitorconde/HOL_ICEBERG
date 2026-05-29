# 08 — Validação de carga incremental feita via Spark MERGE

## Objetivo

Validar no Impala os efeitos do `MERGE INTO` executado no notebook Spark/Livy3.

## História de negócio

Um sistema de origem envia atualizações incrementais de benefícios. O Spark aplica o merge, e o Impala consome o resultado consolidado.

## Atualizar metadados

```sql
INVALIDATE METADATA governo_mg.beneficios_<usuario>;
INVALIDATE METADATA governo_mg.beneficios_staging_<usuario>;
```

## Ver registros da tabela staging

```sql
SELECT *
FROM governo_mg.beneficios_staging_<usuario>
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
FROM governo_mg.beneficios_<usuario>
WHERE id_cidadao IN (100, 200)
ORDER BY id_cidadao;
```

## Comparação analítica após merge

```sql
SELECT
    status,
    COUNT(*) AS total_registros,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM governo_mg.beneficios_<usuario>
GROUP BY status
ORDER BY status;
```

## O que explicar

- O Spark executa a carga incremental.
- O Impala lê o resultado final sem cópia.
- Esse é o padrão Lakehouse multi-engine: cada engine faz o que faz melhor.
