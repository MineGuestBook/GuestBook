# GuestBook — Casos de Uso
 
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

## UC06 — Visualizar perfil e comentários
- **Ator:** Visitante
- **Pré-condição:** Nenhuma (não exige autenticação, RF9).
- **Fluxo principal:** Visitante acessa a URL do perfil → sistema exibe dados do perfil, estilização (RF8) e lista de comentários com likes.

## UC07 — Comentar em perfil
- **Ator:** Visitante
- **Pré-condição:** Perfil de destino existe (nenhuma autenticação exigida, RN6).
- **Fluxo principal:** Visitante acessa um perfil → preenche o comentário e, opcionalmente, nome e email (RF10, RF11, RN7) → sistema publica o comentário → sistema notifica o dono do perfil por email (RF15, RN13).
- **Fluxo alternativo:** Campos de nome/email deixados em branco → comentário é exibido como "Anônimo".

## UC08 — Curtir comentário
- **Ator:** Visitante
- **Pré-condição:** Comentário existente e ainda não curtido pela mesma origem (RN9).
- **Fluxo principal:** Visitante clica em "like" em um comentário → sistema registra o like e atualiza a contagem exibida (RF13).
- **Fluxo alternativo:** Origem já curtiu esse comentário → sistema ignora/bloqueia o novo like.

## UC19 — Excluir comentário
- **Ator:** Usuário (dono do perfil)
- **Pré-condição:** Usuário autenticado e comentário pertence ao seu perfil.
- **Fluxo principal:** Usuário seleciona um comentário em seu perfil → confirma exclusão → sistema remove o comentário e os likes associados (RF12, RN10).

## UC10 — Obter widget embutível
- **Ator:** Usuário
- **Pré-condição:** Usuário autenticado e possui perfil ativo.
- **Fluxo principal:** Usuário acessa a opção de widget em seu perfil → sistema gera o código (iframe/script) referente ao seu guestbook (RF14, RN11) → usuário copia o código para uso em site de terceiros.
- **Pós-condição:** Widget exibido externamente reflete o mesmo conteúdo do perfil (RN12).

## UC11 — Notificar novo comentário *(disparado pelo sistema, incluído em UC08)*
- **Ator:** Sistema
- **Pré-condição:** Dono do perfil possui conta ativa e email válido (RN13).
- **Fluxo principal:** Sistema detecta novo comentário publicado → envia email de notificação ao dono do perfil (RF15).
