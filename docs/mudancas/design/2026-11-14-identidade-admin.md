# Design: identidade do admin

**Status:** vigente
**Data:** 2026-11-14
**Tarefa:** [admin #56](https://github.com/wmreis-labs/admin/issues/56)
**Decisão:** [ADR-20261114](https://github.com/wmreis-labs/airtestto/blob/develop/docs/arquitetura/adr/ADR-20261114-pools-por-sistema-admin-central.md)

## Objetivo

O operador da plataforma convida e administra as pessoas de Hostto, ATO e Zé Salgados. A tela mostra o que a API devolver. Não há cadastro público.

O caixa continua no login que já existe. Esta área usa outra sessão.

## Onde entra

| Rota | Quem entra | O que mostra |
| --- | --- | --- |
| `/identidade` | Quem já entrou no admin | O convite: e-mail da pessoa, o sistema e o tenant que recebe o acesso. A lista mostra o tenant de cada pessoa |

O menu do caixa ganha o grupo Identidade, item Acessos, no caminho `/identidade`.

## Entrada

A sessão que envia o convite passou para [Menu por grupo](2026-11-18-menu-por-grupo.md) em 2026-11-18. Sem a sessão de operador, Acessos pede esse login. O convite continua pedindo e-mail, sistema e tenant.

## Sistemas

A escolha vem de `GET /admin/systems`: nome, papéis, papéis que podem omitir tenant e atributos. A tela não inventa papel.

Um convite abre o sistema da aba. O campo Sistema não se repete no formulário. O tenant é a empresa do caixa, gravada em `custom:tenant`. O nome da empresa pode coincidir com o de um produto. O mesmo e-mail em outro sistema é outro convite.

## Pendentes

A fila é `GET /admin/systems/{appId}/users/pending`. Cada linha mostra nome, e-mail, tenant e situação que a API devolveu. Aprovar pede o papel do catálogo. O tenant é obrigatório quando o papel não está em `tenantOptionalGroups`. Campo vazio não vai no corpo.

## Ativos

A lista é `GET /admin/systems/{appId}/users`, sem quem ainda está em `PendingApproval`. Cada linha mostra nome, e-mail, papel do catálogo, tenant e situação. Hostto mostra document porque o catálogo traz esse atributo. ATO e Zé Salgados não.

Ajustar grava papel e tenant quando mudaram. Desativar, reativar e nova senha pedem confirmação. Pessoa desabilitada não recebe nova senha: o botão fica desligado e o texto diz isso.

## Estados

- **Catálogo indisponível.** O convite continua na tela, com Hostto, ATO e Zé Salgados para escolher o sistema.
- **Fila vazia.** Diz que ninguém aguarda aprovação.
- **Sem ativo.** Diz que quem foi aprovado aparece ali.
- **Erro.** A mensagem de `error.message` fica na tela.
- **Sucesso.** Snackbar com o que a ação fez.

## Fora deste desenho

O login do caixa, `/register` e `/confirm` continuam como estão. Esta tela não cria usuário direto no Cognito e não chama a API de negócio com o token do operador.
