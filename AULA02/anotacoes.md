## Servidor de Desenvolvimento
Será uma interface de desenvolvimento, utilizada para projetar aplicações e banco de dados.

```mermaid
graph LR
A[Cliente: Computador] <--Dados--> B[Servidor: Datacenter]
```
---
## Servidor de Arquivos Educacional
É um servidor para armazenar arquivos e facilitar na hora de realizar a transferência.
>O endereço para acesso ao servidor de arquivos é: `\\10.87.36.10`

>Credenciais de acesso: `Email: "aluno"`; `Senha: "aluno"`

---
## Servidor Pessoal
O Moba será a interface para acesso ao meu servidor de desenvolvimento.
>O acesso será realizado via SSH.

>Credenciais de acesso: `IP -> 192.168.10.97`; `Username -> root`; `Porta -> 2222`

Para o primeiro acesso, utilizamos a senha: `"aluno01"`.
Para alterar a senha, utilizamos o comando:
```bash
passwd
```

Para visualização do uso de recursos, utilizamos o comando:
```bash
htop
```

---
## Recursos e Configurações
|Recurso|Configuração|
|-------|------------|
|Processador|2 cores|
|RAM|512 MB|
|Armazenamento|6 GB|
|Sistema Operacional|Ubuntu 26.04 LTS|

---
## Objetivos Esperados
A utilização de um servidor de desenvolvimento, simula um ambiente real de produção.
Os objetivos esperados são:
- Deploy de projetos;
- Aplicação de banco de dados;
- Experiência real de mercado.

---
## BANCO DE DADOS
Antigamente, os dados eram salvos em arquivos/planilhas.

```mermaid
graph TD
P[Guardar dados] --> A[Arquivo de texto]
P[Guardar dados] --> B[Banco de dados]
A --> A1[Um usuário por vez]
A --> A2[Não possui backup]
A --> A3[Detalhes importantes ocultos]
B --> B1[Vários acessos simultâneos]
B --> B2[Consultas mais eficientes]
B --> B3[Controle de acesso]
B --> B4[Backup instantâneo]
```
---
>Mas afinal, onde entra o Banco de Dados em aplicações WEB? 🤔

```mermaid
graph LR
A[Usuário] --> B[Aplicação WEB] --> C[(Banco de Dados)]
```
---
## SGBD
Sistema Gerenciador de Banco de Dados.

>Função: Gerenciar, controlar e permitir consultas nos nossos bancos de dados.

```mermaid
graph TD
A[SGBD - PostGreSQL] --> B[(Banco de dados)]
A --> C[Armazena usuários]
A --> D[Realiza consultas]
A --> E[Controla acessos]
```