# GuestBook — Diagrama de Blocos (Arquitetura)

> Diagrama agnóstico de provedor de nuvem. A escolha da operadora (Azure, GCP, Oracle Cloud, Render, Railway, etc.) ainda está em aberto — o requisito é apenas ter free tier, já que AWS está descartada.

```mermaid
flowchart TB
    Client["Navegador do Cliente<br/>(Frontend baixado sob demanda)"]

    subgraph Cloud["Nuvem (provedor a definir)"]
        direction TB

        Frontend["Frontend estático<br/>(hospedagem + CDN)"]
        API["Backend / API RESTful"]
        DB[("Banco de Dados<br/>usuários, perfis, comentários, likes")]
        Storage[("Armazenamento de arquivos<br/>fotos de perfil, assets de estilização")]

        Frontend -.->|entrega dos arquivos| Client
        Client -->|requisições REST| API
        API --> DB
        API --> Storage
    end

    AuthProvider["Provedor de Autenticação Externo<br/>(Google, Apple, etc.)"]
    EmailService["Serviço de Email<br/>(notificações)"]
    ThirdPartySite["Site de Terceiros<br/>(widget embutível)"]

    Client -->|login/autorização| AuthProvider
    AuthProvider -->|token| API
    API -->|dispara envio| EmailService
    EmailService -->|notifica| Client
    API -->|serve dados do widget| ThirdPartySite
```

## Componentes

| Bloco | Responsabilidade |
|---|---|
| **Frontend** | Interface web, baixada sob demanda no navegador do cliente |
| **Backend / API RESTful** | Regras de negócio, autenticação/autorização, orquestra acesso a dados |
| **Banco de Dados** | Persistência de contas, perfis, comentários e likes |
| **Armazenamento de arquivos** | Fotos de perfil e assets usados na estilização da página |
| **Provedor de Autenticação Externo** | Login via OAuth (Google, Apple, etc.) — nenhuma senha de terceiro é gerenciada diretamente |
| **Serviço de Email** | Envio de notificações de novo comentário e recuperação de senha |
| **Site de Terceiros** | Consome o widget embutível, exibindo os comentários do perfil fora do domínio do GuestBook |


