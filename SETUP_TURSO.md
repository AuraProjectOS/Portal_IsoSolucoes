# Como ativar a persistência real dos dados (Turso)

## Por que isso é necessário

Este app guarda os cadastros num arquivo SQLite (`isopor_parceiro.db`) junto com o código. No Streamlit Community Cloud esse arquivo **não é permanente**: toda vez que o app reinicia (a cada merge no `main`, ou quando "dorme" por inatividade e acorda de novo), o arquivo volta a ser exatamente o que está salvo no GitHub — apagando qualquer cadastro feito só na versão rodando.

A partir desta atualização, o app pode usar o [Turso](https://turso.tech) — um banco compatível com SQLite (mesma linguagem que o app já usa) que roda na nuvem de verdade. Sem configurar nada, o app continua funcionando exatamente como antes (SQLite local). Com o Turso configurado, os dados passam a sobreviver a qualquer reinício.

## Passo a passo (uns 5 minutos)

### 1. Crie uma conta e um banco no Turso

1. Acesse **https://turso.tech** e clique em "Get Started" / "Sign Up".
2. Entre com sua conta do GitHub (mais rápido) ou e-mail.
3. No painel, clique em **"Create Database"**.
4. Dê um nome, por exemplo `isopor-parceiro`, e escolha a região mais próxima (ex: São Paulo / `gru` se disponível).
5. Depois de criado, clique no banco e procure por:
   - **Database URL** — algo como `libsql://isopor-parceiro-seuusuario.turso.io`
   - **Auth Token** (em "Create Token" ou similar) — uma string longa. Gere um token e copie ele imediatamente (alguns painéis só mostram uma vez).

Guarde essas duas informações — são o `TURSO_DATABASE_URL` e o `TURSO_AUTH_TOKEN`.

### 2. Configure os secrets no Streamlit Community Cloud

1. Acesse **https://share.streamlit.io** e entre na sua conta.
2. Abra o app do Portal IsoSoluções.
3. Clique em **⋮ (menu) → Settings → Secrets**.
4. Cole exatamente isto (substituindo pelos seus valores reais):

```toml
TURSO_DATABASE_URL = "libsql://isopor-parceiro-seuusuario.turso.io"
TURSO_AUTH_TOKEN = "seu-token-aqui"
```

5. Clique em **Save**. O app reinicia sozinho — a partir daí, todo cadastro passa a ser salvo também no Turso.

### 3. Como saber se funcionou

Depois do reinício, abra o app: no topo da barra lateral (sidebar) aparece um aviso:

- 💾 verde: **"Turso conectado — cadastros persistem na nuvem e sobrevivem a reinícios do app."** → tudo certo.
- ⚠️ amarelo: algo está errado (token inválido, URL errada, ou Turso não configurado ainda) — o app continua funcionando, mas ainda sem a proteção extra.

### 4. Rodando localmente (no seu computador)

Não precisa configurar nada — sem as variáveis `TURSO_DATABASE_URL`/`TURSO_AUTH_TOKEN`, o app usa o SQLite local normalmente, do jeito que sempre funcionou. Se quiser testar o Turso localmente, crie um arquivo `.streamlit/secrets.toml` na pasta do projeto com o mesmo conteúdo do passo 2 (esse arquivo não deve ser commitado no Git).

## Sobre os ~19 cadastros perdidos

Essa mudança evita que o problema aconteça de novo, mas infelizmente **não recupera os cadastros já perdidos**: eles só existiam na cópia do app rodando na nuvem e nunca foram salvos em nenhum lugar persistente. Procuramos por um backup (Excel exportado ou cópia do arquivo `.db`) no seu Google Drive e Gmail e não encontramos nada. Se você tiver algum arquivo salvo manualmente (pendrive, pasta do computador, e-mail antigo), ainda vale a pena checar — qualquer cópia do `isopor_parceiro.db` ou um Excel exportado do dashboard permite reimportar os clientes.
