# AGENTS.md

Instrucoes para agentes trabalhando neste repositorio.

## Contexto do projeto

Este repositorio e o blog tecnico Deep Storage, publicado em `www.deepstorage.tech`.

O site e 100% estatico:

- Sem npm, framework, bundler ou build pipeline.
- Deploy por push no branch principal via GitHub Pages.
- HTML, CSS e JavaScript vanilla.
- `marked.js` e carregado via CDN nas paginas que renderizam Markdown.

Antes de editar, leia:

- `CLAUDE.md`
- `SPEC.md`
- este `AGENTS.md`

## Regras principais

1. Nao introduza dependencias externas, package managers, backend, APIs ou etapa de build.
2. Mantenha o site abrindo direto no browser a partir dos arquivos estaticos.
3. Preserve o conteudo editorial tecnico, objetivo e sem comparativos promocionais.
4. Use os tokens existentes em `styles.css` sempre que possivel.
5. Evite hardcode de cores, espacamentos, raios e sombras fora do sistema de design.
6. Nao remova acessibilidade existente: `aria-*`, skip link, labels, headings coerentes e `alt`.
7. Nao quebre a busca global.
8. Atualize `sitemap.xml` quando adicionar pagina publica relevante.

## Estrutura importante

- `script.js`: posts, conteudo de Conceitos, Recursos e logica principal.
- `styles.css`: sistema visual global.
- `index.html`: home e listagem de posts.
- `post.html`: leitor individual por `?id=N`.
- `conceitos.html`: base tecnica.
- `recursos.html`: links curados.
- `conversor.html`: conversor de unidades de storage.
- `sitemap.xml`: URLs publicas indexaveis.

## Edicao de conteudo

Posts ficam no array `posts[]` em `script.js`.

Ao adicionar post:

- Use o proximo `id` disponivel.
- Mantenha `date` no formato `YYYY-MM-DD`.
- Escape backticks dentro de template literals Markdown.
- Verifique que a busca global encontra o novo conteudo.
- Atualize `sitemap.xml` se o post deve ser indexado.

## UI e CSS

O design e dark, tecnico, com fundo quase preto e acentos ciano/violeta:

- Fundo principal: `--bg`
- Superficies: `--surface`, `--surface-2`, `--surface-3`
- Destaque: `--primary`, `--secondary`
- Texto: `--text`, `--text-muted`, `--text-subtle`

Ao criar ou alterar interfaces:

- Use CSS puro.
- Prefira classes com escopo claro por pagina/componente.
- Preserve responsividade mobile.
- Verifique estados de foco, hover e reduced motion.
- Evite UI excessivamente decorativa ou com cara de landing page.

## Busca global

A busca global esta em `script.js` e pesquisa:

- `posts[]`
- `conceitosContent`
- `recursosContent`

Se uma pagina nova tambem precisa ser encontrada pela busca, adicione uma fonte indexavel em `script.js` e um resultado navegavel.

## SEO

Toda pagina publica deve ter:

- `title`
- `meta description`
- Open Graph basico
- Twitter Card basico
- canonical correto
- entrada em `sitemap.xml`, quando indexavel

Nao declarar arquivos inexistentes, como feed RSS, sem criar o arquivo correspondente.

## Validacao recomendada

Para mudancas em JavaScript:

```bash
node --check script.js
```

Para scripts inline em HTML, extraia e valide antes de concluir.

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

## Git e arquivos locais

- Nao reverta alteracoes que voce nao fez.
- Nao commite `.DS_Store`.
- Trate alteracoes em `.claude/` com cuidado; confirme se fazem parte do escopo antes de remover ou restaurar.
- Antes de finalizar, rode `git status --short` e informe qualquer sujeira no working tree.
