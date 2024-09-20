**Aula 5: Estrutura de Arquivos e Diretórios do PostgreSQL**

1. **Diretório `base`:**
   - Armazena dados dos databases.
   - Quando criamos um novo database, um diretório correspondente com um identificador único (OID) é gerado.
   - Exemplo: O OID do database "teste" pode ser visualizado com o comando:
     ```sql
     SELECT oid FROM pg_database WHERE datname = 'teste';
     ```
     Depois, navegando no diretório `base`, podemos ver o diretório com o OID correspondente.

2. **Diretórios adicionais:**
   - **`pg_log`:** Armazena informações sobre transações e status dos commits.
   - **`pg_commit_ts`:** Introduzido na versão 9.5, armazena timestamps de commit.
   - **`pg_dynshmem`:** Utilizado a partir da versão 9.4 para arquivos relacionados à memória compartilhada.
   - **`pg_stat`:** Contém informações estatísticas permanentes.
   - **`pg_tblspc`:** Armazena links simbólicos para tablespaces.

3. **Arquivos Importantes:**
   - **`PG_VERSION`:** Informa a versão do PostgreSQL.
   - **`postgresql.auto.conf`:** Contém configurações alteradas por comandos `ALTER SYSTEM`.
   - **`postmaster.pid`:** Armazena o PID do processo do PostgreSQL enquanto o sistema está em execução.

**Aula 6: Databases e Schemas**

1. **Criação de Databases:**
   - Os databases são criados com o comando:
     ```sql
     CREATE DATABASE nome_database;
     ```
   - Cada database é independente e não compartilha informações diretamente com outros databases.
   - O comando `\l` lista todos os databases disponíveis.

2. **Criação de Schemas:**
   - Schemas são como diretórios dentro de um database, usados para organizar objetos.
   - Exemplo de criação de schema:
     ```sql
     CREATE SCHEMA rh;
     ```
   - Para referenciar objetos dentro de schemas, usamos o caminho completo. Exemplo:
     ```sql
     SELECT * FROM rh.tabela;
     ```

**Aula 7: Tablespaces (Parte 1)**

1. **O que é um Tablespace:**
   - Um tablespace é uma área física, um diretório onde os dados do PostgreSQL são armazenados.
   - Ele ajuda a distribuir a carga em diferentes discos para melhorar a performance.

2. **Criação de um Tablespace:**
   - Para criar um tablespace, primeiro crie um diretório e garanta que o usuário do PostgreSQL tenha permissões adequadas:
     ```bash
     mkdir /path/to/tablespace
     chown postgres:postgres /path/to/tablespace
     ```
   - Em seguida, crie o tablespace no PostgreSQL:
     ```sql
     CREATE TABLESPACE nome_tablespace LOCATION '/path/to/tablespace';
     ```

3. **Verificando Tablespaces:**
   - O comando `\db` mostra os tablespaces existentes.

**Aula 8: Tablespaces (Parte 2)**

1. **Atribuição de Objects a Tablespaces:**
   - Podemos atribuir um database ou uma tabela a um tablespace específico.
   - Exemplo para criar um database em um tablespace:
     ```sql
     CREATE DATABASE nome_database TABLESPACE nome_tablespace;
     ```

2. **Alterando o Tablespace de uma Tabela:**
   - Para mover uma tabela para outro tablespace:
     ```sql
     ALTER TABLE nome_tabela SET TABLESPACE novo_tablespace;
     ```
   - Este comando pode gerar lock na tabela, por isso deve ser feito em uma janela de manutenção.

### O que foi visto nesta aula: 
- Estrutura de diretórios
- Databases
- Schemas
- Tablespaces
- Ferramentas essenciais para a organização e performance do banco de dados.
