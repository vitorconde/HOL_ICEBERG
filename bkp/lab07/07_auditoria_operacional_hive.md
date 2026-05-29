# 07 — Auditoria operacional com Iceberg no Hive

## Objetivo

Criar consultas úteis para auditoria e sustentação da tabela Iceberg.

## Últimas operações

```sql
SELECT
    committed_at,
    snapshot_id,
    operation
FROM iceberg_prod.governo_mg.beneficios.snapshots
ORDER BY committed_at DESC
LIMIT 10;
```

## Quantidade de arquivos e registros

```sql
SELECT
    COUNT(*) AS quantidade_arquivos,
    SUM(record_count) AS quantidade_registros,
    SUM(file_size_in_bytes) AS tamanho_total_bytes
FROM iceberg_prod.governo_mg.beneficios.files;
```

## Arquivos pequenos

```sql
SELECT
    file_path,
    record_count,
    file_size_in_bytes
FROM iceberg_prod.governo_mg.beneficios.files
WHERE file_size_in_bytes < 134217728
ORDER BY file_size_in_bytes ASC;
```

## Histórico da tabela

```sql
SELECT
    made_current_at,
    snapshot_id,
    parent_id,
    is_current_ancestor
FROM iceberg_prod.governo_mg.beneficios.history
ORDER BY made_current_at DESC;
```

## O que explicar

- Hive pode consultar metadados técnicos do Iceberg.
- A equipe consegue identificar operações recentes, arquivos e snapshots.
- Isso ajuda em auditoria e sustentação da plataforma.
