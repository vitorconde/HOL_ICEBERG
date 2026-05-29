# 06 — Snapshots, History e Time Travel

## Objetivo

Mostrar como consultar versões históricas da tabela usando Impala.

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

Copie um `snapshot_id` retornado pela query.

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
FOR SYSTEM_TIME AS OF TIMESTAMP '2025-01-01 00:00:00'
ORDER BY id_cidadao;
```

## O que explicar

- Cada operação na tabela cria um novo snapshot.
- Time Travel permite consultar o estado anterior sem restaurar backup.
- Esse recurso é muito forte para auditoria, investigação e compliance.
