# Design: lembrete no admin

**Status:** vigente
**Data:** 2026-10-17
**ADR:** [lembrete antes do painel](https://wmreis-labs.github.io/wmreis-docs/mudancas/adr/2026-10-17-lembrete-caixa/)
**PRD:** [caixa pessoal e profissional](https://wmreis-labs.github.io/wmreis-docs/mudancas/prd/2026-09-27-caixa-pessoal-profissional/)

## Objetivo

Mostrar o lembrete no admin que já existe. O sino avisa o dia. A tela Contas a vencer é a gestão. Os dois leem `/cashbook` no desenho [lembrete](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-10-17-lembrete-cashbook/). A tela não recalcula contagem, grupo, valor nem vencimento.

## Fora deste desenho

Parser de e-mail, livro e a regra de encerrar ficam no backend. O painel do mês, o upload e a revisão continuam em [Caixa](2026-09-27-caixa-admin.md) e não bloqueiam este corte. O Hostto não recebe este módulo. O snackbar continua avisando sucesso ou erro de uma ação.

## Onde entra

O sino é o ícone de notificações da barra do layout logado. Ele aparece em qualquer página, inclusive fora de Caixa. O número é a contagem que `/cashbook/lembretes/sino` devolver.

A gestão entra no módulo Caixa. A rota é `/financeiro/vencer`.

## Sino

O clique abre um menu ancorado no ícone. Os grupos, nesta ordem, são atrasado, vence hoje e novo. Cada linha mostra livro, credor, valor e vencimento. O clique na linha abre a mesma cobrança em `/financeiro/vencer`.

Grupo vazio não aparece. Os três grupos vazios mostram uma linha: nenhuma conta para hoje.

## Tela

| A pessoa faz | O que a API grava |
| --- | --- |
| Confirmar como rotina | Dia do mês e situação ativo. Sai do grupo novo |
| Confirmar como avulso | Data de vencimento e situação ativo. Sai do grupo novo |
| Dar baixa nesta ocorrência | A rotina continua. Esta data deixa de alertar |
| Encerrar | Motivo obrigatório. A cobrança não volta sozinha |
| Ajustar dia ou valor | Vale para a próxima ocorrência |

Filtros: livro Milton, TESTTO ou WMREIS; natureza rotina ou avulso; janela de atrasados, hoje ou próximos 7 dias.

## Estados

- **Vazio.** A janela não tem cobrança. A tela diz isso e não mostra zero como mês fechado.
- **Novo.** A linha espera confirmação. Não entra como rotina sozinha na tela.
- **Erro.** Falha de API mostra o erro da resposta e mantém a lista anterior. A interface não completa vencimento nem valor em falta.
- **Sessão fechada.** O sino não avisa. O lembrete espera a próxima abertura do admin.

## Riscos

O ícone hoje mostra um número fixo. Este desenho quebra se o número continuar fixo ou se for contado no browser. Rota diferente de `/financeiro/vencer` atualiza este desenho, não um ADR.
