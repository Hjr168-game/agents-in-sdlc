---
title: "Lição 3 - Modos de agente: Plan e Autopilot"
description: "Explore os modos de agente: use Plan para definir uma abordagem, Autopilot para criar a filtragem a partir de uma issue e Interactive para revisar e verificar o resultado."
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

Começamos adicionando um pequeno recurso ao projeto. Mudanças maiores, porém, exigem um processo mais robusto. Felizmente, o aplicativo GitHub Copilot foi criado para funcionar com o fluxo de trabalho existente da organização e ajudar a desenvolver as soluções certas da maneira certa. Nesta lição, você começará a seguir um processo típico de desenvolvimento orientado por agentes: usará um problema para iniciar um novo recurso, verificará se o código é válido e se o recurso funciona como esperado e, por fim, fará o merge das alterações no projeto.

> [!NOTE]
> Você usará a mesma sessão durante todo o fluxo de trabalho do recurso. Normalmente, você usaria sessões ou PRs diferentes para os vários tipos de arquivos, mas faremos um atalho para nos concentrarmos nos conceitos principais.

Nesta lição, você vai:

- iniciar uma sessão de agente a partir de um problema do GitHub;
- definir requisitos no modo **Plan**;
- implementar o novo recurso usando o modo **Autopilot**;
- revisar o código;
- validar o recurso manualmente em um canvas de navegador.

Enquanto continuar trabalhando nesse recurso, você atualizará as instruções do repositório, personalizará a habilidade `quality-checks` existente, adicionará validação com MCP, criará um agente de QA e abrirá o PR do recurso.

## Cenário

O catálogo da Tailspin Toys está crescendo, e os visitantes precisam filtrar os jogos por categoria e empresa publicadora. O problema no backlog descreve o recurso, mas detalhes como combinar categorias precisam ser definidos antes de programar. Você usará o modo Plan para tomar essas decisões e, depois, autorizará uma implementação com escopo definido usando o Autopilot.

## Contexto

Introduzir agentes de programação com IA ao fluxo de desenvolvimento não muda os fundamentos. Na verdade, eles se tornam ainda mais importantes! A maioria dos desenvolvedores segue um fluxo semelhante a este:

1. Abrir um problema registrado com detalhes do que precisa ser feito.
2. Criar um plano do que precisa ser desenvolvido.
3. Desenvolver e revisar o código.
4. Executar os testes para validar o código.
5. Validar manualmente a nova funcionalidade.
6. Criar um pull request (PR).
7. Depois que o código for revisado e o processo de integração contínua for concluído com êxito, fazer o merge do código.

> [!NOTE]
> Os detalhes exatos variam de acordo com a equipe e a organização, mas a maioria segue uma variação do processo listado acima.

Ao seguir essa abordagem padrão, você garante que o código gerado por IA atenda aos requisitos e passe pelo mesmo processo de avaliação que o código escrito manualmente.

## Modos de sessão

O **modo de sessão** controla o nível de autonomia do agente. Você pode defini-lo no menu suspenso abaixo do campo de prompt e alterá-lo a qualquer momento:

- **Interactive**: você e o agente trabalham em conjunto. O agente sugere alterações e aguarda sua confirmação antes de continuar.
- **Plan**: o agente cria primeiro um plano. Você o revisa e aprova antes que o agente o execute.
- **Autopilot**: o agente trabalha com total autonomia: escreve código, executa testes e itera sem aguardar sua confirmação.

Comece no modo Plan, revise o plano e use o Autopilot para implementá-lo.

## Inicie uma sessão a partir do problema

Antes de começar, confirme se o PR de avaliação por estrelas foi integrado e se a `main` local está atualizada.

1. Em **Sessions**, na barra lateral, selecione a pasta do repositório Tailspin Toys e, em seguida, selecione **Issues** na navegação superior do repositório. Abra **Allow users to filter games by category and publisher**. Esse problema já está incluído no modelo; não é necessário criar outro.
2. Selecione **New session** e escolha uma **new working tree** baseada na `main` atualizada.

    ![Visualização do problema no aplicativo GitHub Copilot com uma seta apontando para o botão New session](../../../_images/app-new-session-from-issue.png)

3. Confirme se o problema está anexado à sessão e selecione **Plan** no seletor de modo.

> [!NOTE]
> Se Node.js ou npm não forem encontrados, peça ao agente que execute `node --version` e `npm --version`. Se os comandos funcionarem no terminal, mas não na sessão, verifique a configuração do PATH ou do gerenciador de versões do aplicativo e feche o aplicativo completamente antes de abri-lo novamente. Tente novamente a operação e as verificações que falharam; não reinstale o npm apenas porque o agente não o encontrou.

## Planeje o recurso de filtragem

O planejamento permite revisar a abordagem antes que o Copilot escreva o código. Como você começou pelo problema, o Copilot já tem a solicitação do recurso como contexto. Envie:

```plaintext
Build this feature.
```

Responda às perguntas do Copilot e compare o plano com os critérios de aceitação do problema. Confirme se ele inclui a filtragem por categoria e por empresa publicadora, controles acessíveis, alterações na camada de acesso a dados e testes. Converse sobre qualquer comportamento que não esteja claro, como a forma de combinar várias categorias ou o que acontece quando nenhum jogo corresponde aos filtros.

O plano deve incluir lint, testes de unidade, testes E2E e verificação de tipos com as ferramentas existentes do projeto. Mantenha o foco na implementação e nos testes da filtragem; você criará o PR depois de concluir o fluxo de qualidade. Peça alterações no plano antes de aprová-lo e deixe à mão a URL do problema e os esclarecimentos acordados para a validação posterior.

## Aprove explicitamente o Autopilot

Quando estiver satisfeito com o plano, selecione **Approve and implement this plan** ou a opção equivalente na sua versão. Não presuma que a aprovação muda o modo: confirme se o indicador mostra **Autopilot**.

O Copilot começará a trabalhar na implementação. Você verá que ele seguirá o plano de forma iterativa, gerando código e executando testes.

> [!NOTE]
> A aprovação pode iniciar a implementação imediatamente, portanto revise o plano antes. Se o Copilot informar que há dependências ausentes ou conflito de porta, resolva o problema de configuração antes de considerar as verificações concluídas. Interrompa apenas os servidores que você iniciou.

## Revise e verifique a implementação

Depois que o código for gerado, ele precisa ser revisado antes do merge, assim como qualquer outro código. Vamos revisar o código e executar o site para confirmar se tudo está correto.

1. Abra **Changes** e inspecione a implementação da filtragem e os testes.
2. Compare o resultado com o problema e os esclarecimentos aprovados, incluindo as combinações de categorias e empresas publicadoras. Confirme se as alterações seguem as instruções existentes do repositório.
3. Examine os resultados de lint, testes de unidade, testes E2E e verificação de tipos. Uma verificação ignorada não conta como aprovação.
4. Resolva as falhas e execute novamente as verificações afetadas antes de aceitar a implementação. A configuração E2E do Playwright cria e disponibiliza uma versão de pré-visualização e pode reutilizar um servidor local. Confirme se o servidor testado pertence a esta worktree, e não a uma lição anterior.

## Explore a nova funcionalidade

O código parece correto, mas será que funciona? Vamos iniciar o aplicativo como antes e abrir o site em um canvas de navegador.

1. Use este prompt para pedir ao Copilot que inicie o aplicativo e abra a página no canvas de navegador:

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. Em alguns instantes, o aplicativo será iniciado e uma janela do navegador será aberta dentro do aplicativo Copilot.
3. Selecione uma categoria e confirme se os resultados correspondem a ela. Limpe o filtro, selecione uma empresa publicadora e confirme se os resultados correspondem a essa empresa.
4. Combine os filtros de categoria e empresa publicadora e compare os resultados com o problema e o plano aprovado. Se houver suporte a várias categorias, verifique o comportamento combinado acordado.
5. Teste uma combinação sem jogos correspondentes e confira o estado vazio. Limpe ou redefina os filtros e confirme se a lista completa volta a aparecer.
6. Quando terminar, use este prompt para pedir ao Copilot que pare o servidor de desenvolvimento iniciado para esta sessão:

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## Mantenha esta sessão para as próximas lições

Ainda não crie nem faça merge do PR de filtragem. Mantenha esta sessão, a worktree e a ramificação para as Lições 4 a 8. Em seguida, você aplicará instruções personalizadas ao código de filtragem e adicionará melhorias para a habilidade e o QA antes de criar e fazer merge do PR combinado na Lição 8. Depois de parar o servidor de desenvolvimento, você pode deixar a sessão inativa e voltar a ela na próxima lição.

## Resumo e próximas etapas

Você usou diferentes modos de agente para criar e revisar um recurso. Nesta lição, você:

- iniciou uma sessão de agente a partir de um problema do GitHub;
- definiu requisitos no modo **Plan**;
- implementou o novo recurso usando o modo **Autopilot**;
- revisou o código;
- validou o recurso manualmente em um canvas de navegador.

Em seguida, vamos nos aprofundar na geração de código e garantir que ela siga as práticas documentadas, [usando instruções personalizadas][next-lesson].

## Recursos

- [Trabalhar com sessões de agente no aplicativo GitHub Copilot][agent-sessions]

[next-lesson]: ../4-custom-instructions/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
