## Agregações no MongoDB

### Introdução

As agregações no MongoDB permitem realizar operações complexas de análise de dados, como cálculos, transformações e manipulações. A ferramenta principal para essas operações é o framework de agregação, que utiliza um pipeline de estágios para processar os documentos.

### Estrutura Básica do Pipeline de Agregação

Um pipeline de agregação é uma sequência de estágios, onde cada estágio aplica uma operação nos dados e passa o resultado para o próximo estágio. A estrutura básica é:

```javascript
db.nome_da_colecao.aggregate([
    { $stage1: { ... } },
    { $stage2: { ... } },
    ...
])
```

### Principais Estágios de Agregação

#### `$match`

Filtra os documentos para passar apenas aqueles que atendem aos critérios de filtro.

```javascript
{ $match: { campo: valor } }
```

Exemplo:
```javascript
db.usuarios.aggregate([
    { $match: { idade: { $gte: 30 } } }
])
```
Filtra os documentos onde a idade é maior ou igual a 30.

#### `$group`

Agrupa os documentos por um campo especificado e aplica operações acumuladoras a cada grupo.

```javascript
{ $group: { _id: "$campo", total: { $sum: "$outro_campo" } } }
```

Exemplo:
```javascript
db.vendas.aggregate([
    { $group: { _id: "$produto", totalVendas: { $sum: "$quantidade" } } }
])
```
Agrupa as vendas por produto e calcula o total vendido de cada produto.

#### `$project`

Seleciona os campos a serem incluídos ou excluídos dos documentos no resultado.

```javascript
{ $project: { campo1: 1, campo2: 1, novoCampo: "$outro_campo" } }
```

Exemplo:
```javascript
db.usuarios.aggregate([
    { $project: { nome: 1, idade: 1, anoDeNascimento: { $subtract: [ 2024, "$idade" ] } } }
])
```
Inclui os campos `nome` e `idade` e adiciona um novo campo `anoDeNascimento` calculado a partir da idade.

#### `$sort`

Ordena os documentos em ordem crescente (`1`) ou decrescente (`-1`) por um campo especificado.

```javascript
{ $sort: { campo: 1 } }
```

Exemplo:
```javascript
db.usuarios.aggregate([
    { $sort: { idade: -1 } }
])
```
Ordena os documentos pela idade em ordem decrescente.

#### `$limit` e `$skip`

Controlam o número de documentos a serem passados para o próximo estágio e ignoram um número especificado de documentos.

```javascript
{ $limit: numero }
{ $skip: numero }
```

Exemplo:
```javascript
db.usuarios.aggregate([
    { $skip: 5 },
    { $limit: 10 }
])
```
Ignora os primeiros 5 documentos e passa os próximos 10.

#### `$lookup`

Realiza uma junção (join) com outra coleção.

```javascript
{
  $lookup: {
    from: "outra_colecao",
    localField: "campo_local",
    foreignField: "campo_estrangeiro",
    as: "novo_campo"
  }
}
```

Exemplo:
```javascript
db.pedidos.aggregate([
    {
        $lookup: {
            from: "clientes",
            localField: "clienteId",
            foreignField: "_id",
            as: "detalhesCliente"
        }
    }
])
```
Junta a coleção `pedidos` com a coleção `clientes` usando o campo `clienteId`.

#### `$unwind`

Desconstrói um campo de array em vários documentos.

```javascript
{ $unwind: "$campo_array" }
```

Exemplo:
```javascript
db.usuarios.aggregate([
    { $unwind: "$hobbies" }
])
```
Desconstrói o campo de array `hobbies` em documentos separados.

### Operadores Comuns em Agregações

#### Operadores Acumuladores

- `$sum`: Soma valores.
- `$avg`: Calcula a média.
- `$min` e `$max`: Encontra o valor mínimo ou máximo.
- `$push`: Adiciona valores a um array.

#### Operadores de Expressão

- `$add`, `$subtract`, `$multiply`, `$divide`: Operações aritméticas.
- `$concat`: Concatena strings.
- `$toUpper`, `$toLower`: Converte strings para maiúsculas ou minúsculas.
- `$substr`: Extrai uma substring.

### Exemplos de Pipelines de Agregação

#### Exemplo 1: Total de Vendas por Produto

```javascript
db.vendas.aggregate([
    { $match: { status: "Aprovado" } },
    { $group: { _id: "$produto", totalVendas: { $sum: "$quantidade" } } },
    { $sort: { totalVendas: -1 } }
])
```
Filtra as vendas aprovadas, agrupa por produto, calcula o total de vendas e ordena em ordem decrescente.

#### Exemplo 2: Idade Média dos Usuários por Cidade

```javascript
db.usuarios.aggregate([
    { $group: { _id: "$cidade", idadeMedia: { $avg: "$idade" } } }
])
```
Agrupa os usuários por cidade e calcula a idade média em cada cidade.

#### Exemplo 3: Relatório de Vendas com Detalhes do Cliente

```javascript
db.pedidos.aggregate([
    {
        $lookup: {
            from: "clientes",
            localField: "clienteId",
            foreignField: "_id",
            as: "detalhesCliente"
        }
    },
    { $unwind: "$detalhesCliente" },
    { $project: { _id: 0, pedido: 1, "detalhesCliente.nome": 1, "detalhesCliente.email": 1 } }
])
```
Realiza uma junção com a coleção `clientes`, desconstrói os detalhes do cliente e projeta apenas os campos relevantes.
