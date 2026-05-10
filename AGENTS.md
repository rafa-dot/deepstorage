# AGENTS.md

Contexto e regras para agentes trabalhando neste repositorio.

## Contexto do projeto

Este repositorio e o blog tecnico Deep Storage, publicado em:

- `https://www.deepstorage.tech`

O site e 100% estatico:

- Sem npm, framework, bundler ou build pipeline.
- Deploy por push no branch principal via GitHub Pages.
- HTML, CSS e JavaScript vanilla.
- `marked.js` e carregado via CDN nas paginas que renderizam Markdown.
- O conteudo editorial principal vive em `script.js`.

Audiencia: profissionais de infraestrutura, arquitetos de storage e times enterprise.
Tom editorial: tecnico, objetivo, sem comparativos promocionais e sem dados nao verificaveis.

## Antes de editar

Leia estes arquivos antes de qualquer mudanca relevante:

- `CLAUDE.md`
- `SPEC.md`
- `AGENTS.md`

Depois, confira o estado local:

```bash
git status --short
```

Nao reverta alteracoes que voce nao fez.

## Regras principais

1. Nao introduza dependencias externas, package managers, backend, APIs ou etapa de build.
2. Mantenha o site abrindo direto no browser a partir dos arquivos estaticos.
3. Preserve o conteudo editorial tecnico, objetivo e sem comparativos promocionais.
4. Use os tokens existentes em `styles.css` sempre que possivel.
5. Evite hardcode de cores, espacamentos, raios e sombras fora do sistema de design.
6. Nao remova acessibilidade existente: `aria-*`, skip link, labels, headings coerentes e `alt`.
7. Nao quebre a busca global.
8. Atualize `sitemap.xml` quando adicionar pagina publica relevante.
9. Nao declare recursos inexistentes, como RSS, API ou paginas novas, sem cria-los.
10. Nao commite `.DS_Store`.

## Estrutura importante

- `script.js`: posts, conteudo de Conceitos, Recursos, busca global e logica principal.
- `styles.css`: sistema visual global.
- `index.html`: home e listagem de posts.
- `post.html`: leitor individual por `?id=N`.
- `conceitos.html`: base tecnica.
- `recursos.html`: links curados.
- `conversor.html`: conversor de unidades de storage.
- `about.html`: pagina sobre o projeto.
- `404.html`: erro customizado.
- `sitemap.xml`: URLs publicas indexaveis.
- `snapshot-diagram.svg` e `replication-diagram.svg`: diagramas tecnicos existentes.

## Design system

O design e dark, tecnico, com fundo quase preto e acentos ciano/violeta.

Tokens principais em `styles.css`:

- Fundo: `--bg`
- Superficies: `--surface`, `--surface-2`, `--surface-3`
- Bordas: `--border`, `--border-accent`
- Destaques: `--primary`, `--secondary`, `--accent-warm`
- Texto: `--text`, `--text-muted`, `--text-subtle`
- Fontes: `--font-body`, `--font-mono`
- Espacamento: `--sp-*`
- Raios: `--r-sm`, `--r`, `--r-lg`, `--r-pill`

Ao alterar UI:

- Use CSS puro.
- Prefira classes com escopo claro por pagina/componente.
- Preserve responsividade mobile.
- Verifique foco, hover, estados ativos e `prefers-reduced-motion`.
- Evite visual excessivamente decorativo ou com cara de landing page.
- Nao use cards dentro de cards sem necessidade.
- Garanta que textos nao estourem botoes, tabelas ou paineis no mobile.
- Imagens e SVGs novos devem seguir a paleta do projeto e ter `alt` quando forem informativos.

## Skills locais do usuario

Quando for criar interfaces, dashboards ou componentes UI/UX:

- Leia `/Users/rafael/.Codex/skills/ui-ux-pro-max/SKILL.md` antes de escrever codigo, se o arquivo existir.

Quando for humanizar, reescrever ou melhorar textos:

- Leia `/Users/rafael/.Codex/skills/humanizer/SKILL.md` antes de executar a tarefa, se o arquivo existir.

Se esses caminhos nao existirem, use os skills equivalentes disponiveis no ambiente e continue.

## Edicao de conteudo

Posts ficam no array `posts[]` em `script.js`.

Ao adicionar post:

- Use o proximo `id` disponivel.
- Mantenha `date` no formato `YYYY-MM-DD`.
- Escreva em portugues do Brasil.
- Preserve tom tecnico e direto.
- Escape backticks dentro de template literals Markdown.
- Verifique que a busca global encontra o novo conteudo.
- Atualize `sitemap.xml` se o post deve ser indexado.

Evite:

- Claims comerciais sem fonte.
- Comparativos promocionais entre fabricantes.
- Dados de performance, preco ou roadmap sem verificacao.
- Conteudo opinativo sem deixar claro o criterio tecnico.

## Busca global

A busca global esta em `script.js` e pesquisa:

- `posts[]`
- `conceitosContent`
- `recursosContent`

Se uma pagina nova tambem precisa aparecer na busca:

- Adicione uma fonte indexavel em `script.js`.
- Crie um resultado navegavel.
- Teste busca por titulo, termos principais e tag/categoria.

## SEO

Toda pagina publica indexavel deve ter:

- `title`
- `meta description`
- Open Graph basico
- Twitter Card basico
- canonical correto
- entrada em `sitemap.xml`

Ao alterar pagina existente, preserve metadados atuais salvo se a mudanca exigir atualizacao.

## Conversor de unidades

`conversor.html` usa JavaScript inline e CSS global.

Ao editar:

- Valide o script inline com `node --check`.
- Preserve a diferenca entre SI oficial e IEC/base 2 quando relevante.
- Evite alterar sem querer a semantica de TB/TiB, GB/GiB e bits.
- Teste pelo menos um caso comum, como `1 TB -> GB`.

## Validacao recomendada

Para mudancas em JavaScript global:

```bash
node --check script.js
```

Para scripts inline em HTML, extraia e valide antes de concluir. Exemplo para `conversor.html`:

```bash
sed -n '/<script>/,/<\/script>/p' conversor.html | sed '1d;$d' | node --check -
```

Para checar whitespace:

```bash
git diff --check
```

Para validar o site localmente:

```bash
python3 -m http.server 8000
```

Depois teste as paginas principais no browser:

- `/`
- `/post.html?id=1`
- `/conceitos.html`
- `/recursos.html`
- `/conversor.html`
- `/about.html`
- `/404.html`

Antes de finalizar, rode:

```bash
git status --short
```

Informe qualquer sujeira no working tree, inclusive arquivos nao rastreados.

## Git e arquivos locais

- Nao reverta alteracoes que voce nao fez.
- Nao use `git reset --hard` ou comandos destrutivos sem pedido explicito.
- Nao commite `.DS_Store`.
- Trate alteracoes em `.claude/`, `.codex/`, `.agents/` e arquivos locais de IDE com cuidado.
- Se houver alteracoes fora do escopo, apenas informe no final.
- Se tocar em arquivo que ja tinha mudancas, revise o diff para nao sobrescrever trabalho do usuario.

## Estilo de trabalho esperado

- Seja direto e pragmatico.
- Prefira mudancas pequenas e verificaveis.
- Explique decisoes tecnicas quando houver ambiguidade.
- Ao finalizar, diga o que mudou, como foi validado e quais arquivos ficaram modificados.
