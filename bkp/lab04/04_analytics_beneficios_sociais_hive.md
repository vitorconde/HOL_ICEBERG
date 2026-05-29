# 04 — Consultas analíticas para benefícios sociais no Hive

## Objetivo

Criar consultas de negócio para demonstrar consumo analítico no Hue/Hive.

## Total por status

```sql
SELECT
    status,
    COUNT(*) AS total_beneficiarios,
    ROUND(SUM(valor_beneficio), 2) AS valor_total,
    ROUND(AVG(valor_beneficio), 2) AS valor_medio
FROM iceberg_prod.governo_mg.beneficios
GROUP BY status
ORDER BY status;
```

## Pagamentos por mês

```sql
SELECT
    trunc(data_pagamento, 'MM') AS mes_pagamento,
    COUNT(*) AS total_pagamentos,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM iceberg_prod.governo_mg.beneficios
GROUP BY trunc(data_pagamento, 'MM')
ORDER BY mes_pagamento;
```

## Maiores benefícios

```sql
SELECT
    id_cidadao,
    nome,
    valor_beneficio,
    data_pagamento,
    status
FROM iceberg_prod.governo_mg.beneficios
ORDER BY valor_beneficio DESC
LIMIT 10;
```

## O que demonstrar

- Hive como camada SQL de exploração.
- Iceberg como tabela única compartilhada.
- Consultas de negócio sem duplicação de dados.
