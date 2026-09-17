# GuestBook

Um livro de visitas (guestbook) na web, no estilo dos sites antigos: cada usuário cria uma conta e ganha uma página de perfil própria, que pode estilizar do jeito que quiser. Qualquer visitante, sem precisar de conta ou login, pode ver os comentários de qualquer perfil e deixar um comentário nele, informando um nome livre (ou ficando anônimo) e opcionalmente um email de contato.

O dono do perfil pode excluir comentários no seu próprio perfil. A conta autenticada serve só para gerenciar o próprio perfil (criar, editar, trocar/recuperar senha, excluir). O sistema também notifica por email quando alguém recebe um novo comentário, e oferece um widget embutível para o usuário colocar seu guestbook em outros sites.

> **Status:** projeto em fase de especificação. Ainda não há código implementado.

## Documentação

| Documento | Descrição |
|---|---|
| [Requisitos](./GuestBook-RFs.md) | Requisitos funcionais, não funcionais e regras de negócio |
| [Casos de Uso](./GuestBook-CasosDeUso.md) | Descrição textual dos casos de uso (UC01–UC11) |
| [Diagrama de Casos de Uso](./GuestBook-UseCaseDiagram.md) | Diagrama geral de atores e casos de uso |
| [Fluxos individuais](./casos-de-uso/) | Um fluxograma por caso de uso |

## Atores

- **Visitante** — qualquer pessoa acessando o sistema, autenticada ou não. Pode visualizar perfis, comentar e curtir comentários.
- **Usuário** — visitante autenticado, dono de uma conta e de um perfil. Gerencia o próprio perfil e modera os comentários que recebe.
- **Sistema** — dispara ações automáticas, como o envio de emails de notificação.

## Escopo

**Incluído:**

- Criação e gerenciamento de conta e perfil
- Página de perfil pública e estilizável
- Comentários sem necessidade de autenticação (identificados ou anônimos)
- Likes em comentários
- Exclusão de comentários pelo dono do perfil
- Widget embutível para uso em sites de terceiros
- Notificação por email de novos comentários

**Fora do escopo:**

- Moderação avançada (bloqueio de usuários, denúncia de comentários)
- Edição ou exclusão de comentários pelo próprio autor

## Stack

A definir.

---
