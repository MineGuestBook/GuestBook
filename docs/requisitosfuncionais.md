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

# Regras de Negócio

## Conta e Perfil

- **RN1** — Cada conta possui exatamente um perfil/página de comentários (RF2), criado automaticamente no momento do cadastro (RF1).
- **RN2** — O nome de usuário/identificador do perfil deve ser único no sistema, pois é usado para localizar o perfil (RF7) e montar a URL pública dele (RF9).
- **RN3** — Ao excluir a conta (RF4), o perfil e todos os comentários associados a ele (recebidos e, se houver, os feitos por essa conta em outros perfis) são excluídos permanentemente.
- **RN4** — O link de recuperação de senha (RF6) deve ter validade limitada e só pode ser usado uma vez.
- **RN5** — A redefinição de senha (RF5/RF6) pode resultar em uma senha igual à anterior; o sistema não deve comparar ou bloquear com base na senha antiga, pois isso poderia ser explorado para descobrir a senha atual do usuário.

## Comentários

- **RN6** — Comentar em um perfil (RF10) nunca exige autenticação — autenticação só é necessária para criar, editar ou excluir a própria conta/perfil (RF1, RF3, RF4, RF5, RF6). Todo comentário está associado apenas ao perfil de destino, nunca a uma conta do autor.
- **RN7** — Ao comentar (RF11), o nome de usuário e o email são ambos campos livres e opcionais; se o nome for deixado em branco, o comentário é exibido como "Anônimo". O email, quando informado, é apenas um dado de contato exibido/associado ao comentário, não usado para autenticação.
- **RN8** — Somente o dono do perfil pode excluir comentários publicados nele (RF12); o autor do comentário (autenticado ou não) não tem essa permissão sobre o próprio comentário nesse fluxo.
- **RN9** — Um "like" (RF13) está associado a um comentário específico; a mesma origem (usuário autenticado, ou visitante identificado por sessão/IP quando anônimo) não pode dar mais de um like no mesmo comentário.
- **RN10** — Excluir um comentário (RF12) deve remover também os likes associados a ele.

## Integração e Distribuição

- **RN11** — O widget embutível (RF14) só pode exibir comentários de um perfil que exista e esteja ativo no sistema.
- **RN12** — O conteúdo exibido pelo widget deve refletir o mesmo conjunto de comentários visíveis na página pública do perfil (RF9) — não pode divergir (ex.: mostrar comentário já excluído).

## Notificações

- **RN13** — A notificação por email de novo comentário (RF15) só é enviada ao dono do perfil se ele tiver uma conta ativa e email válido cadastrado.

---

# Casos de Uso
 
**Atores:**
- **Visitante** — qualquer pessoa acessando o sistema, autenticada ou não.
- **Usuário** — visitante autenticado, dono de uma conta e de um perfil.
- **Sistema** — dispara ações automáticas (ex.: envio de email).

## UC01 — Criar conta
- **Ator:** Visitante
- **Pré-condição:** Visitante não possui conta.
- **Fluxo principal:** Visitante informa dados de cadastro (nome, email, senha) → sistema cria a conta e o perfil vinculado (RF1, RF2, RN1) → sistema autentica o usuário automaticamente.
- **Fluxo alternativo:** Nome de usuário já existe (RN2) → sistema rejeita e solicita outro.
- **Pós-condição:** Conta e perfil criados e vazios (sem comentários).

## UC02 — Editar perfil
- **Ator:** Usuário
- **Pré-condição:** Usuário autenticado.
- **Fluxo principal:** Usuário acessa configurações do próprio perfil → altera nome, foto, bio e/ou estilização da página (RF3, RF8) → sistema salva as alterações.
- **Pós-condição:** Perfil atualizado e refletido na visualização pública (RF9).

## UC03 — Excluir conta
- **Ator:** Usuário
- **Pré-condição:** Usuário autenticado.
- **Fluxo principal:** Usuário solicita exclusão da conta → sistema pede confirmação → usuário confirma → sistema exclui a conta, o perfil e todos os comentários associados a ele (RF4, RN3).
- **Pós-condição:** Conta, perfil e comentários deixam de existir permanentemente.

## UC04 — Alterar senha
- **Ator:** Usuário
- **Pré-condição:** Usuário autenticado.
- **Fluxo principal:** Usuário informa senha atual e nova senha (RF5) → sistema valida a senha atual → sistema atualiza a senha (podendo ser igual à anterior, RN5).
- **Fluxo alternativo:** Senha atual incorreta → sistema rejeita a alteração.

## UC05 — Recuperar senha esquecida
- **Ator:** Visitante (dono da conta, mas não autenticado)
- **Pré-condição:** Conta existente com email cadastrado.
- **Fluxo principal:** Visitante solicita recuperação informando o email → sistema envia link/token de redefinição (RF6, RN4) → visitante acessa o link e define nova senha → sistema invalida o link.
- **Fluxo alternativo:** Link expirado ou já utilizado → sistema rejeita e orienta nova solicitação.

## UC06 — Buscar perfil
- **Ator:** Visitante
- **Fluxo principal:** Visitante informa termo de busca (nome de usuário) → sistema retorna perfis correspondentes (RF7) → visitante seleciona um perfil para visualizar.

## UC07 — Visualizar perfil e comentários
- **Ator:** Visitante
- **Pré-condição:** Nenhuma (não exige autenticação, RF9).
- **Fluxo principal:** Visitante acessa a URL do perfil → sistema exibe dados do perfil, estilização (RF8) e lista de comentários com likes.

## UC08 — Comentar em perfil
- **Ator:** Visitante
- **Pré-condição:** Perfil de destino existe (nenhuma autenticação exigida, RN6).
- **Fluxo principal:** Visitante acessa um perfil → preenche o comentário e, opcionalmente, nome e email (RF10, RF11, RN7) → sistema publica o comentário → sistema notifica o dono do perfil por email (RF15, RN13).
- **Fluxo alternativo:** Campos de nome/email deixados em branco → comentário é exibido como "Anônimo".

## UC09 — Curtir comentário
- **Ator:** Visitante
- **Pré-condição:** Comentário existente e ainda não curtido pela mesma origem (RN9).
- **Fluxo principal:** Visitante clica em "like" em um comentário → sistema registra o like e atualiza a contagem exibida (RF13).
- **Fluxo alternativo:** Origem já curtiu esse comentário → sistema ignora/bloqueia o novo like.

## UC10 — Excluir comentário
- **Ator:** Usuário (dono do perfil)
- **Pré-condição:** Usuário autenticado e comentário pertence ao seu perfil.
- **Fluxo principal:** Usuário seleciona um comentário em seu perfil → confirma exclusão → sistema remove o comentário e os likes associados (RF12, RN10).

## UC11 — Obter widget embutível
- **Ator:** Usuário
- **Pré-condição:** Usuário autenticado e possui perfil ativo.
- **Fluxo principal:** Usuário acessa a opção de widget em seu perfil → sistema gera o código (iframe/script) referente ao seu guestbook (RF14, RN11) → usuário copia o código para uso em site de terceiros.
- **Pós-condição:** Widget exibido externamente reflete o mesmo conteúdo do perfil (RN12).

## UC12 — Notificar novo comentário *(disparado pelo sistema, incluído em UC08)*
- **Ator:** Sistema
- **Pré-condição:** Dono do perfil possui conta ativa e email válido (RN13).
- **Fluxo principal:** Sistema detecta novo comentário publicado → envia email de notificação ao dono do perfil (RF15).

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
