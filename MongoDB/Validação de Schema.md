## Validação de Schema no MongoDB

### Introdução

A validação de schema no MongoDB permite definir regras e restrições para os dados inseridos em uma coleção. Essas regras garantem que os documentos inseridos ou atualizados em uma coleção atendam a determinados critérios, ajudando a manter a integridade e a qualidade dos dados. A validação de schema é definida usando o recurso de `validator` em conjunto com o formato de especificação JSON Schema.

### Definindo Validações de Schema

A validação de schema é configurada ao criar uma coleção ou ao modificar uma coleção existente usando o comando `createCollection` ou `collMod`, respectivamente.

### Criando uma Coleção com Validação de Schema

Para criar uma nova coleção com validação de schema, use o comando `createCollection` e defina o parâmetro `validator` com a especificação JSON Schema.

#### Exemplo Básico

Vamos criar uma coleção chamada `usuarios` com validação de schema para garantir que cada documento tenha um campo `nome` do tipo string e um campo `idade` do tipo inteiro.

```javascript
db.createCollection("usuarios", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["nome", "idade"],
            properties: {
                nome: {
                    bsonType: "string",
                    description: "O campo 'nome' é obrigatório e deve ser uma string"
                },
                idade: {
                    bsonType: "int",
                    minimum: 0,
                    description: "O campo 'idade' é obrigatório e deve ser um inteiro não negativo"
                }
            }
        }
    }
})
```

### Modificando uma Coleção Existente com Validação de Schema

Para adicionar ou modificar a validação de schema de uma coleção existente, use o comando `collMod`.

#### Exemplo

Adicionar validação de schema à coleção `usuarios` existente:

```javascript
db.runCommand({
    collMod: "usuarios",
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["nome", "idade"],
            properties: {
                nome: {
                    bsonType: "string",
                    description: "O campo 'nome' é obrigatório e deve ser uma string"
                },
                idade: {
                    bsonType: "int",
                    minimum: 0,
                    description: "O campo 'idade' é obrigatório e deve ser um inteiro não negativo"
                }
            }
        }
    },
    validationLevel: "strict",
    validationAction: "error"
})
```

### Níveis de Validação e Ações

Ao definir validações de schema, você pode especificar o `validationLevel` e o `validationAction`.

#### `validationLevel`

- **`off`**: Desativa a validação de schema.
- **`moderate`**: Valida os documentos inseridos ou atualizados, mas não os documentos existentes.
- **`strict`**: Valida todos os documentos inseridos e atualizados.

#### `validationAction`

- **`error`**: Rejeita documentos que não atendem aos critérios de validação.
- **`warn`**: Permite a inserção ou atualização de documentos que não atendem aos critérios de validação, mas gera um aviso.


### Exemplo Completo

Vamos criar uma coleção `produtos` com regras de validação mais avançadas, incluindo campos opcionais e restrições adicionais.

```javascript
db.createCollection("produtos", {
    validator: {
        $jsonSchema: {
            bsonType: "object",
            required: ["nome", "preco"],
            properties: {
                nome: {
                    bsonType: "string",
                    description: "O campo 'nome' é obrigatório e deve ser uma string"
                },
                preco: {
                    bsonType: "double",
                    minimum: 0,
                    description: "O campo 'preco' é obrigatório e deve ser um número positivo"
                },
                categoria: {
                    bsonType: "string",
                    description: "O campo 'categoria' é opcional e deve ser uma string"
                },
                estoque: {
                    bsonType: "int",
                    minimum: 0,
                    description: "O campo 'estoque' é opcional e deve ser um inteiro não negativo"
                }
            }
        }
    },
    validationLevel: "moderate",
    validationAction: "warn"
})
```

### Conclusão

A validação de schema no MongoDB é uma ferramenta poderosa para garantir a integridade dos dados. Com ela, você pode definir regras e restrições para os documentos em suas coleções, assegurando que os dados atendam aos critérios especificados. Usando a validação de schema, você pode melhorar a consistência e a qualidade dos dados armazenados no MongoDB.