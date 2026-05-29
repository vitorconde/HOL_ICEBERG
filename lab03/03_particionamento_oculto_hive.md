# 03 — Particionamento oculto no Hive

## Objetivo

Demonstrar que o usuário consulta uma coluna de negócio, enquanto o Iceberg usa particionamento oculto internamente.

## História de negócio

Pagamentos de benefícios normalmente são analisados por mês. Com Iceberg, o usuário filtra por `data_pagamento`, sem precisar conhecer colunas técnicas de partição.

## Query no Hue / Hive

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM governo_mg.beneficios_particionados_<usuario>
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
FROM governo_mg.beneficios_particionados_<usuario>
WHERE data_pagamento >= DATE '2025-02-01';
```

## O que explicar

- A tabela foi criada com `PARTITIONED BY (months(data_pagamento))`.
- O usuário não precisa filtrar por uma coluna técnica como `mes`.
- O Iceberg usa metadados para otimizar a leitura.
- O modelo fica mais simples para analistas e mais eficiente para execução.
