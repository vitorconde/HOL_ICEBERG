# 01 — Validação das tabelas Iceberg no Hue / Hive

## Objetivo

Validar que as tabelas criadas pelos notebooks Spark/Livy3 estão visíveis no Hive via Hue.

## Contexto da demo

Esta etapa mostra que o Spark pode criar e modificar tabelas Iceberg, enquanto o Hive pode consultar a mesma estrutura sem cópia de dados.

## SQL para executar no Hue / Hive

```sql
SHOW DATABASES;
```

```sql
SHOW TABLES IN governo_mg;
```

## Validar tabela principal

```sql
DESCRIBE governo_mg.beneficios_<usuario>;
```

## Validar tabela particionada

```sql
DESCRIBE governo_mg.beneficios_particionados_<usuario>;
```

## Ver definição da tabela

```sql
SHOW CREATE TABLE governo_mg.beneficios_<usuario>;
```

## O que explicar

- O Spark criou a tabela Iceberg.
- O Hive enxerga a mesma tabela pelo catálogo/metastore.
- O Iceberg atua como camada aberta de tabela, permitindo consumo multi-engine.
