# 🧊 Apache Iceberg com PySpark: Data Lakes Modernos e Governança de Dados

Este repositório foi desenvolvido para a disciplina **Data Collection** do MBA em Engenharia de Dados, focando no **Apache Iceberg** - o formato de tabela open-source que revoluciona o gerenciamento de dados em data lakes.

## 🎯 Objetivos da Aula

Demonstrar na prática os recursos avançados do Apache Iceberg através de exemplos hands-on:

- **ACID Transactions**: Garantia de consistência em operações
- **Schema Evolution**: Evolução de esquema sem downtime
- **Time Travel**: Consultas históricas e rollbacks
- **Partition Evolution**: Mudança de estratégias de particionamento
- **Compactação**: Otimização de performance e armazenamento
- **Metadados Ricos**: Governança e monitoramento avançado

## 📚 Conteúdo Programático

### 🔧 **Módulo 1: Fundamentos**
- **001 - Snapshots e TimeTravel**: Versionamento e consultas históricas
- **002 - Particionamento de Dados**: Estratégias de particionamento otimizado
- **003 - Rollbacks**: Reversão de operações e recuperação de dados

### 🚀 **Módulo 2: Operações Avançadas**
- **004 - Incorporando Dados Existentes**: Migração de Parquet para Iceberg
- **005 - Merge com Banco Relacional**: Integração PostgreSQL + Iceberg
- **006 - Evolução de Schema**: Mudanças de estrutura sem downtime

### ⚡ **Módulo 3: Otimização e Governança**
- **007 - Compactação de Dados**: Otimização de arquivos pequenos
- **008 - Uso de Metadados e Catálogo**: Governança e monitoramento

## 🏗️ Arquitetura do Ambiente

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Jupyter Lab   │    │   Apache Spark   │    │  PostgreSQL     │
│   (Port 8888)   │◄──►│   + Iceberg      │◄──►│  (Port 2001)    │
│                 │    │   (Port 4040)    │    │  Northwind DB   │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────────────┐
                    │   Docker Network    │
                    │  (172.16.240.0/24)  │
                    └─────────────────────┘
```

## 📂 Estrutura do Projeto

```
dataqualitySpark/
├── 📁 notebooks/           # Notebooks da aula
│   ├── 001 - Snapshots e TimeTravel.ipynb
│   ├── 002 - ParticaoDados.ipynb
│   ├── 003 - Rollbacks.ipynb
│   ├── 004 - IncorporandoDadosExistentes.ipynb
│   ├── 005 - FazendoMergeBancoRelacional.ipynb
│   ├── 006 - EvolucaoSchema.ipynb
│   ├── 007 - CompactacaoDados.ipynb
│   ├── 008 - UsoMetadadosCatalgo.ipynb
│   ├── data/               # Dados de exemplo
│   └── spark_config.py     # Configurações do Spark
├── 📁 data/               # Datasets
│   └── logistica_raw.csv  # Dataset para exercícios
├── 📁 db/                 # Banco de dados
│   └── northwind.sql      # Schema PostgreSQL
├── 🐳 docker-compose.yml  # Orquestração dos serviços
├── 🐳 Dockerfile          # Imagem customizada
├── 📋 exercicio.md        # Exercício prático
└── 📖 readme.md           # Este arquivo
```

## 🚀 Setup e Execução

### Pré-requisitos
- [Docker](https://docs.docker.com/get-docker/) 20.10+
- [Docker Compose](https://docs.docker.com/compose/install/) 2.0+
- 8GB RAM disponível
- 10GB espaço em disco

### 🔥 Início Rápido

1. **Clone o repositório**:
   ```bash
   git clone https://github.com/AleTavares/dataqualitySpark.git
   cd dataqualitySpark
   ```

2. **Inicie o ambiente**:
   ```bash
   docker-compose up -d --build
   ```

3. **Acesse o Jupyter Lab**:
   ```
   http://localhost:8888
   Token: tavares1234
   ```

4. **Acesse o Spark UI** (opcional):
   ```
   http://localhost:4040
   ```

5. **PostgreSQL** (para exercícios de integração):
   ```
   Host: localhost
   Port: 2001
   Database: northwind
   User: postgres
   Password: postgres
   ```

### 🛑 Parar o ambiente
```bash
docker-compose down
```

## 🎓 Roteiro de Estudos

### **Parte 1: Conceitos Fundamentais (45 min)**
1. Execute `001 - Snapshots e TimeTravel.ipynb`
   - Entenda o conceito de snapshots
   - Pratique consultas time travel
   
2. Execute `002 - ParticaoDados.ipynb`
   - Aprenda estratégias de particionamento
   - Compare performance com/sem partições

3. Execute `003 - Rollbacks.ipynb`
   - Pratique operações de rollback
   - Entenda recuperação de dados

### **Parte 2: Operações Avançadas (60 min)**
4. Execute `004 - IncorporandoDadosExistentes.ipynb`
   - Migre dados Parquet para Iceberg
   - Compare funcionalidades

5. Execute `005 - FazendoMergeBancoRelacional.ipynb`
   - Integre PostgreSQL com Iceberg
   - Pratique operações MERGE

6. Execute `006 - EvolucaoSchema.ipynb`
   - Evolua schemas sem downtime
   - Teste compatibilidade retroativa

### **Parte 3: Otimização e Governança (45 min)**
7. Execute `007 - CompactacaoDados.ipynb`
   - Otimize arquivos pequenos
   - Monitore performance

8. Execute `008 - UsoMetadadosCatalgo.ipynb`
   - Explore metadados ricos
   - Implemente governança

### **Parte 4: Exercício Prático (30 min)**
9. Complete o `exercicio.md`
   - Aplique todos os conceitos aprendidos
   - Desenvolva pipeline completo

## 🔧 Tecnologias Utilizadas

| Tecnologia | Versão | Propósito |
|------------|--------|-----------|
| **Apache Spark** | 3.3.0 | Engine de processamento |
| **Apache Iceberg** | 1.6.1 | Formato de tabela |
| **PostgreSQL** | 14.19 | Banco relacional |
| **Python** | 3.11 | Linguagem principal |
| **Jupyter Lab** | Latest | Ambiente de desenvolvimento |
| **Docker** | 20.10+ | Containerização |

## 🎯 Diferenciais do Apache Iceberg

### ✅ **Vantagens sobre Formatos Tradicionais**

| Recurso | Parquet/ORC | **Apache Iceberg** |
|---------|-------------|-------------------|
| ACID Transactions | ❌ | ✅ |
| Schema Evolution | ❌ | ✅ |
| Time Travel | ❌ | ✅ |
| Rollbacks | ❌ | ✅ |
| Partition Evolution | ❌ | ✅ |
| Hidden Partitioning | ❌ | ✅ |
| Metadados Ricos | ❌ | ✅ |

### 🚀 **Casos de Uso Empresariais**
- **Data Warehousing**: Substituição de soluções proprietárias
- **Data Lakes**: Governança e qualidade de dados
- **Analytics**: Consultas históricas e auditoria
- **Machine Learning**: Datasets versionados e reproduzíveis
- **Compliance**: Rastreabilidade e auditoria completa

## 🤝 Contribuições

Este material foi desenvolvido para fins educacionais. Sugestões e melhorias são bem-vindas através de issues e pull requests.

## 📄 Licença

Este projeto está sob a licença MIT. Veja o arquivo [LICENSE](LICENSE) para mais detalhes.

## 📞 Suporte

Para dúvidas sobre o conteúdo da aula:
- Abra uma [issue](https://github.com/AleTavares/dataqualitySpark/issues)
- Entre em contato durante a aula

---

**🎓 MBA Engenharia de Dados - Data Collection**  
*Transformando dados em valor através do Apache Iceberg*