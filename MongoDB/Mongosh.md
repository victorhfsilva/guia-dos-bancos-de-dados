## MongoDB Shell (mongosh)

### Introdução

O MongoDB Shell (`mongosh`) é uma interface de linha de comando interativa para o MongoDB, que permite interagir com bancos de dados MongoDB diretamente do terminal. Ele oferece uma maneira eficiente de realizar operações administrativas, manipular dados e executar consultas.

### Conexão ao MongoDB

Para conectar-se a um banco de dados MongoDB usando o `mongosh`, execute o seguinte comando:

```bash
mongosh "mongodb://<hostname>:<port>"
```

Por exemplo, para se conectar a um MongoDB local na porta padrão (27017):

```bash
mongosh "mongodb://localhost:27017"
```

### Comandos Básicos

1. **Mostrar Bancos de Dados**:
   ```javascript
   show dbs
   ```
   Este comando lista todos os bancos de dados disponíveis.

2. **Usar um Banco de Dados**:
   ```javascript
   use <nome_do_banco>
   ```
   Troca para o banco de dados especificado.

3. **Mostrar Coleções**:
   ```javascript
   show collections
   ```
   Lista todas as coleções dentro do banco de dados atual.

4. **Excluir o Banco de Dados**:
    ```javascript
    use <nome_do_banco>
    db.dropDatabase()
    ```

    Para excluir um banco de dados, selecione o banco de dados e use o comando db.dropDatabase().

### Coleções

#### Criação Explícita

A criação explícita permite definir opções específicas para a coleção.

```javascript
db.createCollection("nome_da_colecao", { opções })
```

**Opções Comuns**:

- `capped`: Se verdadeiro, cria uma coleção de tamanho fixo.
- `size`: Especifica o tamanho máximo em bytes para uma coleção capada.
- `max`: Especifica o número máximo de documentos em uma coleção capada.

**Exemplo**:
```javascript
db.createCollection("usuarios", { capped: true, size: 5242880, max: 5000 })
```

Neste exemplo, a coleção `usuarios` é uma coleção capada com um tamanho máximo de 5 MB e no máximo 5000 documentos.

#### Criação Implícita

A criação implícita ocorre quando você insere um documento em uma coleção que ainda não existe. O MongoDB cria a coleção automaticamente.

**Exemplo**:
```javascript
db.usuarios.insertOne({ nome: "João", idade: 30, cidade: "São Paulo" })
```

Se a coleção `usuarios` não existir, ela será criada automaticamente com a inserção do documento.

#### Exclusão de Coleções

Para deletar uma coleção inteira e todos os documentos que ela contém, use o método `drop`:

```javascript
db.nome_da_colecao.drop()
```

**Exemplo**:
```javascript
db.usuarios.drop()
```
Este comando exclui a coleção `usuarios` e todos os seus documentos.


### Operações CRUD

1. **Criar/Incluir Documento**:
   ```javascript
   db.<nome_da_colecao>.insertOne({ <chave>: <valor>, ... })
   db.<nome_da_colecao>.insertMany([{ <chave>: <valor>, ... }, ...])
   ```
   Exemplo:
   ```javascript
   db.usuarios.insertOne({ nome: "João", idade: 30 })
   db.usuarios.insertMany([{ nome: "Maria", idade: 25 }, { nome: "Pedro", idade: 35 }])
   ```

2. **Ler Documentos**:
   ```javascript
   db.<nome_da_colecao>.find({ <filtro> })
   db.<nome_da_colecao>.findOne({ <filtro> })
   ```
   Exemplo:
   ```javascript
   db.usuarios.find({ idade: { $gte: 30 } })
   db.usuarios.findOne({ nome: "João" })
   ```

3. **Atualizar Documentos**:
   ```javascript
   db.<nome_da_colecao>.updateOne({ <filtro> }, { $set: { <chave>: <novo_valor>, ... } })
   db.<nome_da_colecao>.updateMany({ <filtro> }, { $set: { <chave>: <novo_valor>, ... } })
   ```
   Exemplo:
   ```javascript
   db.usuarios.updateOne({ nome: "João" }, { $set: { idade: 31 } })
   db.usuarios.updateMany({ idade: { $lt: 30 } }, { $set: { status: "jovem" } })
   ```

4. **Deletar Documentos**:
   ```javascript
   db.<nome_da_colecao>.deleteOne({ <filtro> })
   db.<nome_da_colecao>.deleteMany({ <filtro> })
   ```
   Exemplo:
   ```javascript
   db.usuarios.deleteOne({ nome: "João" })
   db.usuarios.deleteMany({ idade: { $lt: 25 } })
   ```

