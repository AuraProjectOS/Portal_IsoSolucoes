# Changelog

Todas as versões do Dashboard do Programa Parceiro Isopor (IsoSoluções).

## v0.4 — 2026-07-17

### Limpeza visual
- **Remoção de todos os emojis da interface.** Todos os emojis foram retirados das telas do dashboard, do portal do cliente e das mensagens de WhatsApp/relatórios, deixando a aparência mais sóbria e profissional. Onde o emoji servia como indicador (status de aviso, tipo de movimentação), ele foi substituído por rótulos de texto (ex.: `[Ativo · Clientes]`, `[Compra]`).
- **Correção de código HTML visível na tela.** Removida a meta tag de viewport injetada via `st.markdown` (que o Streamlit exibia como texto cru) e os wrappers `<div class="chart-container">` que eram abertos e fechados em chamadas separadas, fazendo com que `</div>` aparecesse como texto solto na página.

## v0.3 — 2026-07-17

### Novas funcionalidades
- **🎂 Data de aniversário do cliente.** Agora é possível vincular a data de aniversário ao cadastrar um cliente (campo opcional) e ela aparece no card do cliente com idade e contagem de dias até o próximo aniversário.
- **✏️ Edição de cliente.** Novo expander "Editar Cliente" no painel do cliente selecionado permite corrigir nome, telefone e data de aniversário a qualquer momento.
- **🛠️ Corrigir ou remover lançamentos de compra.** Para casos de erro de digitação, cada compra registrada pode ter sua quantidade de pacotes, valor e data corrigidos — ou o lançamento inteiro pode ser removido. Os pontos e o total de pacotes são recalculados automaticamente.

## v0.2 — 2026-07-01

### Correção crítica
- **Persistência real dos dados via Turso (opcional, recomendado).** Antes, os cadastros ficavam só num SQLite local que era resetado toda vez que o Streamlit Community Cloud reiniciava o app — causando perda de clientes cadastrados. Agora, se `TURSO_DATABASE_URL`/`TURSO_AUTH_TOKEN` forem configurados (veja `SETUP_TURSO.md`), os dados são sincronizados com um banco persistente na nuvem a cada gravação e sobrevivem a qualquer reinício. Sem essa configuração, o comportamento continua idêntico ao de antes (SQLite local), com um aviso visível na sidebar indicando o estado da persistência.

## v0.1 — 2026-06-25

Primeira versão no ar. 🚀

### Funcionalidades
- **Dashboard administrativo** completo: KPIs, gráficos, gestão de clientes, histórico e exportação para Excel.
- **Cartão de Fidelidade (PNG)** gerado sobre o `card_template.png`:
  - Cartão de compra com pontos ganhos, pílulas de Compra/Saldo e barra de progresso real até 500.
  - Cartão de marco ao atingir 500 pontos (recompensa Cafeteira).
  - Título neon **Cartão Fidelidade** abaixo do logo ("Cartão" em vermelho, "Fidelidade" em teal).
- **Aviso automático de WhatsApp** após cada compra, com mensagem pronta e download do cartão.
- **Alterações Manuais** com seções de alto impacto (mensagens, meta/recompensa, nome, regras, automação) e **histórico completo** de cada alteração.
- **Importação de clientes via Excel** com template pronto.
- **Portal do cliente** com link exclusivo.
- Timestamps em **horário de Brasília (UTC-3)**.
- **Reboot automático** do Streamlit Community Cloud a cada merge no main.
