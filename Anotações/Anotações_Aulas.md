# Principais anotações sobre cada aula

### **Aula 1 - Ferramentas Extras do Debian para Gerenciamento de Clusters no PostgreSQL**
- Ferramentas do Debian facilitam a gestão de clusters de PostgreSQL, especialmente com comandos específicos.
- Comandos importantes:
  - `pg_lsclusters`: lista todos os clusters ativos e suas configurações.
  - `pg_createcluster`: cria um novo cluster com uma versão especificada.
  - `pg_ctlcluster`: controla o estado dos clusters (start, stop, restart).
  - `pg_dropcluster`: remove clusters.
- Exemplo de criação de um cluster:
  - Criar um cluster chamado "teste" com a versão 9.5, porta diferente da padrão (5433), pois a porta 5432 já está em uso.
  - Com o comando `pg_ctlcluster`, é possível iniciar, parar ou reiniciar clusters conforme necessário.
- Ferramentas disponibilizadas pelo Debian ajudam a evitar erros manuais, criando automaticamente diretórios, permissões e estruturas necessárias para o cluster.

### **Aula 2 - Bancos de Dados Padrão Criados com Clusters**
- Quando um cluster é criado, três bancos de dados são gerados automaticamente:
  1. **postgres**: usado para o gerenciamento geral do PostgreSQL.
  2. **template1**: modelo base para novos bancos de dados.
  3. **template0**: backup imutável de `template1`, usado para recriação.
- O banco de dados `template1` pode ser modificado para incluir tabelas, estruturas ou objetos que serão herdados por novos bancos.
- Exemplo:
  - Criar uma tabela `usuarios` dentro de `template1`.
  - Ao criar um novo banco de dados, essa tabela será automaticamente copiada para ele.
- O banco `postgres` normalmente não deve ser alterado, mas pode ser usado em situações específicas, como criar catálogos.

### **Aula 3 - Criação de Clusters Manualmente**
- Processo manual de criação de clusters no PostgreSQL sem as ferramentas do Debian:
  - Criar diretório manualmente no caminho desejado.
  - Utilizar o comando `initdb` para configurar o cluster.
  - Ativar o cluster com `pg_ctl` e definir logs e parâmetros de inicialização.
- Comparação entre a criação manual e o uso das ferramentas do Debian:
  - A criação manual envolve mais passos e é mais suscetível a erros.
  - Ferramentas automáticas, como as do Debian, são mais fáceis e evitam problemas.
- Comandos essenciais:
  - `pg_ctl start`: para iniciar o cluster.
  - `pg_ctl stop`: para parar o cluster.

### **Aula 4 - Trabalhando com Múltiplas Versões do PostgreSQL**
- É possível instalar e gerenciar diferentes versões do PostgreSQL em paralelo.
- Cada versão do PostgreSQL roda em uma porta específica, e é possível acessar cada uma delas especificando a porta no comando de conexão.
  - Exemplo: Conectar à versão 9.5 usando a porta 5432 e à versão 9.3 usando a porta 5434.
- O estado de cada cluster (online ou offline) é importante. Clusters offline não aceitam conexões.
- Uso de `pg_ctlcluster` para iniciar ou parar clusters em versões específicas.

### Aula 5: Arquivos e Diretórios do PostgreSQL
- **Diretório Base**: Armazena informações sobre os bancos de dados.
- **PG Estadia**: Permite visualizar informações dos objetos criados.
- **Diretórios Importantes**:
  - `PG Comits`: Armazena status de transações.
  - `PG Yagami`: Armazena arquivos usados pelo subsistema de memória.
  - `PG Log`: Armazena informações sobre os commits.
  - `PG Estatísticas`: Armazena arquivos sobre estatísticas do banco de dados.
  - Outros diretórios incluem `PG Note`, `PG Serial`, `PG Snapshot`, etc.

### Aula 6: Bancos de Dados e Esquemas
- **Banco de Dados**: O maior objeto no PostgreSQL, cada um é independente e possui suas próprias permissões.
- **Comandos**:
  - `CREATE DATABASE`: Cria um novo banco de dados.
  - `DROP DATABASE`: Exclui um banco de dados.
- **Esquemas**: Subdivisões dentro de um banco de dados, comparáveis a diretórios. Não podem conter outros esquemas.
- **Comandos de Esquema**:
  - `CREATE SCHEMA`: Cria um novo esquema.
  - `ALTER TABLE ... SET SCHEMA`: Move uma tabela para outro esquema.

### Aula 7 (Parte 1): Tablespaces
- **Tablespaces**: Diretórios definidos para armazenar objetos do PostgreSQL, permitindo distribuição de carga.
- **Criação de um Tablespace**: Apenas superusuários podem criar. Um diretório deve ser criado previamente.
- **Link Simbólico**: Criado para o tablespace no diretório do cluster.

### Aula 8 (Parte 2): Tablespaces
- **Estrutura Física e Lógica**: Um tablespace pode conter múltiplos objetos (tabelas, índices, etc.).
- **Alterando o Tablespace de uma Tabela**: Uso do comando `ALTER TABLE ... SET TABLESPACE`.
- **Impacto no Ambiente de Produção**: Mudanças podem gerar locks, por isso é importante realizar em janelas de manutenção.

### Aula 9: Índices no PostgreSQL

- **Índices** são estruturas que aceleram o acesso aos dados nas tabelas, funcionando como um índice de um livro, que te leva diretamente à informação.
- Antes de criar um índice, podemos ajustar a tabela, adicionando colunas se necessário. Isso é feito com o comando de alteração da tabela.
- **Tipos de índices**:
  - **B-Tree**: É o mais usado, ideal para comparações do tipo maior, menor, igual. Ele é muito eficiente e serve para a maioria dos casos.
  - **Hash**: Usado apenas para comparações de igualdade. Menos utilizado atualmente, pois não é tão performático quanto o B-Tree e tem desvantagens, como não replicar em servidores de backup.
  - **GiST**: Útil para comparações mais específicas, como operações geométricas ou com dados biométricos. Trabalha bem com certos tipos de comparações e estruturas balanceadas.
  - **SP-GiST**: Similar ao GiST, mas trabalha com estruturas não balanceadas, como árvores de pontos. Ideal para dados geométricos em duas dimensões.
  - **GIN**: Bom para dados complexos como vetores e textos grandes. É muito usado em buscas textuais com vários tokens.
  - **BRIN**: Introduzido na versão 9.5. É usado para grandes volumes de dados, sendo eficiente para dados que seguem uma ordem natural. Porém, ainda é menos comum no dia a dia.

- Também podemos criar **índices compostos** (usando mais de uma coluna) para melhorar consultas mais específicas. Outro recurso é o **índice parcial**, que cria um índice apenas para um subconjunto dos dados da tabela, tornando-o mais rápido e eficiente.

### Aula 10: Carga de Dados no PostgreSQL

- **Carga de dados** é uma forma de inserir dados a partir de um arquivo externo, o que facilita populações grandes de tabelas. Isso é feito geralmente com scripts SQL.
- Um arquivo de carga contém comandos para criar bancos de dados, tabelas e fazer inserções de dados. Esse arquivo pode ser executado pelo terminal do PostgreSQL.
- Além de executar localmente, também é possível realizar essa carga via rede, conectando-se a um servidor remoto.

