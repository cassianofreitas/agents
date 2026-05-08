# Instruções para Agentes de Desenvolvimento (AGENTS.md)

> [!NOTE]
> - **Autor:** [Cassiano Freitas](https://github.com/cassianofreitas)
> - **Versão:** 1.0.1
> - **Idioma de trabalho:** Português do Brasil (pt-BR).
> - **Escopo:** Este documento define as diretrizes, padrões e expectativas para todos os agentes de IA que interagem com este repositório. O objetivo é garantir consistência, acessibilidade e qualidade técnica seguindo as melhores práticas modernas.

---

## 1. Idioma e Comunicação

- **Idioma Principal:** Todas as interações com o usuário e a documentação interna/externa devem ser obrigatoriamente em **Português do Brasil (pt-BR)**.
- **Tom de Voz:** Profissional, técnico e objetivo.

## 2. Inicialização e Fluxo de Trabalho (Get Shit Done)

- **Get Shit Done (GSD):** Este projeto utiliza a metodologia e ferramenta [Get Shit Done](https://github.com/gsd-build/get-shit-done).
- **Ação de Início:** Ao iniciar uma nova sessão ou tarefa, o agente deve sempre acionar o `gsd`.
- **Verificação de Instalação:** Caso o comando `gsd` não esteja disponível no ambiente, o agente deve sinalizar o usuário imediatamente, fornecendo o link do repositório e instruções para instalação.

## 3. Controle de Versão e Histórico

- **Versionamento Semântico (SemVer):** O projeto segue rigorosamente o [Semantic Versioning 2.0.0](https://semver.org/lang/pt-BR/).
- **Conventional Commits:** Todas as mensagens de commit devem seguir a especificação [Conventional Commits 1.0.0](https://www.conventionalcommits.org/pt-br/v1.0.0/).
  - Ex: `feat(api): adiciona endpoint de autenticação`, `fix(ui): corrige contraste no botão principal`.
- **Registro de Mudanças:** Manter o arquivo `CHANGELOG.md` atualizado seguindo os princípios de [Keep a Changelog 1.1.0](https://keepachangelog.com/pt-BR/1.1.0/). Caso o arquivo não exista, crie-o seguindo o padrão.
- **Arquivos de Ignoração:**
  - **`.gitignore`:** Deve seguir os padrões oficiais para Node.js e Next.js, garantindo que arquivos de ambiente (`.env*`), builds (`.next/`, `out/`) e dependências (`node_modules/`) não sejam rastreados.
  - **`.dockerignore`:** Essencial para builds eficientes. Deve ignorar obrigatoriamente `node_modules`, `.next`, `.git`, `dist`, e qualquer arquivo de log ou documentação desnecessária para o runtime.

> [!WARNING]
>
> - Caso o `CHANGELOG.md` não exista, crie-o seguindo o padrão. Lembre-se do idioma de preferência.
> - Caso o `.gitignore` ou `.dockerignore` não existam, crie-os seguindo as melhores práticas para Next.js + Docker.

## 4. Arquitetura e Desenvolvimento (Next.js)

- **Framework:** Next.js (utilizando App Router e boas práticas de Server/Client Components).
- **The Twelve-Factor App:** O desenvolvimento deve respeitar os princípios da [Metodologia 12-Factor](https://12factor.net/pt_br/), especialmente no que tange a configurações via variáveis de ambiente, paridade entre desenvolvimento/produção e logs.
- **Acessibilidade:** Todo componente de interface deve cumprir os critérios de sucesso da **WCAG 2.2** (nível AA, no mínimo). Garanta semântica HTML correta, suporte a leitores de tela e contraste adequado.

## 5. Documentação e Estética

- **README:** O arquivo principal de documentação deve seguir a estrutura proposta pelo [Make a README](https://www.makeareadme.com/).
- **Ícones:** Para representações visuais e badges, utilize exclusivamente ícones do [Simple Icons](https://simpleicons.org/).
- **Documentação de Código:** Comentários JSDoc em português para funções complexas e tipagem rigorosa com TypeScript.

> [!WARNING]
> Caso o `README.md` não exista, crie-o seguindo o padrão. Lembre-se do idioma de preferência.

## 6. Infraestrutura e Deploy (Contexto Específico)

- **Ambiente:** A aplicação será executada em ambiente Docker em um servidor Ubuntu.
- **Rede e Proxy:** Ao gerar arquivos `docker-compose.yml` ou configurações de rede, considere a rede externa `proxy` como padrão para integração com o Nginx Proxy Manager.
- **Automação:** Priorize fluxos que possam ser automatizados via n8n.

## 7. Infraestrutura e Orquestração (Padrão Docker Compose)

- **Redes:** Todo serviço web deve ser conectado à rede externa `proxy`. O arquivo deve declarar a rede no final como `external: true`.
- **Portas:** Não exponha portas diretamente para o host (ex: `80:3000`). Deixe o tráfego fluir internamente pela rede `proxy` para ser interceptado pelo Nginx Proxy Manager.
- **Políticas de Reinicialização:** Use sempre `restart: unless-stopped`.
- **Ambiente:** Use o bloco `env_file` ou referencie variáveis de ambiente que serão alimentadas por um arquivo `.env`. Nunca faça hardcode de credenciais.
- **Volumes:** Mapeie volumes de forma nomeada ou em caminhos relativos consistentes para persistência de dados.

> [!IMPORTANT]
> Ao gerar um `docker-compose.yml`, certifique-se de que o nome do serviço e o hostname sejam amigáveis para a configuração no painel do Nginx Proxy Manager.

## 8. Instruções de Execução para o Agente

- **Verificação de Documentos:** Antes de qualquer desenvolvimento, verifique a existência de `README.md`, `CHANGELOG.md`, `.gitignore`, `.dockerignore` e outros arquivos de padronização citados. Se estiverem ausentes, crie-os com o conteúdo inicial pertinente.
- **Execução Inicial:** Execute `gsd` para alinhar as prioridades da sessão.
- **Padrões de Código:** Valide se o código gerado segue os padrões de linting e acessibilidade (WCAG 2.2).
- **Dependências:** Ao sugerir novos pacotes, priorize soluções leves e compatíveis com Next.js.
- **Builds:** Ao modificar o Dockerfile, valide se o `.dockerignore` está otimizado para evitar o envio de contexto desnecessário ao daemon.

---

## Protocolo de Dockerização e Deploy (Next.js)

Sempre que o projeto for preparado para deploy no ambiente de produção (Portainer/Nginx Proxy Manager) ou acontecer algum update ou merge na main, siga as instruções abaixo para garantir a padronização da infraestrutura.

### 1. Verificação e Criação de Ativos

Se os arquivos abaixo não existirem na raiz do projeto, você deve criá-los seguindo estas especificações:

#### A. `Dockerfile` (Multi-stage Build)

O build deve ser otimizado para produção utilizando três estágios: `deps`, `builder` e `runner`.

- **Base:** `node:20-alpine`
- **Output:** Apenas os artefatos necessários (`.next`, `public`, `node_modules`, `package.json`).

#### B. `.dockerignore`

Deve excluir obrigatoriamente:

- `node_modules`, `.next`, `.git`, `.env.local` e arquivos de log.

#### C. `docker-compose.yml`

Deve ser configurado para o ecossistema Portainer + NPM:

- **Rede:** Utilizar obrigatoriamente a rede externa chamada `proxy`.
- **Restart:** Configurado como `always`.
- **Ambiente:** `NODE_ENV=production`.

### 2. Comandos de Operação

Para construir e testar localmente antes do push:

- **Build:** `docker build -t [nome-do-projeto]:latest .`
- **Execução:** `docker compose up -d`

### 3. Integração com Nginx Proxy Manager (NPM)

Ao configurar o Proxy Host no NPM:

- **Forward Hostname:** Usar o `container_name` definido no `docker-compose.yml`.
- **Forward Port:** `3000`.

> [!NOTE]
> Caso identifique variações na versão do Node no `package.json`, ajuste a imagem base do Dockerfile para manter a compatibilidade.

---

> [!TIP]
> Verifique atualizações deste projeto no [GitHub](https://github.com/cassianofreitas/agents).
