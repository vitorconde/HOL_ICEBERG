# 05 — Metadata Tables Iceberg no Impala

## Objetivo

Explorar metadados nativos do Iceberg diretamente pelo Hue/Impala.

## História de negócio

A equipe de dados precisa entender arquivos, snapshots, histórico e estrutura operacional da tabela para auditoria e sustentação.

## Listar metadata tables disponíveis

```sql
SHOW METADATA TABLES IN governo_mg.beneficios_<usuario>;
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

## Consultar histórico da tabela

```sql
SELECT
    made_current_at,
    snapshot_id,
    parent_id,
    is_current_ancestor
FROM governo_mg.beneficios_<usuario>.history
ORDER BY made_current_at DESC;
```

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

## O que explicar

- Iceberg não é apenas um formato de arquivo.
- Iceberg mantém uma camada rica de metadados.
- Esses metadados permitem auditoria, troubleshooting, performance e rastreabilidade.
