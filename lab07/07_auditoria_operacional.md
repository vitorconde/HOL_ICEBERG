# 07 — Auditoria operacional com Iceberg

## Objetivo

Criar consultas úteis para auditoria e sustentação da tabela Iceberg.

## História de negócio

A área de auditoria quer saber quando a tabela foi alterada, quais operações ocorreram e quais snapshots estão ativos.

## Últimas operações da tabela

```sql
SELECT
    committed_at,
    snapshot_id,
    operation
FROM governo_mg.beneficios_<usuario>.snapshots
ORDER BY committed_at DESC
LIMIT 10;
```

## Quantidade de arquivos e registros

```sql
SELECT
    COUNT(*) AS quantidade_arquivos,
    SUM(record_count) AS quantidade_registros,
    SUM(file_size_in_bytes) AS tamanho_total_bytes
FROM governo_mg.beneficios_<usuario>.files;
```

## Arquivos pequenos

```sql
SELECT
    file_path,
    record_count,
    file_size_in_bytes
FROM governo_mg.beneficios_<usuario>.files
WHERE file_size_in_bytes < 134217728
ORDER BY file_size_in_bytes ASC;
```

## Histórico de versões atuais e ancestrais

```sql
SELECT
    made_current_at,
    snapshot_id,
    parent_id,
    is_current_ancestor
FROM governo_mg.beneficios_<usuario>.history
ORDER BY made_current_at DESC;
```

## O que explicar

- Iceberg permite auditoria técnica sem ferramenta externa.
- A equipe consegue identificar crescimento de arquivos e operações recentes.
- Essas consultas ajudam em sustentação e governança operacional.
