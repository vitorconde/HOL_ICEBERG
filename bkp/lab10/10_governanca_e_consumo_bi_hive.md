# 10 — Governança e consumo BI com Hive

## Objetivo

Demonstrar consultas que poderiam alimentar relatórios ou dashboards de gestão pública.

## Indicadores gerais

```sql
SELECT
    COUNT(*) AS total_beneficiarios,
    ROUND(SUM(valor_beneficio), 2) AS valor_total_pago,
    ROUND(AVG(valor_beneficio), 2) AS valor_medio,
    MIN(data_pagamento) AS primeira_data_pagamento,
    MAX(data_pagamento) AS ultima_data_pagamento
FROM iceberg_prod.governo_mg.beneficios;
```

## Distribuição por faixa de valor

```sql
SELECT
    CASE
        WHEN valor_beneficio < 300 THEN 'Abaixo de 300'
        WHEN valor_beneficio >= 300 AND valor_beneficio < 600 THEN '300 a 599'
        WHEN valor_beneficio >= 600 AND valor_beneficio < 1000 THEN '600 a 999'
        ELSE '1000 ou mais'
    END AS faixa_valor,
    COUNT(*) AS total_beneficiarios,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM iceberg_prod.governo_mg.beneficios
GROUP BY
    CASE
        WHEN valor_beneficio < 300 THEN 'Abaixo de 300'
        WHEN valor_beneficio >= 300 AND valor_beneficio < 600 THEN '300 a 599'
        WHEN valor_beneficio >= 600 AND valor_beneficio < 1000 THEN '600 a 999'
        ELSE '1000 ou mais'
    END
ORDER BY faixa_valor;
```

## Visão operacional

```sql
SELECT
    status,
    COUNT(*) AS total,
    ROUND(SUM(valor_beneficio), 2) AS valor_total,
    ROUND(AVG(valor_beneficio), 2) AS valor_medio
FROM iceberg_prod.governo_mg.beneficios
GROUP BY status
ORDER BY status;
```

## O que explicar

- Hive pode ser usado como camada SQL para relatórios.
- Ranger pode controlar acesso a databases, tabelas e colunas.
- Iceberg mantém histórico e metadados para auditoria.
- A tabela única reduz cópias redundantes de dados.
