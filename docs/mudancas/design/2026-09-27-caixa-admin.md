# Design: módulo de caixa no admin

**Status:** vigente
**Data:** 2026-09-27
**ADR:** [fase 1 do caixa](https://wmreis-labs.github.io/wmreis-docs/mudancas/adr/2026-09-27-fase-1-caixa/)
**PRD:** [caixa pessoal e profissional](https://wmreis-labs.github.io/wmreis-docs/mudancas/prd/2026-09-27-caixa-pessoal-profissional/)

## Objetivo

Mostrar o caixa pessoal e o profissional dentro do admin que já existe, em `https://admin.testto.com.br/login` e `https://dev-admin.testto.com.br/login`. A tela não recalcula receita, despesa, resultado nem sobra. Esses números vêm de `/cashbook`, no desenho [cashbook](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-09-27-caixa-cashbook/).

## Fora deste desenho

Parser de OFX e CSV, pareamento, livros e a fórmula do mês ficam no backend. O Hostto não recebe este módulo. O dashboard atual do admin não passa a inventar série financeira: o painel deste módulo é outra tela.

## Onde entra

O menu principal hoje é Dashboard (`/dashboard`), Aplicativos (`/apps`), Usuários (`/users`), Grupos (`/groups`), Tenants (`/tenants`) e Logs (`/logs`). Configurações continua em `/settings`.

O módulo entra como mais um item desse menu, com o rótulo Caixa. A rota proposta é `/financeiro`. Ela não está travada num ADR: se o caminho mudar, este desenho é que se atualiza.

Login, layout autenticado e o cliente `admin/src/services/api/baseApi.ts` permanecem. Toda chamada manda `Authorization` e `x-tenant` para `REACT_APP_API_URL`.

## Telas

| Tela | O que a pessoa faz | Fase |
| --- | --- | --- |
| Painel | Vê, por livro e no consolidado, receita real, despesa real, resultado e a série dos meses. Interno, retirada, fatura, investimento, patrimonial e empréstimo aparecem ao lado | 1, e a reserva na fase 3 |
| Contas | Cadastra conta corrente e cartão nos livros Milton, TESTTO e WMREIS. As contas Inter da TESTTO e da WMREIS aparecem como coleta automática | 1 |
| Extrato | Envia OFX, CSV ou PDF. Mostra o que já entrou e o que foi para revisão | 1. Drive e Gmail aparecem como origem na fase 2 |
| Revisão | Escolhe um dos tipos do importador. Confirmar grava a regra pela descrição | 1 |
| Lançamento | Lança à mão receita, despesa, transferência ou aporte, com data de competência | 1 |
| Contas a vencer | Confirma rotina ou avulso, dá baixa, encerra ou ajusta dia e valor. O sino da barra abre o recorte do dia | Primeiro corte. O detalhe está em [Lembrete](2026-10-17-lembrete-admin.md) |
| Plano de reserva | Vê meta, reserva atual, quanto falta, aportes e sobra investível | 3 |

O painel responde se o mês melhorou e não bloqueia o lembrete. O lembrete também não espera o painel. Drive e o plano de reserva continuam nos cortes seguintes.

## Estados

- **Vazio.** Nenhuma conta cadastrada: o painel pede o cadastro das contas e não mostra zero como se fosse um mês fechado.
- **Sem livro.** Movimento de conta antiga sem livro aparece na revisão, não no resultado.
- **Revisão.** A fila lista transferência sem par, dois PIX do mesmo valor, conta sem livro e PDF. Confirmar grava a escolha. O painel só usa o que já foi classificado.
- **Erro.** Falha de upload ou de API mostra o erro da resposta e mantém a tela anterior. A interface não completa o número em falta.
- **Google indisponível.** Sem credencial no ambiente da API, Drive e Gmail aparecem como indisponíveis. O upload segue habilitado.

## Riscos

A rota `/financeiro` é proposta. O menu não esconde Dashboard, Aplicativos nem o restante da administração. Número de resultado exibido fora de `/cashbook` quebra este desenho.
