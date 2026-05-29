# 11 — DML com Impala em Iceberg — opcional e dependente da versão

## Objetivo

Demonstrar, se suportado pelo ambiente, operações DML diretamente no Impala.

## Atenção

Execute esta etapa somente se o runtime do ambiente suportar escrita DML do Impala em tabelas Iceberg e se as propriedades da tabela estiverem compatíveis com o modo requerido.

Em alguns ambientes, recomenda-se manter DML transacional no Spark/Hive e usar Impala principalmente para leitura analítica.

## Verificar propriedades da tabela

```sql
SHOW CREATE TABLE iceberg_prod.governo_mg.beneficios;
```

```sql
SHOW TBLPROPERTIES iceberg_prod.governo_mg.beneficios;
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

## Validação após DML

```sql
SELECT *
FROM iceberg_prod.governo_mg.beneficios
ORDER BY id_cidadao;
```

## O que explicar

- Iceberg habilita operações transacionais em tabelas analíticas.
- O suporte exato no Impala depende da versão/configuração.
- Para uma demo estável, a escrita principal pode permanecer no Spark/Livy3, enquanto o Impala demonstra consumo, auditoria e analytics.
