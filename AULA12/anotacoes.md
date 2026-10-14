## AULA 12 - Normalização de Banco de Dados

- **Objetivo:** Normalizar um *banco de dados* com problemas de organização de registros.

---
### 1FN

> **1** - Primeiro, criamos um *banco de dados*:
```sql
CREATE DATABASE empresa_moretti;
```

> **2** - Criamos a tabela *original*:
```sql
CREATE TABLE cadastro_original (
    funcionario_id INTEGER PRIMARY KEY,
    nome TEXT,
    setor_id INTEGER,
    setor_nome TEXT,
    treinamentos TEXT
);
```

> **3** - Inserimos os dados na tabela:
```sql
INSERT INTO cadastro_original VALUES
(1, 'Mayne', 10, 'Montagem', 'Seguranca;Ferramentas'),
(2, 'Miguel', 20, 'Manutencao', 'Seguranca'),
(3, 'Lucas', 10, 'Montagem', 'Ferramentas');
```

> **4** - Ao realizar a primeira consulta, verificamos que a *Mayne* está com dois registros na mesma linha! *Isso não pode acontecer!*

![alt text](image-5.png)

```sql
SELECT * FROM cadastro_original ORDER BY funcionario_id;
```

> **5** - Criamos uma *nova tabela* com a **1FN**:
```sql
CREATE TABLE cadastro_1fn AS
SELECT funcionario_id, nome, setor_id, setor_nome,
    string_to_table(treinamentos, ';') AS treinamento_tratado
FROM cadastro_original;
```

> **6** - Atribuímos o `funcionario_id` ao `treinamento_tratado` 1FN:
```sql
ALTER TABLE cadastro_1fn
ADD PRIMARY KEY (funcionario_id, treinamento_tratado);
```

> **7** - Resultado após a ***normalização***:
```sql
SELECT * FROM cadastro_1fn
ORDER BY funcionario_id, treinamento;
```

![alt text](image-6.png)

---
### 2FN

> **1** - Primeiro, criamos a tabela ***funcionários***:
```sql
CREATE TABLE funcionario AS
SELECT DISTINCT funcionario_id, nome, setor_id, setor_nome
FROM cadastro_1fn;
```
- *O `DISTINCT` evita copiar linhas iguais mais de uma vez. Ou seja, os dois treinamentos da Mayne viram um só.*

> **2** - O comando `CREATE TABLE AS` não traz as chaves primárias automaticamente. Dessa forma, executamos o comando:
```sql
ALTER TABLE funcionario
ADD PRIMARY KEY (funcionario_id);
```

> **3** - Criamos a tabela ***participações***, trazendo apenas o *ID do funcionário* e os seus *treinamentos*:
```sql
CREATE TABLE participacoes AS
SELECT funcionario_id, treinamento_tratado
FROM cadastro_1fn;
```

> **4** - Adicionando as chaves primárias:
```sql
ALTER TABLE participacoes
ADD PRIMARY KEY (funcionario_id, treinamento_tratado);
```

> **5** - Adicionando a chave estrangeira (relações entre tabelas):
```sql
ALTER TABLE participacoes
ADD FOREIGN KEY (funcionario_id)
REFERENCES funcionario (funcionario_id);
```

> **6** - Verificando a **2FN**:
```sql
SELECT * FROM funcionario;
SELECT * FROM participacoes;
```

![alt text](image-7.png)

---
### 3FN

- **Objetivo:** Separar os setores.

---
> **1** - Criamos a tabela ***setores***:
```sql
CREATE TABLE setores AS
SELECT DISTINCT setor_id, setor_nome
FROM funcionario;
```

> **2** - Adicionando a chave primária:
```sql
ALTER TABLE setores
ADD PRIMARY KEY (setor_id);
```

> **3** - Criando a chave estrangeira:
```sql
ALTER TABLE funcionario
ADD FOREIGN KEY (setor_id) REFERENCES setores (setor_id);
```

> **4** - Correção do erro no momento da criação da tabela:
```sql
ALTER TABLE funcionario
ALTER COLUMN setor_id SET NOT NULL;
```

> **5** - Apagamos a coluna `setor_nome`, uma vez que tiramos essa informação da tabela ***funcionário*** **3FN**:
```sql
ALTER TABLE funcionario
DROP COLUMN setor_nome;
```

> **6** - Consultando as tabelas novas:
```sql
SELECT * FROM funcionario;
SELECT * FROM participacoes;
SELECT * FROM setores;
```

![alt text](image-8.png)

> **7** - Vantagem para atualizar *treinamentos, funcionários, setores, etc...*
```sql
UPDATE setores
SET setor_nome = 'Eletrica'
WHERE setor_id = 10;
```

![alt text](image-9.png)