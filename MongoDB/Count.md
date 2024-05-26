# Contagens no MongoDB

## Introdução

### Método `countDocuments`

O método `countDocuments` conta o número de documentos em uma coleção que correspondem a uma consulta específica. É a forma mais precisa de contar documentos com base em critérios de filtro.

#### Sintaxe

```javascript
db.nome_da_colecao.countDocuments(filtro)
```

#### Exemplo

Para contar o número de documentos na coleção `usuarios` onde a cidade é "São Paulo":

```javascript
db.usuarios.countDocuments({ cidade: "São Paulo" })
```

### Método `estimatedDocumentCount`

O método `estimatedDocumentCount` retorna uma estimativa do número total de documentos em uma coleção. É mais rápido que `countDocuments` porque não realiza uma varredura completa da coleção, mas pode ser menos preciso.

#### Sintaxe

```javascript
db.nome_da_colecao.estimatedDocumentCount()
```

#### Exemplo

Para obter uma estimativa do número total de documentos na coleção `usuarios`:

```javascript
db.usuarios.estimatedDocumentCount()
```

### Comparação entre `countDocuments` e `estimatedDocumentCount`

- **`countDocuments`**: 
  - Realiza uma contagem precisa dos documentos que correspondem ao filtro.
  - Pode ser mais lento, pois realiza uma varredura completa da coleção.
  - Ideal para consultas específicas.

- **`estimatedDocumentCount`**: 
  - Fornece uma estimativa rápida do número total de documentos na coleção.
  - Não realiza uma varredura completa da coleção.
  - Ideal para obter uma visão geral rápida do tamanho da coleção.