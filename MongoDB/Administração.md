## Administração de Banco de Dados no MongoDB

### Introdução

Administrar um banco de dados MongoDB envolve diversas tarefas, como criar e gerenciar usuários, realizar backups e restaurações, monitorar a performance, e configurar replicação e sharding para alta disponibilidade e escalabilidade. 

### Gerenciamento de Usuários e Permissões

#### Criar Usuário

Para criar um usuário com permissões específicas, use o comando `createUser`:

```javascript
db.createUser({
    user: "nome_do_usuario",
    pwd: "senha_do_usuario",
    roles: [
        { role: "role", db: "nome_do_banco" }
    ]
})
```

Exemplo:
```javascript
db.createUser({
    user: "admin",
    pwd: "senha123",
    roles: [
        { role: "userAdminAnyDatabase", db: "admin" }
    ]
})
```

#### Remover Usuário

Para remover um usuário:

```javascript
db.dropUser("nome_do_usuario")
```

Exemplo:
```javascript
db.dropUser("admin")
```

#### Listar Usuários

Para listar todos os usuários do banco de dados atual:

```javascript
db.getUsers()
```

### Backup e Restore

#### Backup

Para criar um backup de um banco de dados MongoDB, use o utilitário `mongodump`:

```bash
mongodump --db nome_do_banco --out caminho_do_backup
```

Exemplo:
```bash
mongodump --db minhaBaseDeDados --out /backups/minhaBaseDeDados
```

#### Restore

Para restaurar um banco de dados a partir de um backup, use o utilitário `mongorestore`:

```bash
mongorestore --db nome_do_banco caminho_do_backup/nome_do_banco
```

Exemplo:
```bash
mongorestore --db minhaBaseDeDados /backups/minhaBaseDeDados
```

###  Monitoramento e Performance

#### Monitorar a Performance

MongoDB oferece várias ferramentas e comandos para monitorar a performance do banco de dados.

- **`db.stats()`**: Fornece estatísticas sobre o banco de dados atual.
- **`db.collection.stats()`**: Fornece estatísticas detalhadas sobre uma coleção específica.
- **`db.serverStatus()`**: Fornece uma visão geral do status do servidor.

Exemplo:
```javascript
db.stats()
db.usuarios.stats()
db.serverStatus()
```