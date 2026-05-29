# 11 — DML com Impala em Iceberg — opcional e dependente da versão

## Objetivo

Demonstrar, se suportado pelo ambiente, operações DML diretamente no Impala.

## Atenção

Execute esta etapa somente se o runtime do ambiente suportar escrita DML do Impala em tabelas Iceberg e se as propriedades da tabela estiverem compatíveis com o modo requerido.

Em alguns ambientes, recomenda-se manter DML transacional no Spark/Hive e usar Impala principalmente para leitura analítica.

## Verificar propriedades da tabela

```sql
SHOW CREATE TABLE governo_mg.beneficios_<usuario>;
```

```sql
SHOW TBLPROPERTIES governo_mg.beneficios_<usuario>;
```

## Exemplo UPDATE

```sql
UPDATE governo_mg.beneficios_<usuario>
SET valor_beneficio = 777.77
WHERE id_cidadao = 1;
```

## Exemplo DELETE

```sql
DELETE FROM governo_mg.beneficios_<usuario>
WHERE status = 'SUSPENSO';
```

## Exemplo MERGE

```sql
MERGE INTO governo_mg.beneficios_<usuario> t
USING governo_mg.beneficios_staging_<usuario> s
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
FROM governo_mg.beneficios_<usuario>
ORDER BY id_cidadao;
```

## O que explicar

- Iceberg habilita operações transacionais em tabelas analíticas.
- O suporte exato no Impala depende da versão/configuração.
- Para uma demo estável, a escrita principal pode permanecer no Spark/Livy3, enquanto o Impala demonstra consumo, auditoria e analytics.
