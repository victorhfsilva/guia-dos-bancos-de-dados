# Tabelas em Bancos de Dados PostgreSQL

As tabelas são estruturas fundamentais em bancos de dados PostgreSQL, utilizadas para armazenar e organizar dados de forma tabular. Neste guia, exploraremos como criar, editar e excluir tabelas, além de apresentar exemplos práticos de cada operação.

### Criação de Tabelas:

1. **Criar uma Tabela:**
   - Utilize o comando `CREATE TABLE` para criar uma nova tabela. É possível especificar o nome da tabela, as colunas e suas definições. Por exemplo:
     ```sql
     CREATE TABLE IF NOT EXISTS clientes (
         codigo INTEGER PRIMARY KEY,
         nome VARCHAR(50) NOT NULL,
         ativo BOOLEAN NOT NULL DEFAULT TRUE,
         data_criacao TIMESTAMP NOT NULL DEFAULT NOW()
     );
     ```
   - Este exemplo cria uma tabela chamada "clientes" com quatro colunas: "codigo", "nome", "ativo" e "data_criacao".

2. **Exemplo de Criação de Tabela:**
   - Abaixo, temos um exemplo detalhado de criação de uma tabela "compras" com restrições e chaves estrangeiras:
     ```sql
     CREATE TABLE IF NOT EXISTS compras (
         id SERIAL,
         codigo_cliente INTEGER,
         compra VARCHAR(15) NOT NULL UNIQUE,
         data_compra TIMESTAMP NOT NULL DEFAULT CURRENT_TIMESTAMP,
         PRIMARY KEY (id),
         FOREIGN KEY (codigo_cliente) REFERENCES clientes(codigo)
     );
     ```

#### Edição de Tabelas:

1. **Editar uma Tabela:**
   - Utilize o comando `ALTER TABLE` para editar uma tabela existente e aplicar alterações como adição de colunas, restrições ou exclusão de constraints.

2. **Adicionar uma Coluna na Tabela:**
   - Para adicionar uma nova coluna a uma tabela existente, utilize o comando `ALTER TABLE ADD COLUMN`. Por exemplo:
     ```sql
     ALTER TABLE clientes ADD COLUMN email VARCHAR(100);
     ```

3. **Adicionar Coluna como Chave Estrangeira:**
   - Você pode adicionar uma coluna como chave estrangeira utilizando `ALTER TABLE ADD COLUMN` com a cláusula `REFERENCES`. Por exemplo:
     ```sql
     ALTER TABLE compras ADD COLUMN codigo_cliente INTEGER REFERENCES clientes(codigo);
     ```

4. **Excluir Constraint:**
   - Utilize o comando `ALTER TABLE DROP CONSTRAINT` para excluir uma constraint de uma tabela. Por exemplo:
     ```sql
     ALTER TABLE alunos DROP CONSTRAINT alunos_pkey;
     ```

#### Exclusão de Tabelas:

1. **Deletar uma Tabela:**
   - Utilize o comando `DROP TABLE` para excluir uma tabela existente. Por exemplo:
     ```sql
     DROP TABLE IF EXISTS clientes;
     ```
   - Isso excluirá a tabela "clientes" se ela existir.

#### Conclusão:

As tabelas são componentes essenciais em bancos de dados PostgreSQL, permitindo a organização e armazenamento estruturado de dados. Compreender como criar, editar e excluir tabelas é fundamental para o desenvolvimento e administração eficazes de bancos de dados relacionais.