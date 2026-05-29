# 06 — Snapshots, History e Time Travel no Hive

## Objetivo

Mostrar como consultar versões históricas da tabela usando Hive.

## História de negócio

Uma auditoria solicita: “Como estava a base de benefícios antes da última carga?”

## Passo 1 — Identificar snapshots

```sql
SELECT
    committed_at,
    snapshot_id,
    operation
FROM governo_mg.beneficios_<usuario>.snapshots
ORDER BY committed_at DESC;
```

Copie um `snapshot_id` retornado pela consulta.

## Passo 2 — Time Travel por snapshot

Substitua `<SNAPSHOT_ID>` pelo valor real:

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM governo_mg.beneficios_<usuario>
FOR SYSTEM_VERSION AS OF <SNAPSHOT_ID>
ORDER BY id_cidadao;
```

## Passo 3 — Time Travel por timestamp

Ajuste o timestamp conforme os valores de `committed_at`:

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM governo_mg.beneficios_<usuario>
FOR SYSTEM_TIME AS OF '2025-01-01 00:00:00'
ORDER BY id_cidadao;
```

## Alternativa se a sintaxe acima não estiver habilitada

Se o Hive do ambiente não aceitar `FOR SYSTEM_VERSION AS OF`, faça o Time Travel via Spark/Livy3 e use Hive para consultar as metadata tables:

```sql
SELECT
    committed_at,
    snapshot_id,
    operation
FROM governo_mg.beneficios_<usuario>.snapshots
ORDER BY committed_at DESC;
```

## O que explicar

- Cada alteração na tabela gera snapshots.
- Time Travel permite consultar estados anteriores da tabela.
- Isso reduz dependência de backups para auditoria analítica.
