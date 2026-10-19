# Design: casca do admin

**Status:** vigente
**Data:** 2026-10-18
**RFC:** [menu e layout do admin](https://wmreis-labs.github.io/wmreis-docs/mudancas/rfc/2026-10-18-menu-do-admin/)
**PRD:** [console do admin](https://wmreis-labs.github.io/wmreis-docs/mudancas/prd/2026-10-18-console-do-admin/)

## Objetivo

Organizar o menu e a entrada do admin. A pessoa vê Administração e Caixa como áreas. A entrada não inventa número. As telas de caixa que já existem ganham hierarquia visual, sem mudar o que a API devolve.

## Fora deste desenho

Extrato, revisão e lançamento continuam no desenho [Caixa](2026-09-27-caixa-admin.md). O Hostto e o Zé Salgados não recebem este menu.

## Onde entra

O layout autenticado que já existe. A marca Testto permanece no topo do menu. O seletor Hostto, Zé Salgados e Admin sai da barra.

O menu passa a ter dois rótulos de grupo. O item selecionado é o caminho exato da tela, para um grupo não acender o outro.

| Grupo | Item | Rota | Situação |
| --- | --- | --- | --- |
| Administração | Início | `/dashboard` | Existe, com dado local. Esta proposta troca o conteúdo |
| Administração | Usuários | `/usuarios` | Sem tela. A API já lista usuário e grupo da sessão |
| Administração | Grupos | `/grupos` | Sem tela. A API já lista grupo |
| Administração | Tenants | `/tenants` | Sem tela. A API já lista e grava tenant |
| Administração | Logs | `/logs` | Sem tela. A API já lista evento de auditoria |
| Caixa | Painel | `/financeiro/painel` | Existe |
| Caixa | Contas | `/financeiro/contas` | Existe |
| Caixa | Contas a vencer | `/financeiro/vencer` | Existe |

Configurações continua em `/settings`, no rodapé, ao lado de Sair. O avatar abre Perfil e Configurações. Perfil ainda não tem tela: até lá, o item leva a `/settings`. Aplicativos sai do menu. Não há rota `/apps`.

As rotas `/users` e `/groups` do menu atual não têm tela. As rotas novas são `/usuarios` e `/grupos`, para não herdar o caminho vazio.

## Entrada

A pergunta da tela é o que a administração tem para mostrar hoje. Enquanto a leitura não existir, o texto é: a entrada ainda não lê a API. Dois caminhos: Administração e Caixa. Sem card de pedido, cliente, produto, entrega ou gráfico.

Quando a leitura existir, a tela mostra só contagens que a API devolver, cada uma com o caminho da lista. Sem contagem, o bloco some.

## Caixa já na tela

O painel, as contas e as contas a vencer continuam nas rotas atuais. O ajuste é de composição.

- Uma linha de título diz a pergunta da tela. No painel: o resultado do mês. Em contas: as contas dos livros. Em contas a vencer: o que precisa de ação.
- Livro e mês, quando existirem, ficam na mesma linha do título.
- No painel, resultado é o número grande. Receita e despesa ficam menores, na mesma faixa. O bloco ao lado do resultado usa rótulo menor e não repete o tamanho do resultado.
- A série de doze meses continua em tabela. O mês pedido fica marcado. Sem gráfico neste corte.
- Sem conta no livro, a tela inteira pede o cadastro. Os zeros não aparecem em volta do pedido.
- Em contas, a lista vem primeiro. Cadastrar abre o formulário por cima da lista e fecha ao gravar. O formulário não empurra a tabela para baixo.
- Erro de API mantém a lista anterior e mostra a mensagem. Sucesso e erro de ação continuam no snackbar.

## Usuários

A tela lista as pessoas que a API devolver e o grupo de cada uma. A pessoa abre uma linha e ajusta os grupos. A tela não cria senha nem confirma cadastro: isso continua em `/register` e `/confirm`.

Antes desta tela, a leitura e a gravação de usuário e grupo passam a exigir a mesma sessão das outras rotas protegidas.

## Grupos

A tela lista os grupos e quem está em cada um. A edição do vínculo continua na tela de usuários. Esta tela não inventa permissão que a API não tenha.

## Tenants

A tela lista nome e situação. Incluir pede nome. Alterar muda nome ou situação. Excluir pede confirmação. A tela mostra o que a API gravou. Não troca o tenant da sessão aberta.

## Logs

A tela pede a referência que a API exige e lista os eventos dessa referência, em ordem de chegada. Cada linha mostra a data, o tipo e o domínio que a API devolveu. Sem outro filtro. Sem referência, a tela não consulta. Sem evento, diz que não há evento.

## Estados

- **Item sem tela.** Não aparece como se funcionasse. Até a história daquele item, o menu pode omitir o atalho. O quadro guarda a história.
- **Entrada vazia.** Texto e dois caminhos, sem zero e sem gráfico.
- **Erro.** A mensagem da API fica na tela. A lista anterior permanece.
- **Sessão.** Usuário, grupo, tenant e log usam o mesmo cliente das outras telas, com `Authorization` e `x-tenant`.
