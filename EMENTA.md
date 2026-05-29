# Ementa — Demo Apache Iceberg no Cloudera Data Services

## Tema da Demonstração

Construção de um Lakehouse Governamental moderno para gestão de benefícios sociais utilizando Apache Iceberg no Cloudera Data Services.

---

## Objetivo

Demonstrar como o Apache Iceberg pode ser utilizado para modernizar ambientes analíticos e operacionais da PRODEMGE, oferecendo:

* armazenamento aberto e transacional;
* governança e auditoria;
* interoperabilidade entre múltiplas engines;
* evolução segura de schemas;
* recuperação operacional;
* alta performance analítica.

---

## Caso de Uso

A demo utiliza um cenário fictício de gestão de benefícios sociais do Governo de Minas Gerais, simulando:

* pagamentos mensais;
* correções cadastrais;
* atualizações incrementais;
* auditoria histórica;
* rollback operacional;
* consumo multi-engine por Spark, Hive e Impala.

---

## Conhecimento Prévio Recomendado

A demonstração foi construída para profissionais de dados, arquitetura e governança, mas não exige experiência prévia com Apache Iceberg.

Conhecimentos recomendados:

* Conceitos básicos de Data Lake e Data Warehouse;
* Noções de SQL;
* Conhecimento básico de Spark, Hive ou Impala;
* Familiaridade com ambientes analíticos corporativos;
* Entendimento geral sobre governança de dados.

Não é necessário conhecimento prévio em:

* Apache Iceberg;
* Time Travel;
* Snapshots;
* Metadata Tables;
* Lakehouse;
* Livy;
* Cloudera Data Services.

---

## Conhecimentos Adquiridos ao Final da Demonstração

Ao final desta demonstração, os participantes serão capazes de compreender:

### Fundamentos do Apache Iceberg

* O que é Apache Iceberg;
* Diferenças entre Data Lake tradicional e Lakehouse;
* Benefícios do uso de tabelas abertas e transacionais;
* Arquitetura de metadados do Iceberg.

### Modelagem e Armazenamento

* Como criar tabelas Iceberg;
* Como armazenar dados de forma otimizada;
* Como utilizar Hidden Partitioning;
* Como evitar problemas comuns de particionamento tradicional.

### Operações Transacionais

* Como realizar UPDATE em tabelas analíticas;
* Como executar DELETE sem recriação de datasets;
* Como utilizar MERGE INTO para cargas incrementais;
* Como garantir consistência através de transações ACID.

### Evolução de Dados

* Como evoluir schemas sem recriar tabelas;
* Como adicionar novas colunas de negócio;
* Como manter compatibilidade entre versões de dados.

### Auditoria e Rastreabilidade

* Como utilizar Snapshots;
* Como consultar versões históricas utilizando Time Travel;
* Como investigar alterações realizadas na tabela;
* Como rastrear operações através dos metadados do Iceberg.

### Recuperação Operacional

* Como identificar cargas incorretas;
* Como executar rollback para snapshots anteriores;
* Como reduzir riscos operacionais em ambientes produtivos.

### Performance e Manutenção

* Como identificar arquivos pequenos;
* Como realizar compactação de dados;
* Como gerenciar snapshots;
* Como manter tabelas Iceberg saudáveis ao longo do tempo.

### Interoperabilidade

* Como compartilhar dados entre Spark, Hive e Impala;
* Como eliminar cópias redundantes de datasets;
* Como habilitar múltiplas ferramentas sobre o mesmo conjunto de dados.

### Governança de Dados

* Como integrar Iceberg com governança corporativa;
* Como aplicar segurança através do Ranger;
* Como utilizar Catálogo de Dados e Lineage;
* Como preparar o ambiente para iniciativas de IA e Analytics Avançado.

---

## Resultado Esperado

Ao final da demonstração, os participantes terão uma visão completa de como construir e operar um Lakehouse moderno utilizando Apache Iceberg no Cloudera Data Services, compreendendo desde conceitos fundamentais até recursos avançados de governança, auditoria, recuperação e interoperabilidade.

---

## Funcionalidades do Iceberg exploradas

### Fundamentos

* `CREATE TABLE USING ICEBERG`
* `INSERT INTO`
* `SELECT`

### Particionamento Inteligente

* Hidden Partitioning
* Partition Pruning

### Evolução de Dados

* Schema Evolution
* `ADD COLUMN`
* Compatibilidade retroativa

### Operações Transacionais

* `UPDATE`
* `DELETE`
* `MERGE INTO`

### Auditoria e Governança

* Snapshots
* Time Travel
* Metadata Tables
* Histórico de commits

### Recuperação Operacional

* Rollback de snapshots
* Recuperação de cargas indevidas

### Performance e Manutenção

* Rewrite Data Files
* Compactação
* Gestão de arquivos pequenos
* Snapshot Management

### Interoperabilidade

* Spark SQL
* Hive
* Impala
* Multi-engine Lakehouse

### Governança

* Ranger
* Data Catalog
* Lineage
* Iceberg REST Catalog

---

## Tecnologias Utilizadas

* Cloudera Data Services
* Apache Iceberg
* Apache Spark
* Livy3
* Hive
* Impala
* Hue
* Apache Ranger
* Cloudera Data Catalog

---

## Benefícios Esperados

* redução de complexidade operacional;
* maior governança e rastreabilidade;
* recuperação rápida de falhas;
* interoperabilidade entre ferramentas;
* redução de cópias de dados;
* modernização do Data Lake para arquitetura Lakehouse.
