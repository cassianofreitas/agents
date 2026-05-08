# Instruções para Agentes de IA

![Next.js](https://img.shields.io/badge/Next.js-000000?style=for-the-badge&logo=nextdotjs&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white)
![Ubuntu](https://img.shields.io/badge/Ubuntu-E9430F?style=for-the-badge&logo=ubuntu&logoColor=white)

Este repositório contém as diretrizes mestras para a operação de Agentes de IA (como o Antigravity) em projetos de desenvolvimento. O foco principal é estabelecer um ambiente de trabalho padronizado, acessível e altamente técnico para aplicações construídas com **Next.js**.

## O que é este projeto?

O coração deste repositório é o arquivo `AGENTS.md`. Ele serve como um "manual de conduta" para agentes de codificação, garantindo que qualquer IA que interaja com o código siga rigorosamente padrões de versionamento, acessibilidade (WCAG 2.2), e fluxos de trabalho modernos como o **Get Shit Done (GSD)**.

## Pré-requisitos

Para que o fluxo definido nestas instruções funcione perfeitamente, os seguintes requisitos devem ser atendidos:

- **Ferramenta GSD:** Instalada e disponível no PATH. [Instale aqui](https://github.com/gsd-build/get-shit-done).
- **Ambiente Node.js:** Compatível com Next.js (App Router).
- **Docker:** Para orquestração de containers.

## Instalação

Para utilizar estas instruções em seu próprio projeto:

1. Clone este repositório ou copie o arquivo `AGENTS.md` para a raiz do seu projeto.
2. Certifique-se de que o seu agente de IA tenha permissão de leitura para este arquivo.
3. (Opcional) Configure seu ambiente VPS para utilizar a rede `proxy` padrão para o **Nginx Proxy Manager**.

```bash
# Exemplo de rede docker necessária
docker network create proxy
```

## Como Usar

Ao iniciar uma nova sessão com seu Agente de IA, ele lerá o arquivo `AGENTS.md` e automaticamente:

1. Verificará a existência do `README.md` e `CHANGELOG.md`.
2. Executará o comando `gsd` para alinhar as tarefas.
3. Aplicará os padrões de **Conventional Commits** em todos os registros.

## Padrões Adotados

Este projeto é guiado pelas seguintes especificações:

- **Versionamento:** [SemVer 2.0.0](https://semver.org/lang/pt-BR/)
- **Commits:** [Conventional Commits 1.0.0](https://www.conventionalcommits.org/pt-br/v1.0.0/)
- **Metodologia:** [The Twelve-Factor App](https://12factor.net/pt_br/)
- **Acessibilidade:** [WCAG 2.2 (Nível AA)](https://www.w3.org/WAI/standards-guidelines/wcag/)
- **Documentação:** [Make a README](https://www.makeareadme.com/) e [Keep a Changelog](https://keepachangelog.com/pt-BR/1.1.0/)

## Contribuição

Contribuições são bem-vindas! Certifique-se de que suas sugestões mantenham o foco na automação via **n8n** e na paridade com ambientes Ubuntu/Docker.

## Licença

Este projeto está sob a licença [MIT](https://choosealicense.com/licenses/mit/).

---

> [!TIP]
> Desenvolvido com foco em eficiência por [Cassiano Freitas](https://github.com/cassianofreitas)
