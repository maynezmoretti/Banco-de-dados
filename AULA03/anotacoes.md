## SGBD
Instalar e configurar o SGBD PostgreSQL.

Comando para instalar o SGBD:
```bash
sudo apt install -y postgresql
```
>OBS: O comando sudo, no nosso caso, pode ser omitido pois já somos root.
---
Realizando verificação do SGBD:
```bash
pg_lsclusters
```

Para realizar o acesso ao SGBD **sem senha**, utilizar o comando:
```bash
sudo -u postgres psql
```
>Com esse comando o acesso é feito sem senha, pois o Linux já provou quem você é (root). Autenticação PEER.

Para primeiro acesso, alterei a senha:
```sql
ALTER USER postgres PASSWORD '250318';
```
>O retorno correto é `ALTER ROLE`.

Para sair do postgres, o comando é `\q` (igual o '\quit' de vários jogos).

```mermaid
graph LR
A[sudo -u postgres psql] --<b>Autenticação</b>--> B[Só funciona entrando dentro do próprio Linux - Ubuntu, Debian.]
```

```mermaid
graph LR
A[sudo psql -h 127.0.0.1 -U postgres] --<b>Autenticação</b>--> B[Funciona vindo de qualquer máquina, porém é necessário inserir a senha.]
```
---
## Configurações de Serviço
>Caminho padrão para as configurações do POSTGRESQL.

![alt text](image-1.png)

**Primeira configuração:**
```bash
sudo nano postgresql.conf
```
'CTRL + W' para buscar a linha do 'listen_addresses' e descomentamos, alterando para `*`.

Se ficar 'localhost', somente o meu PC acessa.

---
**Segunda configuração:**
```bash
sudo nano pg_hba.conf
```
Nas últimas linhas (CTRL + Fn + End), adicionei: `host - all - all - 10.87.38.0/24 - scram-sha-256`.

Para criar um banco de dados, usamos o comando:
```sql
CREATE DATABASE lojamax;
```
Para visualizar os bancos, usamos:
```bash
\l
```