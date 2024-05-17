# Tipos de Colunas no PostgreSQL

As colunas em um banco de dados PostgreSQL representam os atributos ou campos de uma tabela, e cada coluna tem um tipo de dado associado que define o tipo de valor que pode ser armazenado nela.

## Tipos de Dados Básicos:

### INTEGER / SERIAL:

- Armazena números inteiros.
- O tipo `SERIAL` é usado para gerar automaticamente valores sequenciais únicos.

- **Exemplo:**
  ```sql
  CREATE TABLE produtos (
      id SERIAL PRIMARY KEY,
      nome VARCHAR(100),
      preco INTEGER
  );
  ```
  
### VARCHAR / TEXT:

- Armazena strings de comprimento variável.
- `VARCHAR` tem limite de comprimento especificado, enquanto `TEXT` não tem limite.

- **Exemplo:**
  ```sql
  CREATE TABLE clientes (
      id SERIAL PRIMARY KEY,
      nome VARCHAR(100),
      endereco TEXT
  );
  ```

### BOOLEAN:

- Armazena valores de verdadeiro ou falso (true/false).

- **Exemplo:**
  ```sql
  CREATE TABLE tarefas (
      id SERIAL PRIMARY KEY,
      descricao VARCHAR(200),
      concluida BOOLEAN DEFAULT FALSE
  );
  ```

### NUMERIC / DECIMAL:

- **Utilização:** Armazena números decimais de precisão arbitrária.
- **Exemplo:**
  ```sql
  CREATE TABLE contas (
      id SERIAL PRIMARY KEY,
      nome VARCHAR(100),
      saldo DECIMAL(10, 2)
  );
  ```

- **Precisão e Escala:** O tipo `DECIMAL` aceita dois parâmetros opcionais: precisão e escala. A precisão define o número total de dígitos significativos que podem ser armazenados, enquanto a escala define o número de dígitos que podem ser armazenados após o ponto decimal.

- **Sintaxe:** A sintaxe básica para criar uma coluna `DECIMAL` é a seguinte:

  ```sql
  nome_da_coluna DECIMAL(precisao, escala)
  ```

- **Exemplo:** Por exemplo, se criarmos uma coluna `preco` do tipo `DECIMAL(10, 2)`, isso significa que podemos armazenar valores com até 10 dígitos, dos quais 2 estão reservados para a parte decimal.
  ```sql
  CREATE TABLE produtos (
      id SERIAL PRIMARY KEY,
      nome VARCHAR(100),
      preco DECIMAL(10, 2)
  );
  ```

- **Armazenamento de Valores:** O valor real armazenado em uma coluna `DECIMAL` pode ser menor que a precisão definida, mas não maior que a precisão. Por exemplo, em uma coluna `DECIMAL(10, 2)`, podemos armazenar valores como 123.45 ou 999999.99, mas não 1234567.89.

- **Arredondamento:** O PostgreSQL gerencia automaticamente o arredondamento dos valores de acordo com a escala definida. Por exemplo, se tentarmos inserir 123.456 em uma coluna `DECIMAL(5, 2)`, o valor será arredondado para 123.46.


### DATE:

- Armazena datas (ano, mês e dia).

- **Exemplo:**
  ```sql
  CREATE TABLE funcionarios (
      id SERIAL PRIMARY KEY,
      nome VARCHAR(100),
      data_contratacao DATE
  );
  ```

### TIME:

- Armazena informações de tempo (horas, minutos, segundos).

- **Exemplo:**
  ```sql
  CREATE TABLE reunioes (
      id SERIAL PRIMARY KEY,
      assunto VARCHAR(200),
      hora_inicio TIME,
      hora_fim TIME
  );
  ```

### TIMESTAMP / TIMESTAMPTZ:

- **Utilização:** Armazena data e hora, com ou sem fuso horário.
- **Exemplo:**
  ```sql
  CREATE TABLE registros (
      id SERIAL PRIMARY KEY,
      evento VARCHAR(200),
      data_hora TIMESTAMP DEFAULT CURRENT_TIMESTAMP
  );
  ```

## Conclusão:

Os tipos de colunas no PostgreSQL oferecem uma variedade de opções para armazenar diferentes tipos de dados de forma eficiente e segura. Ao escolher um tipo de coluna para uma tabela, é importante considerar o tipo de dados a ser armazenado, o tamanho dos dados e os requisitos de consulta e indexação. Compreender os diferentes tipos de colunas disponíveis no PostgreSQL é fundamental para projetar e otimizar bancos de dados de maneira eficaz.