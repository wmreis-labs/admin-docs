# Design: shell BC-like e páginas do caixa

**Status:** proposta (draft PR)
**Data:** 2026-10-22
**Corte:** A + B parcial (OK Milton) — shell + Role Center Caixa + List Revisão/Extrato
**Referência de produto:** [casca vigente](2026-10-18-casca-do-admin.md) · [caixa](2026-09-27-caixa-admin.md)

## Objetivo

Remodelar a casca do admin e as telas do caixa para dinâmica **skin-like** de ERP Role Center / List page (padrões públicos de Business Central / Learn). Identidade visual **WMReis / TESTTO**. A API `/cashbook` não muda. O Hostto e o razão de imóveis ficam fora.

## Posicionamento legal / marca

- Copy e UI: “inspirado em Role Center / List page de ERP”, **não** “Business Central” como marca na interface.
- Zero logo Dynamics, zero ícones proprietários Microsoft, zero assets de marketing Microsoft.
- Tokens, tipografia e marca própria (abaixo).

## Tokens

| Token | Papel | Valor |
| --- | --- | --- |
| `--wm-appbar` | App bar | `#0f2a3d` |
| `--wm-accent` | Links, selected, cues | `#0b6e4f` |
| `--wm-accent-soft` | Hover / cue bg | `#e6f4ef` |
| `--wm-surface` | Fundo da página | `#f3f2f1` |
| `--wm-panel` | Painéis / listas | `#ffffff` |
| `--wm-border` | Divisores | `#e1dfdd` |
| `--wm-text` | Corpo | `#242424` |
| `--wm-text-muted` | Secundário | `#605e5c` |
| `--wm-row-h` | Densidade de linha | `32–36px` |
| `--wm-font` | UI | `"Segoe UI", "Source Sans 3", system-ui, sans-serif` |

Densidade: corpo 13–14 px; título de página 18–20 px; padding de célula 6–8 px. Evitar `Paper` generoso (`p: 3`) em listas.

Stack: React + MUI 5. **Não** migrar para Fluent UI neste corte. Layouts e chrome com MUI + CSS tokens.

## Shell (fase A)

### App bar

Altura ~40 px, fundo `--wm-appbar`. Da esquerda para a direita:

1. Marca TESTTO (mark + “admin · caixa”)
2. Seções horizontais: **Caixa** · **Administração** (trocam o menu lateral / ênfase)
3. Campo **Diga-me…** (stub desabilitado — Tell Me real é fase C)
4. Seletor de **Livro** (Consolidado / Milton / TESTTO / WMREIS) — preferência local; não inventa dado de API
5. Sino de lembrete (componente já existente)
6. Avatar / menu Perfil · Configurações · Sair

### Navegação

- Painel esquerdo colapsável (pin local, ~220 px → ~48 px, só ícones).
- Grupos: Caixa e Administração com os itens já existentes (Painel, Contas, Extrato, Revisão, Lançamento, A vencer; Início, Usuários, Grupos, Tenants, Logs).
- Item novo: **Role Center** / Início do caixa em `/financeiro`.
- Em rotas `/financeiro/*`, barra **embedding** sob o chrome: Início · Revisão · Extrato · Lançamento · Contas · A vencer · Painel.

### Breadcrumbs

Sempre no conteúdo: `Caixa › Revisão`, `Administração › Usuários`, etc.

### Fora do shell neste corte

Redesenho profundo de Usuários/Grupos/Tenants/Logs além do encaixe na área de conteúdo. Personalize. Atalhos de teclado avançados.

## Role Center Caixa (fase B parcial)

Rota: `/financeiro` (home do perfil caixa). Painel detalhado permanece em `/financeiro/painel`.

| Parte | Conteúdo | Fonte |
| --- | --- | --- |
| Headline | Frase do mês + livro selecionado | `/cashbook/painel` |
| Data cues | Resultado · Entradas · Saídas | painel |
| Cue Em revisão (N) | Contagem da fila | `/cashbook/revisao` |
| Cue A vencer (N) | Contagem do sino | `/cashbook/lembretes/sino` |
| Action cues | Enviar extrato · Revisar · Lançar · Contas | rotas existentes |
| ListPart | Próximos vencimentos (amostra) | lembretes `janela=7dias` ou sino |

Sem conta / sem dado: mensagem e caminho para Contas — sem zero inventado.

## List pages — Revisão e Extrato

### Comum

- Título de página + meta (ex.: “7 em aberto”).
- **Action bar**: ações promoted à esquerda; menus Ações / Navegar à direita (secundárias podem ser stub).
- Toolbar: search na lista + toggle de filtro simples (client-side neste corte).
- Tabela densa (`size="small"`, altura de linha ~34 px).
- **FactBox** à direita (~280 px), colapsável: contexto da linha selecionada (descrição, motivo/regra, conta/livro/status). Sem FactBox se nada selecionado.

### Revisão

- Ações: Confirmar (existente), alterar tipo/livro na linha.
- Filtros locais: motivo, livro (sobre a lista já carregada).
- API: `GET/POST /cashbook/revisao*` inalterada.

### Extrato

- Promoted: **Novo upload** (formulário / escolher arquivo).
- Lista de envios da sessão (como hoje) + FactBox do lote selecionado (conta, sha256, situação parse).
- Upload via bank inalterado.

## Contas / Lançamento / Painel

Opcional no mesmo corte: só alinhar título + action bar mínima para não destoarem do shell. Sem FactBox obrigatório agora.

## Estados

- **Erro de API:** mensagem na página; lista anterior permanece.
- **Lista vazia:** texto claro, sem cards decorativos.
- **Tell Me:** desabilitado com indicação de fase futura.
- **Livro na app bar:** preferência de UI; cada tela continua podendo ter filtro próprio até unificar.

## Aceite do 1º corte

1. Shell reconhecível vs. protótipo (app bar, sections, collapse, embedding, breadcrumbs, tokens TESTTO).
2. `/financeiro` funciona como Role Center com cues reais da API.
3. Revisão e Extrato com action bar + search/filter + FactBox.
4. Nenhuma chamada nova que invente número; contratos `/cashbook` preservados.
5. Sem menção de marca Microsoft na UI; sem merge/prod sem Milton.
