# GuestBook — Resumo do Projeto

## Visão geral

O GuestBook é um livro de visitas na web, no estilo dos sites antigos de guestbook. Cada usuário cria uma conta e ganha uma página de perfil própria, que pode estilizar do jeito que quiser. Qualquer visitante, sem precisar de conta ou login, pode ver os comentários de qualquer perfil e deixar um comentário nele, informando um nome livre (ou ficando anônimo) e opcionalmente um email de contato.

A assimetria é o ponto central do projeto: **ter um perfil exige conta, comentar não**. A autenticação serve apenas para gerenciar o próprio perfil, nunca para participar.

## Atores

| Ator | Descrição |
|---|---|
| **Visitante** | Qualquer pessoa acessando o sistema, autenticada ou não. Visualiza perfis, comenta e curte comentários. |
| **Usuário** | Visitante autenticado, dono de uma conta e de um perfil. Gerencia o próprio perfil e exclui comentários recebidos. |
| **Sistema** | Dispara ações automáticas, como o envio de emails de notificação. |

## Funcionalidades

**Conta e perfil (exige autenticação)**
- Criar conta, com perfil criado automaticamente
- Editar dados do perfil e estilização da página
- Alterar senha e recuperar senha esquecida
- Excluir a conta, removendo perfil e comentários associados

**Comentários (não exige autenticação)**
- Comentar em qualquer perfil, com nome livre ou anônimo
- Email opcional, apenas como dado de contato
- Curtir comentários (um like por origem)
- Exclusão de comentários pelo dono do perfil

**Outros**
- Visualização pública de perfis e comentários, sem login
- Busca de perfis
- Widget embutível (iframe/script) para uso em sites de terceiros
- Notificação por email ao receber um novo comentário

## Decisões relevantes

- **Comentar é aberto.** Nenhuma etapa do fluxo de comentário exige conta, o que torna a prevenção de spam (rate limiting) e a sanitização de conteúdo requisitos não funcionais críticos.
- **Comentário anônimo é imutável.** Sem vínculo com uma conta, não há como validar autoria, então o autor não pode editar nem excluir o próprio comentário. Apenas o dono do perfil modera.
- **Exclusão de conta é destrutiva.** Remove perfil e comentários em cascata, sem estado intermediário.
- **Redefinição de senha não compara com a anterior.** Permitir senha igual evita que o formulário seja usado para inferir a senha atual do usuário.

## Fora do escopo

- Moderação avançada (bloqueio de usuários, denúncia de comentários)
