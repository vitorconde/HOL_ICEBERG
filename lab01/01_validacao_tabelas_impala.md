# 01 — Validação das tabelas Iceberg no Hue / Impala

## Objetivo

Validar que as tabelas criadas pelos notebooks Spark/Livy3 estão visíveis para o Impala.

## Contexto da demo

Esta etapa mostra a interoperabilidade do Iceberg: os dados são criados com Spark, mas ficam imediatamente disponíveis para consulta SQL em outra engine.

## SQL para executar no Hue / Impala

```sql
-- Atualiza o cache de metadados do Impala.
INVALIDATE METADATA governo_mg.beneficios_<usuario>;
INVALIDATE METADATA governo_mg.beneficios_particionados_<usuario>;

-- Lista as tabelas do schema da demo.
SHOW TABLES IN governo_mg;
```

## Validação da tabela principal

```sql
DESCRIBE governo_mg.beneficios_<usuario>;
```

## Validação da tabela particionada

```sql
DESCRIBE governo_mg.beneficios_particionados_<usuario>;
```

## O que explicar

- O Spark criou as tabelas Iceberg.
- O Impala consulta as mesmas tabelas sem cópia de dados.
- O Iceberg funciona como camada de tabela aberta e compartilhada entre engines.
