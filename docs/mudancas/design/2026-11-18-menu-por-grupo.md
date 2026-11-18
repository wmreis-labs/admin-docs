# Design: menu por grupo do operador

**Status:** vigente
**Data:** 2026-11-18
**Tarefa:** [admin #61](https://github.com/wmreis-labs/admin/issues/61)
**História:** [admin #60](https://github.com/wmreis-labs/admin/issues/60)

## Objetivo

O menu do admin segue os grupos da sessão de operador. O convite usa essa sessão, no pool do host em que a tela está aberta.

## Pool

| Host | Client | API |
| --- | --- | --- |
| `admin.testto.com.br` | `hninnb3mn82nbpdgcfbav2pvc` | `https://api.testto.com.br/platform-admin` |
| Qualquer outro | `5pqcis5lkvfg68cj0jovhboij6` | `https://dev-api.testto.com.br/platform-admin` |

`REACT_APP_PLATFORM_CLIENT_ID` e `REACT_APP_PLATFORM_API_URL`, quando existem no build, substituem a linha do host.

## Entrada

Acessos sem sessão de operador mostra o login dessa conta. A chamada de identidade leva só o access token dessa sessão.

O login do caixa não muda. Sem a sessão de operador, o menu do caixa continua inteiro.

## Menu

Com a sessão aberta, o item some e a URL direta volta para a primeira área permitida.

| Grupo | Áreas |
| --- | --- |
| `OperadorPlataforma` | Caixa, Administração e Identidade |
| `Caixa` | Início e as telas de `/financeiro` |
| `Administracao` | Usuários, grupos, tenants e logs |
| `Identidade` | Acessos |

Configurações permanece para quem já entrou.
