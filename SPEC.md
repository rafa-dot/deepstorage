# Deep Storage — Especificações do Projeto

**Domínio:** [www.deepstorage.tech](https://www.deepstorage.tech)
**Idioma:** Português (Brasil)
**Última atualização deste documento:** 2026-03-30

---

## 1. Propósito

O **Deep Storage** é uma publicação técnica independente dedicada ao universo do armazenamento de dados corporativo (storage enterprise). O conteúdo é voltado para:

- Profissionais de infraestrutura de TI
- Arquitetos de storage
- Times que operam ambientes enterprise

O blog cobre plataformas dos principais fabricantes (Hitachi Vantara, NetApp, Dell Technologies, Pure Storage, HPE, Huawei, IBM), protocolos de armazenamento (FC, iSCSI, NVMe-oF, NFS, SMB, S3), tecnologias de data protection e fundamentos conceituais do setor — sempre com foco em conteúdo técnico objetivo, sem comparativos promocionais ou dados não verificáveis.

---

## 2. Stack Técnico

O site é uma aplicação **100% estática**, sem framework, sem build pipeline e sem dependências de servidor.

| Camada | Tecnologia |
|---|---|
| Markup | HTML5 semântico |
| Estilo | CSS3 puro (custom properties, Grid, Flexbox) |
| Lógica | Vanilla JavaScript (ES6+) |
| Markdown | [marked.js](https://marked.js.org/) v15.0.7 via CDN |
| Hospedagem | GitHub Pages |
| Domínio customizado | CNAME → `www.deepstorage.tech` |
| Fontes | Google Fonts: Inter + JetBrains Mono |

### Por que sem framework?

Decisão deliberada de zero dependências de build. Não há npm, webpack, vite, next.js nem nenhum processo de compilação. O site abre direto no browser a partir dos arquivos estáticos — ideal para hospedagem via GitHub Pages com deploy instantâneo por push.

---

## 3. Estrutura de Arquivos

```
my-blog/
├── index.html              # Homepage — listagem de posts + busca global
├── post.html               # Leitor de post individual (conteúdo via JS)
├── conceitos.html          # Base de conhecimento conceitual (longa)
├── recursos.html           # Página de recursos curados
├── about.html              # Sobre o blog
├── 404.html                # Página de erro customizada
├── blog.html               # Legado / redirecionamento
│
├── styles.css              # Sistema de design completo (~1800 linhas)
├── script.js               # Toda a lógica + dados do blog (~6700 linhas)
│
├── snapshot-diagram.svg    # Diagrama SVG: Como funcionam snapshots
├── replication-diagram.svg # Diagrama SVG: Replicação síncrona vs assíncrona
│
├── robots.txt              # Diretivas para crawlers
├── sitemap.xml             # Sitemap para SEO
├── CNAME                   # Domínio customizado do GitHub Pages
└── README.md               # Documentação básica original
```

### Observação sobre `script.js`

Todo o conteúdo do blog vive dentro de `script.js` — não há arquivos `.md` externos nem banco de dados. O arquivo contém:

- O array `posts[]` com todos os posts (id, title, tag, date, excerpt, content em Markdown)
- A string `conceitosContent` com toda a base de conhecimento em Markdown
- A função `buildRecursosHTML()` que gera HTML estruturado para a página de recursos
- Toda a lógica de renderização, busca, roteamento e navegação

---

## 4. Sistema de Design

### 4.1 Tokens CSS (`:root`)

```css
--bg:            #080808        /* fundo principal */
--surface:       #111111        /* cards, painéis */
--surface-2:     #1a1a1a        /* superfícies elevadas */
--surface-3:     #222222        /* chips, badges */
--border:        rgba(255,255,255,0.06)
--border-accent: rgba(0,255,159,0.18)

--primary:       #00ff9f        /* verde neon — destaque principal */
--primary-dim:   rgba(0,255,159,0.12)
--primary-glow:  rgba(0,255,159,0.25)
--secondary:     #00d9ff        /* ciano — destaque secundário */

--text:          #e8e8e8
--text-muted:    #8a8a8a
--text-subtle:   #555555

--font-body: 'Inter', system-ui, sans-serif
--font-mono: 'JetBrains Mono', monospace

/* Grade de espaçamento 8pt */
--sp-1: 4px  |  --sp-2: 8px   |  --sp-3: 12px  |  --sp-4: 16px
--sp-5: 20px |  --sp-6: 24px  |  --sp-8: 32px  |  --sp-10: 40px
--sp-12: 48px | --sp-16: 64px

/* Raios */
--r-sm: 4px  |  --r: 8px  |  --r-lg: 12px  |  --r-pill: 999px

/* Transições */
--t:    150ms ease-out
--t-md: 220ms ease-out
```

### 4.2 Tipografia

- **Corpo:** Inter, 16px base, line-height 1.6
- **Código inline / blocos:** JetBrains Mono
- **Hierarquia de peso:** 700 títulos, 600 subtítulos, 500 labels, 400 corpo

### 4.3 Identidade Visual

Tema escuro profundo com acentos em verde neon (`#00ff9f`) e ciano (`#00d9ff`). Sem elementos decorativos animados — escolha deliberada por clareza e performance. Estética técnica / terminal, coerente com o público-alvo (profissionais de infraestrutura).

---

## 5. Páginas

### 5.1 Homepage (`index.html`)

- Lista todos os posts em ordem cronológica inversa
- Card de cada post: título, data, tag badge colorido, excerpt, link "Ler mais"
- Busca global em tempo real (client-side) que pesquisa posts, Conceitos e Recursos
- Seção de posts em destaque (featured) no topo

### 5.2 Post Individual (`post.html`)

- Recebe `?id=N` na query string
- Busca o post no array `posts[]` em `script.js`
- Renderiza o campo `content` (Markdown) via `marked.parse()`
- Exibe: título, data, tag badge, corpo do post
- Suporte a tabelas GFM, blocos de código, listas, imagens inline
- Todos os links abertos em nova aba (`target="_blank"`)
- Imagens SVG embutidas diretamente no Markdown via tag `<img>`

### 5.3 Conceitos (`conceitos.html`)

Base de conhecimento técnica completa sobre storage enterprise. Conteúdo único, longo, em Markdown renderizado no browser.

**Seções cobertas:**

- **Fundamentos de mídia:** HDD (SAS, NL-SAS, SATA), SSD (SLC/MLC/TLC/QLC NAND, SATA vs SAS vs NVMe), SCM (Storage Class Memory), comparativo geral
- **RAID:** níveis 0/1/5/6/10/50/60, cálculo de capacidade útil, rebuild time
- **Tipos de armazenamento:** Block, File, Object — diferenças, casos de uso, comparativo
- **SAN — Storage Area Network:** Fibre Channel (FC), iSCSI, FCoE, FCIP, NVMe-oF; componentes, topologias, zoning
- **NAS — Network Attached Storage:** NFS, pNFS, CIFS/SMB (histórico, versões, SMB 1.0 deprecation, EternalBlue), multiprotocolo (NFS + CIFS no mesmo share, mapeamento de identidade LDAP/AD/RFC 2307/Winbind)
- **Object Storage:** S3, namespaces, metadata, políticas de lifecycle
- **Interconexão:** RDMA, RoCE v1/v2, InfiniBand, iWARP, comparativo de latência
- **Data Protection:** snapshots (CoW vs RoW, crash-consistent vs application-consistent), replicação (síncrona, assíncrona, near-sync), RPO/RTO, topologias de DR (two-site, three-site tiebreaker, cascade, fan-out), grupos de consistência, testes de DR

### 5.4 Recursos (`recursos.html`)

Diretório curado de links para profissionais. Implementado como HTML estruturado gerado dinamicamente (não Markdown). Seções:

| Seção | Formato | Qtd |
|---|---|---|
| Notícias & Reviews | Cards com link externo | 2 |
| Comunidades | Cards com link externo | 4 |
| Fabricantes | Vendor cards com sub-links | 7 |
| Certificações | Cert cards com código, nome e vendor badge | 8 |
| Blogs Independentes | Cards com link externo | 3 |
| YouTube | Cards compactos com ícone play | 8 |
| Newsletter | Card com link externo | 1 |
| Twitter / X | Pills horizontais | 6 |

### 5.5 About (`about.html`)

Apresentação editorial do blog: propósito, fabricantes cobertos, política de conteúdo (sem dados não verificáveis, sem comparativos promocionais).

### 5.6 404 (`404.html`)

Página de erro customizada com link de retorno ao homepage.

---

## 6. Posts Publicados

| ID | Título | Tag | Data |
|---|---|---|---|
| 1 | Pure Storage FlashArray//C: A Nova Era do All-Flash Acessível | Pure Storage | 2026-03-15 |
| 2 | NetApp ONTAP: Storage Unificado On-Prem e Cloud | NetApp | 2026-03-13 |
| 3 | NVMe-oF: O Futuro do Storage em Rede | Protocolos | 2026-03-05 |
| 4 | Hitachi Vantara: Portfólio VSP, HNAS, Ops Center e VSP One | Hitachi Vantara | 2026-03-14 |
| 6 | Snapshots: Como Funciona a Tecnologia por Trás da Proteção de Dados | Data Protection | 2026-03-22 |
| 7 | Replicação de Storage: Síncrona vs Assíncrona vs Near-Sync | Data Protection | 2026-02-15 |
| 9 | Huawei Storage: Dorado V6, V7 e OceanStor Pacific | Huawei | 2026-03-17 |
| 10 | Hitachi Vantara: Storage Unificado, Replicação e Alta Disponibilidade | Hitachi Vantara | 2026-03-18 |
| 11 | Hitachi Content Platform (HCP): Object Storage com Compliance Enterprise | Hitachi Vantara | 2026-03-16 |
| 12 | Dell EMC Storage: PowerStore, PowerMax e PowerScale | Dell Technologies | 2026-03-20 |
| 13 | HPE Storage: Alletra, 3PAR, Nimble e a Evolução do Portfólio | HPE | 2026-03-19 |
| 14 | A História do Storage: De Cartões Perfurados à NVMe | História | 2026-03-21 |

**Total:** 12 posts publicados

**Distribuição por tag:**

| Tag | Posts |
|---|---|
| Hitachi Vantara | 3 |
| Data Protection | 2 |
| Pure Storage | 1 |
| NetApp | 1 |
| Protocolos | 1 |
| Huawei | 1 |
| HPE | 1 |
| Dell Technologies | 1 |
| História | 1 |

---

## 7. Diagrams SVG

Dois diagramas técnicos foram criados especificamente para o blog, no mesmo estilo visual dark do site:

### `snapshot-diagram.svg`

Explica a tecnologia de snapshots (Copy-on-Write vs Redirect-on-Write). Usado no post id:6.

### `replication-diagram.svg`

Diagrama de sequência comparando replicação **Síncrona** vs **Assíncrona** (760×456px). Inclui:
- Painel esquerdo: fluxo síncrono de 4 passos (write → forward → ACK secundário → ACK app)
- Painel direito: fluxo assíncrono (write → ACK imediato → réplica em background)
- Footer comparativo: Síncrona | Near-Sync | Assíncrona com RPO e distância
- Legenda de tipos de seta

Usado no post id:7.

**Paleta dos SVGs:** fundo `#111111`, verde `#00ff9f`, ciano `#00d9ff`, cinza `#555`. Compatível com o tema do site.

---

## 8. SEO e Metadados

- **Open Graph** (`og:type`, `og:title`, `og:description`, `og:site_name`) em todas as páginas
- **Twitter Card** (`summary`) em todas as páginas
- **Canonical URL** explícito em cada página
- **`sitemap.xml`** com todas as URLs e datas de modificação
- **`robots.txt`** com `Allow: /` e referência ao sitemap
- **Meta description** individual por página

---

## 9. Acessibilidade e UX

- **Skip link** ("Pular para o conteúdo") em todas as páginas
- **`aria-label`** em todos os botões de ícone (hamburger menu, busca)
- **`aria-current="page"`** no item de navegação ativo
- **`aria-expanded`** no botão do menu mobile (atualizado via JS)
- **`aria-live="polite"`** nos resultados de busca em dropdown
- **`role="search"`** no campo de busca global
- **`alt` text** descritivo em imagens (incluindo SVGs)
- **Hierarquia de headings** (h1 → h2 → h3) sem saltos
- **`@media (prefers-reduced-motion)`** — desativa todas as transições e animações
- **Print styles** — oculta nav, footer, botões; expande links com URL

---

## 10. Responsividade

Breakpoints principais:

| Breakpoint | Comportamento |
|---|---|
| `< 480px` | Layout mobile, grid single-column, fontes menores |
| `480px–768px` | Tablet portrait, grid 2 colunas |
| `> 768px` | Desktop, grid multi-coluna, nav horizontal |

- Menu hamburger ativado em mobile (SVG icon, toggle JS inline)
- `grid-template-columns: repeat(auto-fill, minmax(..., 1fr))` em todos os grids
- Navbar fixa com `max-width` consistente
- `viewport-meta`: `width=device-width, initial-scale=1`

---

## 11. Busca Global

Implementada 100% no client-side em `script.js`:

- Escuta o input `#global-search` com debounce
- Pesquisa em: títulos de posts, excerpts, conteúdo de posts, `conceitosContent`, `recursosContent`
- Exibe dropdown `#search-results-dropdown` com resultados categorizados
- Navega para `post.html?id=N` ou para página estática conforme o tipo de resultado
- Fecha ao clicar fora ou pressionar Escape

---

## 12. Como Adicionar um Novo Post

Editar o array `posts` em `script.js`, adicionando um objeto com:

```javascript
{
    id: 15,                          // próximo ID disponível
    title: "Título do Post",
    tag: "Categoria",                // ex: "Protocolos", "Data Protection", etc.
    date: "2026-MM-DD",
    excerpt: "Resumo curto exibido na listagem.",
    content: `# Título do Post

Conteúdo em Markdown aqui.

## Seção

Texto...

\`\`\`bash
# Blocos de código com backticks escapados dentro do template literal
\`\`\`
`
}
```

**Atenção:** o campo `content` é uma template literal (backtick). Qualquer backtick inline dentro do Markdown (ex: `` `comando` ``) deve ser escapado como `` \` ``.

---

## 13. Deploy

O site é hospedado via **GitHub Pages**.

1. Commitar alterações em `script.js`, `styles.css` ou arquivos HTML
2. Push para o branch principal
3. GitHub Pages publica automaticamente em `www.deepstorage.tech`
4. Não há build step, CI/CD ou processo de compilação

Tempo de propagação típico: < 2 minutos após o push.

---

## 14. Decisões de Arquitetura

| Decisão | Razão |
|---|---|
| Todo o conteúdo em `script.js` | Zero infraestrutura de backend; sem CMS; deploy trivial |
| Markdown renderizado no browser | Conteúdo editável como texto puro; sem pré-processamento |
| SVGs inline/embutidos | Diagramas técnicos com controle total de estilo |
| Sem framework CSS | Evita dependências externas; CSS custom properties cobrem todas as necessidades |
| Recursos como HTML gerado por JS | Permite layout visual rico (cards, grids) impossível de atingir com Markdown puro |
| Sem analytics | Privacidade do leitor; sem cookies de terceiros |
| `prefers-reduced-motion` | Acessibilidade para usuários com sensibilidade a movimento |

---

## 15. Limitações Conhecidas

- **JavaScript obrigatório:** todo o conteúdo (posts, conceitos, recursos) é carregado via JS. Uma tag `<noscript>` exibe aviso, mas o site não funciona sem JS habilitado.
- **Sem paginação:** todos os posts são carregados de uma vez no array `posts[]`. Para muitos posts (> 50), considerar paginação ou lazy-loading.
- **Sem sistema de comentários:** publicação unidirecional.
- **Sem RSS/Atom feed:** leitores não podem assinar via feed reader.
- **SEO de conteúdo dinâmico:** o conteúdo dos posts é inserido via JS; crawlers que não executam JS (raro hoje) não indexam o texto completo.
- **Sem i18n:** idioma fixo em pt-BR.
