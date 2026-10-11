# Design: cadastro no admin

**Status:** vigente
**Data:** 2026-10-11

## Objetivo

A rota `/register` do admin mostra o mesmo formulário de cadastro do Hostto. A pessoa cria a conta e segue para a confirmação do código.

## Fora deste desenho

Login e confirmação do código ficam como estão. Esta tela não cria pessoa, imóvel nem permissão. Nome, telefone e documento seguem no cadastro e ficam nos atributos do usuário, no desenho [atributos do cadastro](https://wmreis-labs.github.io/airtestto-docs/mudancas/design/2026-10-11-atributos-cadastro/).

## Onde entra

`/register`, no layout de autenticação que já existe. O cartão usa a marca Testto, como o login do admin.

## O que a pessoa faz

Informa nome completo, documento, e-mail, telefone, senha e a confirmação da senha. O telefone pode ficar vazio. Marca que leu e aceitou os termos. Cadastrar segue para `/confirm` com o e-mail informado. O link Entrar volta para `/login`.

## Estados

- **Incompleto.** Falta nome, documento, e-mail, senha ou a confirmação: a tela pede os campos obrigatórios e não envia.
- **Senha diferente.** A confirmação não repete a senha: a tela avisa e não envia.
- **Termos.** Sem o aceite, a tela pede o aceite e não envia.
- **Erro.** A API recusa o cadastro: a tela mostra a mensagem recebida e mantém o que foi digitado.
- **Enviando.** O botão fica desabilitado e indica que o cadastro está em andamento.
