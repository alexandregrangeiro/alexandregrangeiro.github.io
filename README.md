# alexandregrangeiro.github.io

Site de write-ups (CTF, máquinas, prática de AppSec) publicado via GitHub Pages.

## Estrutura

```
index.md              → página inicial, lista os write-ups
_config.yml            → configuração do Jekyll
writeups/
  _template.md          → modelo pra copiar em cada novo write-up
  juice-shop/            → write-ups do OWASP Juice Shop
  ctf/                    → write-ups de CTF
  machines/               → write-ups de máquinas (HTB, THM, etc.)
```

## Como adicionar um novo write-up

1. Copia `writeups/_template.md` pra dentro da pasta certa (ex: `writeups/juice-shop/`)
2. Renomeia com um nome descritivo, ex: `xss-search-field.md`
3. Preenche o template
4. Adiciona o link em `index.md`, na seção correspondente
5. `git add . && git commit -m "add write-up: nome" && git push`

## Rodando localmente (opcional)

Se quiser ver o site antes de publicar:

```bash
gem install bundler jekyll
bundle exec jekyll serve
```

Acessa em `http://localhost:4000`.
