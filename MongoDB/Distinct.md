## Método `distinct` no MongoDB

### Introdução

O método `distinct` no MongoDB é usado para retornar valores distintos de um campo específico em uma coleção. Esse método é útil para encontrar todos os valores únicos que um campo pode ter, eliminando duplicatas e fornecendo uma visão clara da diversidade de valores dentro de seus documentos.

### Sintaxe Básica

A sintaxe básica do método `distinct` é a seguinte:

```javascript
db.nome_da_colecao.distinct(campo, query, options)
```

- `campo`: O campo para o qual você deseja obter valores distintos.
- `query`: (Opcional) Um documento de consulta para filtrar os documentos antes de encontrar os valores distintos.
- `options`: (Opcional) Configurações adicionais para a operação.

### Exemplos de Uso

#### Obter Valores Distintos de um Campo

Para obter todos os valores distintos do campo `cidade` na coleção `usuarios`, use o seguinte comando:

```javascript
db.usuarios.distinct("cidade")
```

#### Obter Valores Distintos com Filtro

Você pode aplicar um filtro para refinar os documentos antes de buscar valores distintos. Por exemplo, para encontrar todas as idades distintas dos usuários que vivem em "São Paulo":

```javascript
db.usuarios.distinct("idade", { cidade: "São Paulo" })
```

### Usando Opções Adicionais

O método `distinct` pode aceitar opções adicionais, embora elas sejam menos comuns. Por exemplo, você pode usar collation para especificar regras de comparação de string ao executar a operação distinct.

#### Exemplo de Collation

Para obter valores distintos de `nome` em uma coleção `usuarios` com collation insensível a maiúsculas/minúsculas:

```javascript
db.usuarios.distinct("nome", {}, { collation: { locale: 'en', strength: 2 } })
```

### Opções Adicionais

#### `collation`

A opção `collation` permite especificar regras de comparação de strings ao executar a operação distinct. Isso é útil para controlar a sensibilidade a maiúsculas/minúsculas, acentos, e outras regras de ordenação de texto.

**Sintaxe**:
```javascript
{ collation: { locale: 'en', strength: 2 } }
```

- `locale`: Especifica a localidade a ser usada. Exemplo: `'en'` para inglês, `'fr'` para francês.
- `strength`: Define a sensibilidade da comparação:
  - `1`: Insensível a caso e diacríticos (acentos).
  - `2`: Sensível a caso, mas insensível a diacríticos.
  - `3`: Sensível a caso e diacríticos.
  - `4`: Sensível a caso, diacríticos e variações de símbolos.

**Exemplo**:
Para obter valores distintos de `nome` com insensibilidade a maiúsculas/minúsculas:

```javascript
db.usuarios.distinct("nome", {}, { collation: { locale: 'en', strength: 1 } })
```

#### `maxTimeMS`

A opção `maxTimeMS` especifica um limite de tempo (em milissegundos) para a execução da operação distinct. Isso ajuda a evitar consultas longas que possam impactar negativamente o desempenho do banco de dados.

**Sintaxe**:
```javascript
{ maxTimeMS: tempo_em_ms }
```

**Exemplo**:
Para limitar a operação a 1000 milissegundos (1 segundo):

```javascript
db.usuarios.distinct("cidade", {}, { maxTimeMS: 1000 })
```

#### `hint`

A opção `hint` permite especificar um índice a ser usado para a operação distinct. Isso pode melhorar a performance da consulta ao direcionar o MongoDB para usar um índice específico.

**Sintaxe**:
```javascript
{ hint: { campo: 1 } }
```

**Exemplo**:
Para usar o índice no campo `cidade`:

```javascript
db.usuarios.distinct("cidade", {}, { hint: { cidade: 1 } })
```
