# 05 — Metadata Tables Iceberg no Hive

## Objetivo

Explorar metadados nativos do Iceberg diretamente pelo Hive.

## História de negócio

A equipe técnica e de governança precisa auditar arquivos, snapshots, histórico de alterações e estrutura física da tabela.

## Consultar snapshots

```sql
SELECT
    committed_at,
    snapshot_id,
    parent_id,
    operation
FROM governo_mg.beneficios_<usuario>.snapshots
ORDER BY committed_at DESC;
```

## Consultar histórico

```sql
SELECT
    made_current_at,
    snapshot_id,
    parent_id,
    is_current_ancestor
FROM governo_mg.beneficios_<usuario>.history
ORDER BY made_current_at DESC;
```

## Consultar arquivos físicos

```sql
SELECT
    file_path,
    file_format,
    record_count,
    file_size_in_bytes
FROM governo_mg.beneficios_<usuario>.files;
```

## Consultar manifests

```sql
SELECT *
FROM governo_mg.beneficios_<usuario>.manifests;
```

## O que explicar

- Iceberg mantém metadados de tabela, não apenas arquivos Parquet.
- Esses metadados ajudam em auditoria, troubleshooting e performance.
- É possível investigar a tabela diretamente com SQL.
