# Design: conta no admin

**Status:** vigente
**Data:** 2026-10-18
**ADR:** [conta do bank no livro](https://wmreis-labs.github.io/wmreis-docs/mudancas/adr/2026-10-18-conta-bank-livro/)
**PRD:** [caixa pessoal e profissional](https://wmreis-labs.github.io/wmreis-docs/mudancas/prd/2026-09-27-caixa-pessoal-profissional/)

## Objetivo

Cadastrar a conta bancária no admin. A rota continua `/financeiro/contas`. O formulário envia um cadastro só para `POST /cashbook/contas` e a lista mostra a visão que `GET /cashbook/contas` devolver. A tela não grava no `bank` por conta própria e não monta agência, número nem livro no browser.

O contrato está em [Conta do bank](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-10-18-conta-bank-cashbook/).

## Fora deste desenho

O painel do mês, o upload e a revisão continuam em [Caixa](2026-09-27-caixa-admin.md). O sino continua em [Lembrete](2026-10-17-lembrete-admin.md). O Hostto não recebe este formulário. O snackbar continua avisando sucesso ou erro de uma ação.

## Formulário

| Campo | Quando |
| --- | --- |
| Livro | Sempre. Milton, TESTTO ou WMREIS |
| Nome | Sempre. É o nome que a lista mostra |
| Provedor | Sempre. O banco da conta |
| Produto | Conta ou cartão |
| Corrente ou poupança | Só quando o produto é conta |
| Agência e número | Só quando o produto é conta |
| Quatro últimos dígitos | Só quando o produto é cartão |
| Gerente e telefone | Opcionais |

Conta Inter da TESTTO ou da WMREIS mostra a marca de coleta automática que a API devolver. A marca não dispara a coleta.

## Lista

Cada linha mostra livro, nome, provedor, agência e número ou o final do cartão, produto, corrente ou poupança, e a situação. O filtro de livro usa o mesmo parâmetro da API.

## Estados

- **Vazio.** Nenhuma conta cadastrada. A tela diz isso e não mostra zero como mês fechado.
- **Erro.** Falha de API mostra o erro da resposta e mantém a lista anterior.
- **Conflito.** A mesma conta no mesmo provedor já existe. A tela mostra o erro e não cria outra linha.

## Riscos

A tela quebrar se passar a gravar agência no `cashbook` ou a chamar `/bank/accounts` direto. Rota diferente de `/financeiro/contas` atualiza este desenho, não um ADR.
