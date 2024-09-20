### Aula 1: **Gerenciamento de Clusters com Ferramentas do Debian**

- **Ferramentas extras do Debian**: Sistemas derivados do Debian (como Ubuntu) oferecem ferramentas que facilitam o gerenciamento de clusters PostgreSQL.
  
#### Comandos Principais:
1. **Listar Clusters**:
   ```bash
   pg_lsclusters
   ```
   **Exemplo de Saída**:
   ```
   Ver  Cluster  Port Status Owner    Data directory               Log file
   9.5  main     5432 online postgres /var/lib/postgresql/9.5/main /var/log/postgresql/postgresql-9.5-main.log
   ```

2. **Criar um Novo Cluster** (versão 9.5, nome "teste"):
   ```bash
   pg_createcluster 9.5 teste
   ```
   Isso criará um novo cluster PostgreSQL rodando na versão 9.5, com o nome "teste".

3. **Verificar o Estado dos Clusters**:
   ```bash
   pg_lsclusters
   ```
   Após criar um novo cluster, o comando mostrará a lista atualizada de clusters, como:
   ```
   Ver  Cluster  Port Status Owner    Data directory               Log file
   9.5  main     5432 online postgres /var/lib/postgresql/9.5/main /var/log/postgresql/postgresql-9.5-main.log
   9.5  teste    5433 down   postgres /var/lib/postgresql/9.5/teste /var/log/postgresql/postgresql-9.5-teste.log
   ```

4. **Iniciar um Cluster** (versão 9.5, nome "teste"):
   ```bash
   pg_ctlcluster 9.5 teste start
   ```

5. **Parar um Cluster**:
   ```bash
   pg_ctlcluster 9.5 teste stop
   ```

6. **Excluir um Cluster**:
   ```bash
   pg_dropcluster 9.5 teste
   ```

**Exemplo Completo**: Criação e gerenciamento de dois clusters chamados "teste" e "teste2":
```bash
pg_createcluster 9.5 teste
pg_createcluster 9.5 teste2
pg_lsclusters   # Exibe os clusters criados
pg_ctlcluster 9.5 teste start   # Inicia o cluster "teste"
pg_ctlcluster 9.5 teste2 start  # Inicia o cluster "teste2"
pg_ctlcluster 9.5 teste stop    # Para o cluster "teste"
pg_dropcluster 9.5 teste2       # Exclui o cluster "teste2"
```

---

### Aula 2: **Bancos de Dados Criados ao Iniciar um Cluster**

#### Bancos de dados padrão criados:
- `postgres`
- `template1`
- `template0`

#### Comandos e exemplos:
1. **Criar uma tabela no `template1`**:
   ```sql
   psql -d template1
   CREATE TABLE usuarios (
       id SERIAL PRIMARY KEY,
       nome VARCHAR(100)
   );
   ```

2. **Criar um novo banco de dados a partir do `template1`**:
   ```bash
   createdb meu_novo_banco
   ```

3. **Verificar se a tabela foi herdada**:
   ```sql
   psql -d meu_novo_banco
   \dt   # Verifica as tabelas
   ```

Neste exemplo, o novo banco de dados "meu_novo_banco" herdará a tabela `usuarios` do `template1`.

---

### Aula 3: **Criação de Clusters Manualmente**

#### Processo manual:
1. **Criar diretório para o cluster**:
   ```bash
   mkdir -p /var/lib/postgresql/9.5/teste
   ```

2. **Inicializar o Cluster com `initdb`**:
   ```bash
   initdb -D /var/lib/postgresql/9.5/teste
   ```

3. **Iniciar o Cluster manualmente**:
   ```bash
   pg_ctl -D /var/lib/postgresql/9.5/teste -l logfile start
   ```

4. **Parar o Cluster manualmente**:
   ```bash
   pg_ctl -D /var/lib/postgresql/9.5/teste stop
   ```

**Exemplo Completo**:
```bash
mkdir -p /var/lib/postgresql/9.5/teste  # Criar diretório para o cluster
initdb -D /var/lib/postgresql/9.5/teste  # Inicializar o cluster
pg_ctl -D /var/lib/postgresql/9.5/teste -l /tmp/teste.log start  # Iniciar o cluster
pg_ctl -D /var/lib/postgresql/9.5/teste stop  # Parar o cluster
```

---

### Aula 4: **Trabalhando com Múltiplas Versões do PostgreSQL**

#### Exemplo de Múltiplas Instalações:
- Verifique os clusters disponíveis:
  ```bash
  pg_lsclusters
  ```
  **Saída**:
  ```
  Ver  Cluster  Port Status Owner    Data directory               Log file
  9.1  main     5432 online postgres /var/lib/postgresql/9.1/main /var/log/postgresql/postgresql-9.1-main.log
  9.3  main     5433 online postgres /var/lib/postgresql/9.3/main /var/log/postgresql/postgresql-9.3-main.log
  9.5  main     5434 online postgres /var/lib/postgresql/9.5/main /var/log/postgresql/postgresql-9.5-main.log
  ```

1. **Conectar-se a um cluster específico (versão 9.5, porta 5434)**:
   ```bash
   psql -p 5434
   ```

2. **Conectar-se a um cluster de outra versão (9.3, porta 5433)**:
   ```bash
   psql -p 5433
   ```

3. **Parar um cluster específico (9.1, porta 5432)**:
   ```bash
   pg_ctlcluster 9.1 main stop
   ```

**Exemplo Completo**: Gerenciando múltiplas versões
```bash
pg_ctlcluster 9.1 main start    # Inicia o cluster da versão 9.1
psql -p 5432                    # Conecta-se à versão 9.1
pg_ctlcluster 9.3 main stop     # Para o cluster da versão 9.3
psql -p 5434                    # Conecta-se à versão 9.5
```
