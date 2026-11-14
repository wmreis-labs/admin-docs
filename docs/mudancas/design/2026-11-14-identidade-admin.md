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
| `/identidade/acesso` | Público desta área | E-mail e senha. Sem link de cadastro |
| `/identidade` | Operador com sessão desta área | Os três sistemas, a fila e os ativos |

O menu do caixa ganha o grupo Identidade, item Acessos, no caminho `/identidade`. Quem já entrou no caixa vê o atalho. Quem só tem o pool da plataforma usa a barra desta área, com Sair. Se a sessão do caixa também estiver aberta, a barra mostra Caixa.

## Entrada

O texto diz que o acesso é por convite e que o primeiro login troca a senha temporária. Não há botão cadastrar, confirmar conta nem esqueci a senha.

Se o pool pedir a troca da senha temporária, a mesma tela pede a nova senha e a confirmação. A senha precisa de 8 caracteres, com maiúscula, minúscula e número. As duas têm de coincidir. Sem essa troca, a área não abre.

Conta sem o grupo de operador da plataforma não grava sessão. A mensagem fica na tela.

## Sistemas

A escolha vem de `GET /admin/systems`: nome, papéis, papéis que podem omitir tenant e atributos. A tela não inventa papel.

Um convite abre o sistema escolhido. O mesmo e-mail em outro sistema é outro convite.

## Pendentes

A fila é `GET /admin/systems/{appId}/users/pending`. Cada linha mostra nome, e-mail, tenant e situação que a API devolveu. Aprovar pede o papel do catálogo. O tenant é obrigatório quando o papel não está em `tenantOptionalGroups`. Campo vazio não vai no corpo.

## Ativos

A lista é `GET /admin/systems/{appId}/users`, sem quem ainda está em `PendingApproval`. Cada linha mostra nome, e-mail, papel do catálogo, tenant e situação. Hostto mostra document porque o catálogo traz esse atributo. ATO e Zé Salgados não.

Ajustar grava papel e tenant quando mudaram. Desativar, reativar e nova senha pedem confirmação. Pessoa desabilitada não recebe nova senha: o botão fica desligado e o texto diz isso.

## Estados

- **Sem sessão.** `/identidade` volta para `/identidade/acesso`.
- **Fila vazia.** Diz que ninguém aguarda aprovação.
- **Sem ativo.** Diz que quem foi aprovado aparece ali.
- **Erro.** A mensagem de `error.message` fica na tela. A sessão inválida volta para o acesso.
- **Sucesso.** Snackbar com o que a ação fez.

## Fora deste desenho

O login do caixa, `/register` e `/confirm` continuam como estão. Esta tela não cria usuário direto no Cognito e não chama a API de negócio com o token do operador.
