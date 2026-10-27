---
title: "Lição 5 - Personalize e use uma skill de verificações de qualidade"
description: "Explore a skill quality-checks existente, personalize o formato do relatório e use-a para validar a filtragem."
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Desenvolver software envolve mais do que apenas escrever código. Já validamos manualmente o funcionamento do código e usamos arquivos de instruções para garantir que ele siga os padrões. Mas e os testes? E o lint? E as outras partes da integração contínua (CI)?

Para esse tipo de tarefa, as **skills de agente** são ideais. Elas ajudam o Copilot a entender como executar corretamente operações como essas.

Nesta lição, você vai:

- explorar a skill `quality-checks` existente,
- personalizar o formato dos resultados,
- recarregar e executar a skill.

## Cenário

A Tailspin Toys usa a skill `quality-checks` para testes unitários, lint e verificações de tipos. A equipe quer aprimorar o relatório para facilitar a leitura dos resultados.

## Instruções, scripts e recursos

As skills de agente reúnem instruções reutilizáveis, scripts executáveis e recursos de apoio que um agente carrega sob demanda. Em geral, uma skill é uma pasta com o nome dela e um arquivo Markdown chamado `SKILL.md`. O Markdown contém frontmatter com nome e descrição, uma visão geral da função da skill e orientações sobre quando ela deve ser usada. A pasta também pode incluir subpastas com scripts e outros recursos que a skill utiliza.

> [!NOTE]
> Uma skill não precisa de outros arquivos ou pastas. A skill `quality-checks` da Tailspin Toys contém apenas `SKILL.md`, pois usa os comandos existentes no projeto.

As skills podem ficar na pasta `.github/skills` do projeto para serem compartilhadas e reutilizadas pela equipe ou na pasta pessoal de skills `~/.copilot/skills`.

## Explore a skill

Vamos explorar a skill `quality-checks` criada pela equipe Tailspin Toys para executar testes unitários, lint e verificações de tipos.

1. Volte ao Codespace. No editor do Codespaces, abra `.github/skills/quality-checks/SKILL.md`.
2. Leia `name` e `description` no início do arquivo. A descrição ajuda o Copilot a entender quando deve usar a skill.
3. Leia as instruções e observe como elas orientam o Copilot durante os testes e o lint.
4. Observe que a skill ainda não inclui uma seção **Results output formatting**.

## Execute a skill antes de fazer alterações

Você pode chamar skills diretamente pelo Copilot CLI ou usando linguagem natural. Vamos pedir ao Copilot que execute as três verificações da skill.

1. Volte à conversa de filtragem no modo Interactive.
2. Use este prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório apresentado ao final.

## Personalize o relatório

Queremos um relatório melhor, que indique o que foi executado, se houve aprovação ou falha e o que as ferramentas relataram. Vamos atualizar a skill para produzir esse relatório.

1. Volte a `.github/skills/quality-checks/SKILL.md`.
2. Adicione a seção a seguir ao final do arquivo:

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. O arquivo será salvo automaticamente.

## Execute a skill atualizada

Agora vamos testar a alteração. O Copilot CLI pode recarregar skills editadas sem reiniciar a conversa.

1. Digite:

   ```plaintext
   /skills reload
   ```

2. Use exatamente o mesmo prompt:

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. Observe o relatório final e compare-o com o primeiro.

## Resumo e próximos passos

Você personalizou e usou uma skill de agente existente. Nesta lição, você:

- explorou a skill `quality-checks` para testes unitários, lint e verificações de tipos,
- personalizou o formato dos resultados,
- recarregou e executou a skill.

Essa alteração acompanhará a filtragem no PR da funcionalidade. Em seguida, você permitirá que o Copilot interaja diretamente com o site [usando o servidor Playwright MCP][next-lesson].

## Mais exemplos de skills

Estes exemplos da comunidade são referências, não tarefas adicionais:

- [Especificação de Agent Skills][skill-spec]
- [Fluxo de contribuição: `make-repo-contribution`][contribution-example]
- [Documentos de requisitos: `prd`][prd-example]
- [Diagramas e script de exportação incluído: `drawio`][drawio-example]
- [Testes de navegador: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
