# 02 — Consulta básica da tabela de benefícios sociais no Hive

## Objetivo

Consultar a tabela principal da demo diretamente no Hue usando Hive.

## História de negócio

A PRODEMGE mantém uma base central de benefícios sociais, e analistas podem consultar pagamentos, status e valores usando SQL.

## SQL

```sql
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

- Leitura de tabela Iceberg via Hive.
- Dados gravados via Spark disponíveis para consulta no Hue.
- Separação entre processamento e consumo analítico.
