## Relacionamentos no MongoDB

### Introdução

O MongoDB é um banco de dados NoSQL que organiza dados em documentos flexíveis, ao contrário dos bancos de dados relacionais que usam tabelas e colunas. No entanto, ainda é possível modelar relacionamentos entre dados no MongoDB. Existem duas principais abordagens para lidar com relacionamentos: **referências** e **incorporação** (embedding).

### Referências

As referências são usadas para manter os dados normalizados e evitar duplicações. Elas funcionam como chaves estrangeiras nos bancos de dados relacionais.

#### Referências Simples

Um documento pode armazenar o identificador (_id) de outro documento.

**Exemplo**:
Vamos supor que temos duas coleções: `usuarios` e `pedidos`.

- **Coleção `usuarios`**:
  ```javascript
  db.usuarios.insertOne({
      _id: ObjectId("60c72b2f4f1a4e1a4f123456"),
      nome: "João",
      email: "joao@example.com"
  })
  ```

- **Coleção `pedidos`**:
  ```javascript
  db.pedidos.insertOne({
      _id: ObjectId("60c72b2f4f1a4e1a4f123457"),
      usuarioId: ObjectId("60c72b2f4f1a4e1a4f123456"),
      produto: "Produto A",
      quantidade: 2,
      data: new Date()
  })
  ```

Para buscar pedidos de um usuário específico, você pode usar a referência `usuarioId`:

```javascript
db.pedidos.find({ usuarioId: ObjectId("60c72b2f4f1a4e1a4f123456") })
```

#### Join com `$lookup`

O operador `$lookup` é usado para realizar junções (joins) entre coleções, semelhante a uma junção SQL.

**Exemplo**:
```javascript
db.pedidos.aggregate([
    {
        $lookup: {
            from: "usuarios",
            localField: "usuarioId",
            foreignField: "_id",
            as: "detalhesUsuario"
        }
    }
])
```

Este pipeline de agregação adiciona os detalhes do usuário de cada pedido na coleção `pedidos`.

### Incorporação (Embedding)

A incorporação é usada para aninhar documentos dentro de outros documentos. Esta abordagem é útil para dados que são frequentemente acessados juntos.

#### Incorporação Simples

Um documento pode incorporar diretamente outros documentos como subdocumentos.

**Exemplo**:
Vamos criar uma coleção `pedidos` que incorpora informações do usuário diretamente.

- **Coleção `pedidos`**:
  ```javascript
  db.pedidos.insertOne({
      _id: ObjectId("60c72b2f4f1a4e1a4f123458"),
      usuario: {
          _id: ObjectId("60c72b2f4f1a4e1a4f123456"),
          nome: "João",
          email: "joao@example.com"
      },
      produto: "Produto A",
      quantidade: 2,
      data: new Date()
  })
  ```

Com esta estrutura, os dados do usuário são armazenados diretamente no documento do pedido.

### Quando Usar Referências vs. Incorporação

A escolha entre usar referências ou incorporação depende de vários fatores, como a natureza dos dados, a frequência de acesso e a estrutura das consultas.

#### Use Referências Quando:

- **Dados Normalizados**: Você deseja evitar duplicação de dados e manter uma estrutura normalizada.
- **Tamanho dos Dados**: Os documentos embutidos seriam muito grandes e excederiam o limite de tamanho de documento do MongoDB (16 MB).
- **Relacionamentos Muitos-para-Muitos**: Quando múltiplos documentos em uma coleção estão relacionados a múltiplos documentos em outra coleção.

#### Use Incorporação Quando:

- **Acesso Frequente**: Os dados embutidos são frequentemente acessados juntos.
- **Performance**: Deseja melhorar a performance de leitura ao evitar consultas adicionais.
- **Relacionamentos Um-para-Muitos**: Quando um documento é relacionado a muitos documentos menores que não serão consultados de forma independente.

### Exemplo de Relacionamento Muitos-para-Muitos

Vamos criar um exemplo onde temos `autores` e `livros`, com um relacionamento muitos-para-muitos usando referências.

- **Coleção `autores`**:
  ```javascript
  db.autores.insertMany([
      { _id: ObjectId("60c72b2f4f1a4e1a4f123459"), nome: "Autor A" },
      { _id: ObjectId("60c72b2f4f1a4e1a4f123460"), nome: "Autor B" }
  ])
  ```

- **Coleção `livros`**:
  ```javascript
  db.livros.insertMany([
      { _id: ObjectId("60c72b2f4f1a4e1a4f123461"), titulo: "Livro 1", autoresIds: [ ObjectId("60c72b2f4f1a4e1a4f123459"), ObjectId("60c72b2f4f1a4e1a4f123460") ] },
      { _id: ObjectId("60c72b2f4f1a4e1a4f123462"), titulo: "Livro 2", autoresIds: [ ObjectId("60c72b2f4f1a4e1a4f123459") ] }
  ])
  ```

Para buscar livros com seus autores:

```javascript
db.livros.aggregate([
    {
        $lookup: {
            from: "autores",
            localField: "autoresIds",
            foreignField: "_id",
            as: "detalhesAutores"
        }
    }
])
```
