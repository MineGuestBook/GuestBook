# GuestBook — Requisitos Funcionais (revisado)

## Conta e Perfil

- **RF1** — O sistema deve permitir que o usuário crie uma conta.
- **RF2** — O sistema deve criar a página de comentários do usuário (seu "guestbook" pessoal).
- **RF3** — O sistema deve permitir que usuários editem os dados do próprio perfil (nome, foto, bio, etc.).
- **RF4** — O sistema deve permitir que o usuário exclua a própria conta.
- **RF5** — O sistema deve permitir alteração de senha (usuário autenticado).
- **RF6** — O sistema deve permitir recuperação de senha esquecida, via link/token enviado por email.
- **RF7** — O sistema deve permitir a busca de perfis de outros usuários.
- **RF8** — O sistema deve permitir a estilização da página de perfil.
- **RF9** — O sistema deve permitir a visualização do perfil e dos comentários sem necessidade de autenticação.

## Comentários

- **RF10** — O sistema deve permitir que qualquer visitante comente no perfil de outro usuário, sem necessidade de possuir conta.
- **RF11** — Ao comentar sem estar autenticado, o sistema deve permitir que o autor informe um nome de usuário livre ou deixe em branco, sendo exibido como "Anônimo" nesse caso.
- **RF12** — O sistema deve permitir que o dono do perfil exclua qualquer comentário em seu próprio perfil.
- **RF13** — O sistema deve permitir deixar "likes" em comentários.

## Integração e Distribuição

- **RF14** — O sistema deve disponibilizar um widget embutível (ex.: iframe ou script) para que o usuário insira seu guestbook em páginas de terceiros.

## Notificações

- **RF15** — O sistema deve notificar o usuário por email quando receber um novo comentário em seu perfil.

---

# Requisitos não Funcionais

- RNF1 — O sistema deve limitar a taxa de comentários por IP/sessão em um curto intervalo de tempo, para mitigar spam de comentários anônimos.
- RNF2 — O sistema deve suportar múltiplos comentários sendo publicados simultaneamente sem inconsistência ou perda de dados.
- RNF3 — O carregamento da página de perfil (com comentários) deve ocorrer em até X segundos, mesmo com um volume alto de comentários.
- RNF4 — O sistema deve sanitizar o conteúdo dos comentários para evitar ataques de XSS/injeção, já que qualquer visitante pode postar sem autenticação.
- RNF5 — A estilização customizada do perfil (RF8) não deve comprometer a segurança ou o layout de outras páginas do sistema.

---

## Requisitos gerais da disciplina (referência)

### Funcionais

- Aplicação cliente-servidor sobre plataforma Web.
- Frontend executado no navegador do cliente, com código baixado sob demanda.
- Backend na nuvem para atender às requisições do frontend.
- Documentação de API RESTful para comunicação frontend/backend.
- Acesso controlado por autenticação e autorização via provedores externos (Google, Apple, etc.).
- Persistência de dados de usuários em banco de dados.
- Documentação de modelagem de dados e arquitetura do sistema.
- Envio de email e notificações aos usuários.
- Registro (log) de todas as operações críticas para análise posterior.
- Cenários de desenvolvimento e produção.
- Implantação em nuvem com uso de IaC.
- Implantação automática em produção com uso de CI/CD.

### Não funcionais

- Boa responsividade.
- Baixa latência.
- Custo mínimo de operação.