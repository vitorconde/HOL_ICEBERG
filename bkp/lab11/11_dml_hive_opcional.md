# 11 — DML com Hive em Iceberg — opcional

## Objetivo

Demonstrar, se suportado no ambiente, operações DML diretamente no Hive.

## Atenção

Execute esta etapa apenas se o runtime do ambiente suportar escrita DML do Hive em tabelas Iceberg.

Para uma demo estável, mantenha a escrita principal em Spark/Livy3 e use Hive para consulta, auditoria e exploração.

## Verificar definição da tabela

```sql
SHOW CREATE TABLE iceberg_prod.governo_mg.beneficios;
```

## Exemplo UPDATE

```sql
UPDATE iceberg_prod.governo_mg.beneficios
SET valor_beneficio = 777.77
WHERE id_cidadao = 1;
```

## Exemplo DELETE

```sql
DELETE FROM iceberg_prod.governo_mg.beneficios
WHERE status = 'SUSPENSO';
```

## Exemplo MERGE

```sql
MERGE INTO iceberg_prod.governo_mg.beneficios t
USING iceberg_prod.governo_mg.beneficios_staging s
ON t.id_cidadao = s.id_cidadao
WHEN MATCHED THEN UPDATE SET
    nome = s.nome,
    valor_beneficio = s.valor_beneficio,
    data_pagamento = s.data_pagamento,
    status = s.status
WHEN NOT MATCHED THEN INSERT VALUES
    (s.id_cidadao, s.nome, s.valor_beneficio, s.data_pagamento, s.status);
```

## Validação

```sql
SELECT *
FROM iceberg_prod.governo_mg.beneficios
ORDER BY id_cidadao;
```

## O que explicar

- Iceberg permite operações transacionais em tabelas analíticas.
- O suporte de DML no Hive pode depender da versão/configuração.
- Spark/Livy3 permanece como caminho mais seguro para escrita na demo.
