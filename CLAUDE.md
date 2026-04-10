# Deep Storage — Contexto do Projeto para Claude

## O que é este projeto

Blog técnico independente sobre storage enterprise, publicado em `www.deepstorage.tech`. Audiência: profissionais de infraestrutura, arquitetos de storage, times enterprise.

Fabricantes cobertos: Hitachi Vantara, NetApp, Dell Technologies, Pure Storage, HPE, Huawei, IBM.
Temas: FC, iSCSI, NVMe-oF, NFS, SMB, S3, snapshots, replicação, RAID, data protection.

**Política editorial:** sem comparativos promocionais, sem dados não verificáveis, conteúdo técnico objetivo.

---

## Stack

Site **100% estático**, sem build pipeline, sem npm, sem framework.

| Camada | Tech |
|---|---|
| Markup | HTML5 semântico |
| Estilo | CSS3 puro (`styles.css`) |
| Lógica + Conteúdo | Vanilla JS (`script.js`) |
| Markdown | marked.js v15.0.7 via CDN |
| Hospedagem | GitHub Pages |

Deploy = push para main. Sem CI/CD. Propagação < 2 min.

---

## Arquivos principais

| Arquivo | O que contém |
|---|---|
| `script.js` | ~6700 linhas — array `posts[]`, `conceitosContent`, `buildRecursosHTML()`, toda a lógica |
| `styles.css` | ~1800 linhas — sistema de design completo com CSS custom properties |
| `index.html` | Homepage com listagem e busca global |
| `post.html` | Leitor individual (`?id=N`) |
| `conceitos.html` | Base de conhecimento técnica |
| `recursos.html` | Diretório curado de links |
| `about.html` | Sobre o blog |

**Todo o conteúdo vive em `script.js`.** Não há arquivos `.md` externos, CMS ou banco de dados.

---

## Design System

**Tema:** dark profundo, estética técnica/terminal.

```
--bg:       #080808    (fundo principal)
--surface:  #111111    (cards)
--primary:  #00ff9f    (verde neon — destaque principal)
--secondary:#00d9ff    (ciano — destaque secundário)
--text:     #e8e8e8
--font-body: Inter
--font-mono: JetBrains Mono
Espaçamento: grade 8pt
```

SVGs usam: fundo `#111111`, verde `#00ff9f`, ciano `#00d9ff`, cinza `#555`.

---

## Como adicionar um post

Inserir objeto no array `posts[]` em `script.js`:

```javascript
{
    id: 15,                    // próximo ID disponível (último: 14)
    title: "Título",
    tag: "Categoria",          // ex: "Protocolos", "Data Protection", "NetApp"
    date: "2026-MM-DD",
    excerpt: "Resumo curto.",
    content: `# Título

Conteúdo em Markdown.

\`\`\`bash
# backticks dentro do template literal devem ser escapados com \\
\`\`\`
`
}
```

Posts publicados atualmente: IDs 1, 2, 3, 4, 6, 7, 9, 10, 11, 12, 13, 14 (total: 12).

---

## Regras ao editar este projeto

1. **Nunca introduzir dependências externas** — sem npm, sem frameworks, sem bibliotecas novas além das já declaradas via CDN.
2. **Nunca criar arquivos de backend ou build** — o site deve continuar abrindo direto no browser.
3. **CSS sempre via custom properties** — usar os tokens já definidos no `:root` de `styles.css`; não hardcodar cores ou espaçamentos.
4. **Conteúdo dos posts é Markdown em template literals** — backticks inline devem ser escapados (`\``).
5. **Manter acessibilidade** — `aria-*` attributes, hierarquia de headings sem saltos, `alt` em imagens.
6. **Não quebrar a busca global** — ela pesquisa em `posts[]`, `conceitosContent` e `recursosContent`; qualquer novo conteúdo indexável deve ser exposto a essas fontes.
7. **SVGs novos** devem seguir a paleta do design system (`#111111`, `#00ff9f`, `#00d9ff`).

---

## Spec completa

Ver [SPEC.md](SPEC.md) para documentação detalhada de todas as páginas, seções de Conceitos, tabelas de Recursos, decisões de arquitetura e limitações conhecidas.
