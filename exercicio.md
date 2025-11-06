# 🧊 Exercício Prático: Pipeline Completo com Apache Iceberg

## 🎯 Objetivo

Desenvolver um pipeline completo de dados utilizando **Apache Iceberg**, aplicando todos os conceitos aprendidos na aula: versionamento, particionamento, schema evolution, compactação e integração com bancos relacionais.

---

## 📋 Cenário Empresarial

Você é um **Engenheiro de Dados** em uma empresa de e-commerce que precisa implementar um data lake moderno usando Apache Iceberg. O sistema deve:

- Processar dados de vendas de múltiplas fontes
- Manter histórico completo com versionamento
- Permitir evolução de schema sem downtime
- Integrar dados transacionais (PostgreSQL) com dados analíticos (Iceberg)
- Garantir performance através de particionamento e compactação

---

## 🚀 **PARTE 1: Criação da Arquitetura Base **

### **Tarefa 1.1: Setup do Ambiente**
Crie um novo notebook chamado `exercicio_final.ipynb` e configure o ambiente Spark com Iceberg.

**Requisitos:**
- Configure SparkSession com nome "EcommerceDataLake"
- Habilite extensões Iceberg
- Configure catálogo hadoop_catalog
- Defina warehouse em `/home/tavares/warehouse`

### **Tarefa 1.2: Criação da Tabela Principal**
Crie uma tabela Iceberg chamada `vendas_ecommerce` com o seguinte schema:

```sql
CREATE TABLE hadoop_catalog.default.vendas_ecommerce (
    venda_id INT,
    produto_nome STRING,
    categoria STRING,
    quantidade INT,
    preco_unitario DOUBLE,
    data_venda DATE,
    cliente_id STRING,
    vendedor_id INT
)
USING iceberg
PARTITIONED BY (year(data_venda), categoria)
```

---

## 📊 **PARTE 2: Operações com Versionamento **

### **Tarefa 2.1: Inserção de Dados Históricos**
Insira dados de vendas de 2023:

```sql
INSERT INTO hadoop_catalog.default.vendas_ecommerce VALUES
(1, 'Notebook Dell', 'Eletrônicos', 2, 2500.00, DATE('2023-01-15'), 'CLI001', 101),
(2, 'Mouse Logitech', 'Eletrônicos', 5, 80.00, DATE('2023-01-16'), 'CLI002', 102),
(3, 'Mesa Escritório', 'Móveis', 1, 800.00, DATE('2023-02-10'), 'CLI003', 101),
(4, 'Cadeira Gamer', 'Móveis', 2, 600.00, DATE('2023-02-15'), 'CLI001', 103),
(5, 'Smartphone Samsung', 'Eletrônicos', 1, 1200.00, DATE('2023-03-20'), 'CLI004', 102)
```

### **Tarefa 2.2: Inserção de Dados de 2024**
Adicione vendas de 2024:

```sql
INSERT INTO hadoop_catalog.default.vendas_ecommerce VALUES
(6, 'Tablet iPad', 'Eletrônicos', 1, 3000.00, DATE('2024-01-10'), 'CLI002', 101),
(7, 'Sofá 3 Lugares', 'Móveis', 1, 1500.00, DATE('2024-01-20'), 'CLI005', 103),
(8, 'Monitor 4K', 'Eletrônicos', 2, 800.00, DATE('2024-02-05'), 'CLI003', 102)
```

### **Tarefa 2.3: Análise de Snapshots**
- Liste todos os snapshots criados
- Identifique quantos snapshots foram gerados
- Mostre o histórico da tabela

### **Tarefa 2.4: Time Travel**
- Consulte apenas os dados de 2023 usando o primeiro snapshot
- Compare com os dados atuais da tabela

---

## 🔄 **PARTE 3: Schema Evolution**

### **Tarefa 3.1: Evolução do Schema**
Evolua o schema da tabela adicionando as seguintes colunas:
- `desconto DOUBLE` (para armazenar percentual de desconto)
- `canal_venda STRING` (online, loja_fisica, telefone)

### **Tarefa 3.2: Inserção com Novo Schema**
Insira dados usando o schema evoluído:

```sql
INSERT INTO hadoop_catalog.default.vendas_ecommerce VALUES
(9, 'Headset Gamer', 'Eletrônicos', 3, 250.00, DATE('2024-03-15'), 'CLI006', 101, 10.0, 'online'),
(10, 'Mesa Centro', 'Móveis', 1, 400.00, DATE('2024-03-20'), 'CLI007', 102, 5.0, 'loja_fisica')
```

### **Tarefa 3.3: Verificação de Compatibilidade**
- Mostre que dados antigos têm valores NULL nas novas colunas
- Confirme que todas as consultas ainda funcionam

---

## 🔧 **PARTE 4: Operações ACID e Merge **

### **Tarefa 4.1: Simulação de Erro**
Faça uma atualização "problemática":

```sql
UPDATE hadoop_catalog.default.vendas_ecommerce 
SET preco_unitario = preco_unitario * 100 
WHERE categoria = 'Eletrônicos'
```

### **Tarefa 4.2: Rollback**
- Identifique o snapshot antes da atualização problemática
- Execute rollback para esse snapshot
- Verifique que os dados voltaram ao estado anterior

### **Tarefa 4.3: Operação MERGE**
Crie uma view temporária com atualizações e execute MERGE INTO:

```sql
CREATE OR REPLACE TEMPORARY VIEW vendas_updates AS
SELECT 1 as venda_id, 'Notebook Dell UPDATED' as produto_nome, 'Eletrônicos' as categoria, 
       2 as quantidade, 2600.00 as preco_unitario, DATE('2023-01-15') as data_venda, 
       'CLI001' as cliente_id, 101 as vendedor_id, 0.0 as desconto, 'online' as canal_venda
UNION ALL
SELECT 11 as venda_id, 'Teclado Mecânico' as produto_nome, 'Eletrônicos' as categoria,
       4 as quantidade, 300.00 as preco_unitario, DATE('2024-04-01') as data_venda,
       'CLI008' as cliente_id, 103 as vendedor_id, 15.0 as desconto, 'online' as canal_venda
```

Execute MERGE INTO para atualizar registro existente e inserir novo.

---

## 📈 **PARTE 5: Otimização e Análise **

### **Tarefa 5.1: Análise de Fragmentação**
- Use metadados para analisar quantos arquivos foram criados
- Identifique se há necessidade de compactação
- Mostre estatísticas de arquivos por partição

### **Tarefa 5.2: Compactação**
- Execute compactação usando `rewrite_data_files`
- Compare número de arquivos antes e depois
- Verifique que os dados permanecem íntegros

### **Tarefa 5.3: Análise de Performance**
- Demonstre partition pruning consultando apenas dados de 2024
- Mostre consulta otimizada por categoria
- Use metadados para mostrar eficiência das consultas

### **Tarefa 5.4: Limpeza de Snapshots**
- Execute `expire_snapshots` mantendo apenas os últimos 3 snapshots
- Verifique quantos arquivos foram removidos

---

## 🏆 **ENTREGA FINAL**

### **Relatório de Análise**
Crie uma célula markdown final com:

1. **Resumo Executivo**:
   - Quantos snapshots foram criados no total?
   - Qual foi a redução de arquivos após compactação?
   - Quantas partições foram criadas?

2. **Benefícios Observados**:
   - Liste 3 vantagens do Iceberg que você observou na prática
   - Compare com o que seria necessário usando Parquet tradicional

3. **Casos de Uso Identificados**:
   - Descreva 2 cenários empresariais onde este pipeline seria útil
   - Explique como o versionamento ajudaria em cada caso

### **Consultas de Validação**
Inclua estas consultas para validar seu trabalho:

```sql
-- 1. Total de registros por ano
SELECT year(data_venda) as ano, COUNT(*) as total_vendas 
FROM hadoop_catalog.default.vendas_ecommerce 
GROUP BY year(data_venda) 
ORDER BY ano;

-- 2. Vendas por categoria com desconto médio
SELECT categoria, 
       COUNT(*) as total_vendas,
       ROUND(AVG(COALESCE(desconto, 0.0)), 2) as desconto_medio,
       ROUND(SUM(preco_unitario * quantidade), 2) as receita_total
FROM hadoop_catalog.default.vendas_ecommerce 
GROUP BY categoria;

-- 3. Análise temporal de snapshots
SELECT operation, COUNT(*) as num_operacoes 
FROM hadoop_catalog.default.vendas_ecommerce.snapshots 
GROUP BY operation;
```

---

## 📝 **Critérios de Avaliação**

| Critério | Pontos | Descrição |
|----------|--------|-----------|
| **Configuração** | 25 | Setup correto do Spark e criação da tabela |
| **Versionamento** | 25 | Snapshots, time travel e rollbacks |
| **Schema Evolution** | 20 | Evolução sem downtime e compatibilidade |
| **ACID Operations** | 15 | UPDATE, MERGE INTO e transações |
| **Otimização** | 15 | Compactação, análise de metadados e performance |

### **Pontuação Extra (+10 pontos)**
- Implementar particionamento adicional por `canal_venda`
- Criar análise avançada usando múltiplas tabelas de metadados
- Demonstrar integração com dados do PostgreSQL (tabela customers)

---

## 🎓 **Dicas para Sucesso**

1. **Execute os notebooks em ordem** antes de começar o exercício
2. **Use apenas SQL puro** para evitar erros de serialização
3. **Documente cada etapa** com células markdown explicativas
4. **Verifique sempre** a integridade dos dados após cada operação
5. **Monitore snapshots** para entender o versionamento
6. **Aproveite os metadados** para análises avançadas

---

## 🏁 **Entrega**

Salve o notebook `exercicio_final.ipynb` com todas as tarefas completas e documentadas. O exercício deve demonstrar domínio prático dos conceitos fundamentais do Apache Iceberg aplicados em um cenário empresarial realista.

**Boa sorte! 🚀**