# SQL Injection no login

**Categoria:** SQL Injection
**Dificuldade:** Fácil

## Contexto

Em `/#/login`, somos apresentados ao campo de login do site. Ao enviar o
usuário como uma aspas simples (`'`) no campo de usuário, percebemos uma
exception com a qual a página não lida. Isso sugere a possibilidade de
uma injection.

## Vulnerabilidade

O campo de Email parece concatenar o input diretamente numa query
SQL sem sanitização adequada, permitindo que caracteres de controle SQL
(como aspas simples) quebrem a estrutura da query e executem outras coisas
no lugar.

## Exploração

Payload usado no campo de login:

    ' OR 1=1--

Qualquer combinação de caracteres funciona no campo de senha, já que o
payload comenta o restante da query antes da checagem de senha ser avaliada.

## Resultado

Somos autenticados como `admin@juice-sh.op`, presumivelmente o primeiro
usuário da tabela SQL, ou seja, bypass completo da autenticação;

## Remediação

Usar prepared statements / queries parametrizadas em vez de concatenar
input diretamente na query SQL, e validar/sanitizar o input do campo de
login antes de qualquer processamento.

---

*Write-up feito em 24/09/2026*
