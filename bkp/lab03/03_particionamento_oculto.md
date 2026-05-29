# 03 — Particionamento oculto no Impala

## Objetivo

Mostrar que o usuário consulta uma coluna de negócio, enquanto o Iceberg utiliza particionamento oculto internamente.

## História de negócio

Pagamentos de benefícios são analisados por mês, mas o analista não precisa conhecer colunas técnicas de partição.

## Tabela criada no Spark

```sql
CREATE TABLE iceberg_prod.governo_mg.beneficios_particionados (
    id_cidadao BIGINT,
    nome STRING,
    valor_beneficio DOUBLE,
    data_pagamento DATE,
    status STRING
)
USING iceberg
PARTITIONED BY (months(data_pagamento));
```

## Query no Hue / Impala

```sql
INVALIDATE METADATA iceberg_prod.governo_mg.beneficios_particionados;

SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios_particionados
WHERE data_pagamento >= DATE '2025-02-01'
ORDER BY data_pagamento;
```

## Explain da consulta

```sql
EXPLAIN
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios_particionados
WHERE data_pagamento >= DATE '2025-02-01';
```

## O que explicar

- No particionamento tradicional, muitas vezes o usuário precisava filtrar uma coluna técnica.
- No Iceberg, o particionamento pode ser definido por uma transformação, como `months(data_pagamento)`.
- O usuário continua filtrando por `data_pagamento`, que é uma coluna de negócio.
- O Iceberg usa metadados para reduzir a leitura de arquivos.
