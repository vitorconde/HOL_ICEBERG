# 09 — Consulta do CSV carregado para Iceberg no Hive

## Objetivo

Consultar no Hive a tabela criada a partir de um CSV local carregado para Iceberg.

## Tabela esperada

```sql
governo_mg.beneficios_csv_<usuario>
```

## Consultar dados carregados

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM governo_mg.beneficios_csv_<usuario>
ORDER BY data_pagamento, id_cidadao;
```

## Validação de qualidade simples

```sql
SELECT
    COUNT(*) AS total_linhas,
    COUNT(DISTINCT id_cidadao) AS cidadaos_distintos,
    SUM(CASE WHEN data_pagamento IS NULL THEN 1 ELSE 0 END) AS datas_invalidas,
    SUM(CASE WHEN valor_beneficio IS NULL THEN 1 ELSE 0 END) AS valores_invalidos
FROM governo_mg.beneficios_csv_<usuario>;
```

## Agregação por status

```sql
SELECT
    status,
    COUNT(*) AS total,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM governo_mg.beneficios_csv_<usuario>
GROUP BY status
ORDER BY status;
```

## O que explicar

- Um CSV local foi promovido para tabela Iceberg.
- O dado deixa de ser apenas um arquivo e passa a ser uma tabela governável.
- O Hive pode consultar essa tabela diretamente pelo Hue.
