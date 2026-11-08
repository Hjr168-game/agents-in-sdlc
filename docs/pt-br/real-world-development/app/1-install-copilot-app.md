---
title: "Lição 1 - Instale o aplicativo GitHub Copilot"
description: "Instale o aplicativo GitHub Copilot, conecte o repositório criado a partir do modelo, conheça o espaço de trabalho e experimente um chat rápido."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

O **[aplicativo GitHub Copilot][about-copilot-app]** é um aplicativo para desktop voltado ao desenvolvimento orientado por agentes. Ele foi criado com base no GitHub Copilot CLI e tem integração nativa com o GitHub. Portanto, os repositórios, as ramificações e os pipelines de CI funcionam sem configuração adicional. O aplicativo foi projetado para fluxos de trabalho em que você orienta vários agentes em paralelo, cada um no próprio espaço de trabalho isolado, e automatiza tarefas repetitivas em vez de fazer tudo por conta própria. Com o Node.js instalado e a cópia do projeto pronta, o próximo passo é instalar o aplicativo e conectar esse repositório.

Nesta lição, você vai:

- instalar o aplicativo GitHub Copilot e entrar na sua conta;
- adicionar o projeto ao aplicativo pelo repositório do GitHub;
- conhecer o espaço de trabalho, incluindo o backlog criado pelo modelo;
- experimentar um chat rápido para saber mais sobre o próprio aplicativo.

## Cenário

Sua equipe está adotando agentes de IA para trabalhar em um backlog crescente. O aplicativo Copilot oferece um único lugar para orientar esse trabalho: selecionar problemas, executar agentes, revisar alterações e fazer merge de pull requests. Nesta lição, você instalará e conectará o aplicativo e aprenderá a iniciar uma conversa sobre o projeto.

> [!NOTE]
> É necessário ter um plano elegível do Copilot: Copilot Free, Copilot Student ou qualquer plano pago (Pro, Pro+, Business ou Enterprise). Se você usa o Copilot Business ou o Copilot Enterprise, o administrador precisa manter habilitada a política **GitHub Copilot app**.

## Instale e configure o aplicativo GitHub Copilot

Para usar o aplicativo GitHub Copilot, primeiro é preciso instalá-lo. Há versões para Windows, macOS e Linux. Vamos instalar o aplicativo, autenticar a conta e adicionar o repositório Tailspin Toys.

1. Em um navegador, abra a [página inicial do aplicativo GitHub Copilot][download-app].
2. Baixe o aplicativo para sua plataforma e instale-o seguindo as instruções da página.
3. Abra o aplicativo após a instalação.
4. Selecione **Sign in to GitHub** e siga as instruções para se autenticar. Se você usa o GitHub Enterprise Cloud com residência de dados, selecione **Use GitHub Enterprise** e informe o nome do host `*.ghe.com` quando solicitado. O GitHub Enterprise Server não é compatível.
5. Se o processo de integração oferecer a seleção de repositórios, selecione o repositório Tailspin Toys criado a partir do modelo. Você também pode ignorar essa etapa e adicionar o projeto depois.
6. Quando solicitado, escolha um tema e conclua a integração.
7. Procure seu repositório em **Sessions**, na barra lateral. Se ele já estiver listado, não é necessário adicioná-lo novamente.
8. Se o repositório ainda não estiver conectado, selecione **+** ao lado de **Sessions** para criar uma sessão e, em seguida, selecione **Add GitHub repository**. Informe o proprietário e o nome do repositório anotados na lição de pré-requisitos.
9. Confirme se o repositório aparece na barra lateral. No layout usado aqui, as pastas de repositório, como `tailspin-toys`, ficam agrupadas em **Sessions**, com as sessões individuais abaixo de cada repositório.

## Conheça o espaço de trabalho

Com o projeto conectado, reserve um momento para conhecer o espaço de trabalho. O aplicativo organiza tudo em algumas áreas na barra lateral:

- **New**: inicie uma nova sessão de chat com o Copilot.
- **Pull requests**: acesse seus pull requests por meio da integração nativa com o GitHub. Você pode procurar e filtrar PRs, verificar o status de CI e revisá-los sem sair do aplicativo.
- **Issues**: acesse seus problemas do GitHub da mesma maneira. Você pode procurá-los, filtrá-los e iniciar uma sessão a partir de um problema.
- **Automations**: acesse tarefas de agente salvas que são executadas em uma agenda ou sob demanda. Elas são úteis para gerenciar listas de tarefas, manutenção regular do projeto ou outras atividades repetitivas que você queira delegar. A recapitulação indica essas tarefas como uma próxima etapa, não como outro exercício do workshop.
- **Customize**: adicione recursos ao aplicativo Copilot na forma de servidores MCP, plugins, habilidades e outros componentes. Você usará esta área para configurar o MCP do Playwright.
- **Chats**: acesse conversas leves para perguntas e brainstorming que não precisam de uma ramificação nem de um espaço de trabalho próprios. Esta seção aparece depois que você inicia o primeiro chat. Se ela não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat.
- **Sessions**: agrupe pastas de repositório e as sessões de agente. Selecione o nome de um repositório, como `tailspin-toys`, para abrir a visualização dele, ou selecione uma sessão abaixo do repositório para voltar à conversa. Cada sessão é executada em seu próprio espaço de trabalho isolado, permitindo executar várias ao mesmo tempo sem conflito entre as alterações. Você iniciará sua primeira sessão ao adicionar avaliações por estrelas.

Ao longo do workshop, você explorará o espaço de trabalho!

> [!TIP]
> Na dúvida, pergunte ao Copilot! Se não souber como fazer algo ou se algo é possível, pergunte ao Copilot. Ele ajudará a orientar você.

### Localize o backlog criado pelo modelo

Provavelmente não existe projeto sem backlog, e o Tailspin Toys não é diferente. Vamos explorar o backlog criado quando você usou o modelo.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys, não uma sessão individual abaixo dela. Em seguida, selecione **Issues** na navegação superior do repositório.
2. Encontre os problemas a seguir pelo título, sem presumir seus números:

   - Allow users to filter games by category and publisher
   - Update our repository coding standards

3. Selecione um problema para ler seus detalhes. Cada problema também permite iniciar uma sessão de agente. Depois de concluir uma primeira alteração rápida, você começará pelo problema de filtragem.

> [!NOTE]
> As listas de itens em **Issues** e **Pull requests** são filtradas automaticamente para mostrar apenas itens dos repositórios adicionados ao aplicativo Copilot. Quer ver itens de trabalho de outros repositórios? Adicione-os ao aplicativo.

## Experimente um chat rápido

Uma ótima maneira de se familiarizar com o aplicativo é usá-lo para saber mais sobre o *próprio aplicativo* — e um **chat rápido** é a ferramenta ideal para isso. Os chats rápidos permitem fazer uma pergunta ou trocar ideias sem criar uma ramificação ou worktree. São perfeitos para perguntas rápidas e descartáveis, sem precisar de uma sessão.

1. Na barra lateral, selecione **+** ao lado de **Chats** para abrir um novo chat. Se **Chats** ainda não estiver visível, selecione **+** ao lado de **Sessions** para iniciar seu primeiro chat; **Chats** aparecerá depois disso.
2. Pergunte ao aplicativo como as sessões dele funcionam:

   ```plaintext
   How does the GitHub Copilot app use worktrees?
   ```

3. Leia a resposta na visualização da conversa. Você verá que cada sessão é executada em seu próprio git worktree isolado — isso permite executar vários agentes em paralelo sem conflito entre as alterações. Você pode continuar a conversa ou iniciar um novo chat a qualquer momento.

## Resumo e próximas etapas

Parabéns! Você instalou o aplicativo GitHub Copilot, conectou o projeto e explorou o espaço de trabalho. Você aprendeu a:

- instalar o aplicativo e entrar no GitHub;
- adicionar um projeto pelo repositório do GitHub;
- conhecer o espaço de trabalho e localizar o backlog criado em **Issues**;
- usar um chat rápido para fazer uma pergunta breve e descartável.

Em seguida, você [iniciará sua primeira sessão de agente][next-lesson] e a usará para exibir uma avaliação por estrelas nos cartões dos jogos.

## Recursos

- [Sobre o aplicativo GitHub Copilot][about-copilot-app]
- [Introdução ao aplicativo GitHub Copilot][getting-started]
- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../2-add-star-rating/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[download-app]: https://gh.io/app
