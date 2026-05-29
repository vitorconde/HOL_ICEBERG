# 02 — Consulta básica da tabela de benefícios sociais

## Objetivo

Consultar a tabela principal criada na demo com Spark/Livy3.

## História de negócio

A PRODEMGE possui uma base central de benefícios sociais. Analistas podem consultar pagamentos, status e valores diretamente no Hue usando Impala.

## SQL

```sql
INVALIDATE METADATA iceberg_prod.governo_mg.beneficios;

SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios
ORDER BY id_cidadao;
```

## Consulta com filtro de negócio

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios
WHERE status = 'ATIVO'
ORDER BY valor_beneficio DESC;
```

## O que demonstrar

- Consulta SQL tradicional sobre tabela Iceberg.
- Dados carregados via Spark disponíveis para consumo analítico.
- Separação entre camada de processamento e camada de consumo.
