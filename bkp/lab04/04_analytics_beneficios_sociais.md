# 04 — Consultas analíticas para benefícios sociais

## Objetivo

Criar consultas de negócio para demonstrar consumo analítico via Impala.

## História de negócio

Gestores querem acompanhar volume de beneficiários, valor total pago, valor médio e status dos benefícios.

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
    TRUNC(data_pagamento, 'MONTH') AS mes_pagamento,
    COUNT(*) AS total_pagamentos,
    ROUND(SUM(valor_beneficio), 2) AS valor_total
FROM iceberg_prod.governo_mg.beneficios
GROUP BY TRUNC(data_pagamento, 'MONTH')
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

- Impala como camada de consumo BI/SQL.
- Iceberg como tabela única compartilhada.
- Consultas de negócio sem movimentação de dados.
