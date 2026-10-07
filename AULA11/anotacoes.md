## AULA 11 - Relacionamentos entre tabelas

> - **Ideia principal:** Um cliente pode possuir vários pedidos.

### Diagrama

```mermaid
erDiagram
    CLIENTES ||--o{ PEDIDOS : faz

CLIENTES {
    id int PK
    nome varchar(50)
}

PEDIDOS{
    id int PK
    produto varchar(50)
    id_cliente int FK
}
```
---

> **1** - Primeiro, criamos a tabela:
```sql
CREATE TABLE clientes (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(50) NOT NULL
);
```

> **2** - Criamos a tabela `pedidos`, referenciando com a coluna `id` da tabela `clientes`:
```sql
CREATE TABLE pedidos (
    id SERIAL PRIMARY KEY,
    produto VARCHAR(50) NOT NULL,
    id_cliente INT REFERENCES clientes(id)
);
```

> **3** - Inserimos clientes na tabela `clientes`:
```sql
INSERT INTO clientes (nome) VALUES
('Hannah'),
('Miguel'),
('Murilo'),
('Lucas');
```

> **4** - Inserimos os pedidos e seus respectivos clientes na tabela `pedidos`:
```sql
INSERT INTO pedidos (produto, id_cliente) VALUES
('Chocolate', 3),
('Salgado', 1),
('Refri', 2),
('Chocolate', 1);
```

> **5** - Para verificar quais os produtos que cada cliente comprou:
```sql
SELECT clientes.nome, pedidos.produto
FROM pedidos
INNER JOIN clientes ON pedidos.id_cliente = clientes.id;
```
*A tabela aparecerá dessa forma:*

![alt text](image.png)

> **6** - Para verificar todos os clientes, até mesmo os que não compraram nada:
```sql
SELECT clientes.nome, pedidos.produto
FROM clientes
LEFT JOIN pedidos ON pedidos.id_cliente = clientes.id;
```
*A tabela aparecerá dessa forma:*

![alt text](image-1.png)

> **7** - Para verificar somente quem não comprou nada:
```sql
SELECT clientes.nome, pedidos.produto
FROM clientes
LEFT JOIN pedidos ON pedidos.id_cliente = clientes.id
WHERE pedidos.id IS NULL;
```
*A tabela aparecerá dessa forma:*

![alt text](image-2.png)