---
name: obsidian-register
description: Estrutura e mantém o cofre Obsidian usado como base de conhecimento de um codebase (features, bugs, refatorações, decisões arquiteturais e convenções) e como registro de troubleshooting recorrente. Use sempre que o usuário pedir para criar, atualizar ou consultar notas do vault sobre um projeto — ao terminar uma feature, resolver um bug, fazer uma refatoração, tomar uma decisão técnica, ou quando precisar relembrar contexto de um projeto antes de mexer nele. Também use ao iniciar trabalho em um projeto já documentado no vault, para ler o index/MOC antes de qualquer alteração.
---

## Caminho do Vault:

`/Users/paulorodrigues/Documents/cofre_do_paulo`

# Obsidian Vault como Base de Conhecimento do Codebase

Esta skill define como ler, escrever e manter um cofre Obsidian que funciona como memória
persistente de projetos de código. O objetivo é que o Claude nunca perca contexto entre
sessões: decisões já tomadas, bugs já resolvidos e convenções do projeto ficam registrados
em markdown, versionáveis e linkados entre si.

## Estrutura do cofre

Cada projeto é uma pasta própria dentro do cofre, organizada assim:

```
<Projeto>/
├── index.md            # MOC (Map of Content) do projeto — ponto de entrada
├── convencoes.md        # Padrões de código, nomenclatura, arquitetura do projeto
├── decisoes/             # ADRs — por que escolhemos X e não Y
│   └── <slug-da-decisao>.md
├── features/             # Uma nota por feature — o que é, como funciona, links relevantes
│   └── <slug-da-feature>.md
├── bugs/                 # Uma nota por bug relevante/recorrente — sintoma, causa, correção
│   └── <slug-do-bug>.md
└── refatoracoes/          # O que mudou estruturalmente e por quê (não confundir com decisoes/)
    └── <slug-da-refatoracao>.md
```

Regra de ouro: **decisões (ADR) e refatorações são coisas diferentes.**

- `decisoes/` responde "por que escolhemos essa abordagem" — o raciocínio, as alternativas
  consideradas, os trade-offs.
- `refatoracoes/` registra "o que mudou de fato na estrutura do código e quando" — o resultado.

Todas as notas devem terminar com uma seção `## Lições aprendidas` quando aplicável, e devem
linkar entre si com `[[wikilinks]]` (ex: um bug pode linkar a feature afetada e a decisão que
o originou).

## O index.md (MOC) de cada projeto

O `index.md` é a primeira coisa a ler antes de mexer em qualquer projeto documentado no vault.
Ele deve listar e linkar:

- As principais features (com uma linha de resumo cada)
- Bugs recorrentes ou notáveis
- Decisões arquiteturais chave
- Link para `convencoes.md`

Isso evita ter que varrer todas as pastas — uma leitura do index já dá o mapa geral do projeto.

## Frontmatter / metadados

Toda nota deve ter frontmatter simples para permitir filtragem rápida, além dos links:

```yaml
---
projeto: <nome-do-projeto>
tipo: feature | bug | decisao | refatoracao
status: aberto | resolvido | em-andamento | arquivado
modulo: <ex: auth, pagamentos, ui>
data: <YYYY-MM-DD>
---
```

## Workflow: quando e como escrever notas

**Ao terminar uma feature:**

1. Criar/atualizar `features/<slug>.md` com o que foi feito, como funciona, e por quê.
2. Linkar a partir do `index.md`.
3. Se alguma decisão técnica relevante foi tomada no processo, criar uma nota em `decisoes/`.

**Ao resolver um bug:**

1. Criar `bugs/<slug>.md` com: sintoma observado, causa raiz, correção aplicada.
2. Se o bug é recorrente ou a causa raiz é sutil, adicionar seção `## Lições aprendidas`.
3. Linkar à feature/módulo afetado.

**Ao fazer uma refatoração:**

1. Criar `refatoracoes/<slug>.md` descrevendo o que mudou estruturalmente.
2. Se a refatoração decorre de uma decisão arquitetural nova, linkar (ou criar) a nota
   correspondente em `decisoes/`.

**Ao tomar uma decisão arquitetural (ADR):**

1. Criar `decisoes/<slug>.md` no formato: Contexto → Decisão → Alternativas consideradas →
   Consequências.

**Ao iniciar trabalho em um projeto já existente no vault:**

1. Ler `index.md` primeiro.
2. Ler `convencoes.md` antes de escrever ou alterar qualquer código.
3. Consultar `bugs/` se o trabalho envolve uma área com histórico de problemas.

## Checklist rápido antes de fechar uma sessão de trabalho

- [ ] A nota relevante (feature/bug/decisão/refatoração) foi criada ou atualizada?
- [ ] O frontmatter está preenchido (projeto, tipo, status, modulo, data)?
- [ ] A nota está linkada a partir do `index.md` do projeto?
- [ ] Se houve algo não óbvio ou custoso de descobrir, isso virou uma "Lição aprendida"?
