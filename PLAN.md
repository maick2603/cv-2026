# PLAN.md — CVs por vaga (cv-2026)

## Problema

Hoje cada candidatura é um markdown solto (os 6 atuais estão em `archive/`). O conteúdo é quase o mesmo, mas as **datas e métricas já divergiram** (a versão Revolut coloca Radix em mar/2023 e encurta a Vetta). Não tem pasta por vaga, nem JD, nem status. O digest diário de vagas não tem para onde aterrissar.

## Fonte da verdade

Um único `profile.yaml` com o que **não pode ser reescrito** numa customização:

- contato, cidade, idiomas
- datas canônicas (`roles[]`)
  - Radix — UX/UI Designer, remoto — mar/2026–presente
  - Vetta — Product Designer, remoto — set/2021–mar/2026
  - UTFPR — Web Designer — fev/2019–set/2021
  - Nós — PD voluntário — nov/2020–set/2021 (iF Social Impact 2021)
  - Bacharelado Design, UTFPR — 2018–2022
- pontos-chave de experiência (`experience[]`): cada bullet é um record com `id`, textos PT/EN, `keywords` e `metrics` (só número já presente nos CVs-fonte)
- stack em `skills.tools` / `skills.methods` / `skills.domains` / `skills.aliases`
- inglês avançado

Customizar = **reordenar e enfatizar** bullets + trocar o resumo + montar a linha de skills a partir dos `keyword_hits` da vaga. Nunca inventar data, cargo ou número. Se o fato mudou, edita o `profile.yaml`.

## ATS

O `cv.md` de cada vaga tem de ser **ATS-friendly**:

- Uma coluna só. Sem tabelas, text boxes, ícones, multi-coluna, headers/footers desenhados.
- Headings padrão, no idioma do template:
  - PT: **Resumo**, **Skills**, **Experiência**, **Formação**
  - EN: **Summary**, **Skills**, **Experience**, **Education**
- Bloco de **Skills perto do topo** (logo depois do resumo). Montado **só** a partir de `profile.skills` e `experience[].keywords` que **batem** com `job.keywords`. Palavra da JD que não existe no profile **não entra**.
- Espelhar a língua da JD: se o posting diz "Design System", usar **essa frase exata** nas skills **e** em pelo menos um bullet de experiência que já seja verdadeiro (não reescrever o fato; só alinhar o termo via `skills.aliases`).
- Cargo no header do CV pode espelhar o posting **somente** quando for alias honesto do que ele fez: **Product Designer** / **UX/UI Designer** / **Design System Designer**. Nunca inventar **Senior** num template pleno, a menos que a vaga seja sênior **e** o profile suporte (5 anos + ownership de DS = sênior ok).
- Datas consistentes no formato **mês/yyyy** (mar/2026, set/2021). Fonte: `profile.roles`.
- v1 continua em **markdown**. Export posterior deve ser **.docx** (ATS prefere Word, não PDF diagramado). **Fora de escopo neste PR:** gerar .docx de verdade.

Checklist operacional: [#11](https://github.com/maick2603/cv-2026/issues/11).

## profile.yaml — pontos-chave da experiência

Cada bullet de experiência é um **record**, não um dump de parágrafo:

```yaml
experience:
  - id: vetta-plasma-tokens
    company: Vetta
    text_pt: "..."
    text_en: "..."
    keywords: [design system, tokens, figma, documentation, docusaurus]
    metrics: []  # só números já nos CVs-fonte; vazio se não verificado
```

Também:

- `roles[]` — emprego com datas canônicas (a regra de ouro aplica aqui)
- `skills.tools`, `skills.methods`, `skills.domains`
- `skills.aliases` — mapa frase-da-JD → keyword canônica (ex.: `"design tokens"` → `tokens`)
- `metrics[].unverified: true` se o número aparece em **um** CV só (ou conflita). **Não usar** esse número em template / `cv.md`.

Seed dos keypoints: texto real dos CVs em `archive/` (Plasma UI / data-grid são trabalho verdadeiro do stack industrial; os CVs de archive falam só "Design System da empresa" — o record existe, sem métrica inventada). Sketch só no Revolut, básico → `skills.optional_weak`, nunca stuffing.

## Árvore proposta

```
cv-2026/
  profile.yaml
  templates/
    pd-senior.md
    pd-pleno.md
    design-system.md
    ux-en.md
  jobs/
    2026-08-nasajon-pd-senior/
      job.md      # URL, empresa, senioridade, must-haves, keywords, keyword_hits
      cv.md       # saída ATS, template + profile + hits
      STATUS      # draft | sent | closed
    2026-08-mayflower-ds/
      ...
  archive/        # os 6 CVs atuais (corpos intactos)
  PLAN.md
  README.md
```

## Templates

Quatro bases, não um CV por empresa. Cada uma declara: **idioma dos headings**, **stack default de `experience.id`**, e que a linha de skills é **montada de `keyword_hits`**.

| Template | Headings | Stack default de experience ids | Quando usar |
|---|---|---|---|
| `pd-senior.md` | PT (Resumo / Skills / Experiência / Formação) | `radix-manufacturing-dt`, `radix-design-system`, `radix-user-research`, `vetta-mes-modules`, `vetta-plasma-tokens`, `vetta-docusaurus`, `vetta-user-research`, `vetta-agile-designops`, `nos-if-prize` | Nasajon, FCamara, INGENIOUS, Customer.io — vaga sênior (5 anos + DS ownership = ok) |
| `pd-pleno.md` | PT | `radix-manufacturing-dt`, `radix-design-system`, `vetta-mes-modules`, `vetta-docusaurus`, `vetta-user-research`, `utfpr-moodle-sophia`, `utfpr-wordpress`, `nos-if-prize` | Starian, PicPay, Wave, Globo/GFT-like. **Não** escrever Senior no cargo. |
| `design-system.md` | PT | `vetta-plasma-tokens`, `vetta-data-grid`, `vetta-docusaurus`, `radix-design-system`, `vetta-mes-modules`, `radix-manufacturing-dt`, `vetta-agile-designops` | Mayflower, alt.bank (ênfase tokens / DS / Docusaurus) |
| `ux-en.md` | EN (Summary / Skills / Experience / Education) | `radix-manufacturing-dt`, `radix-design-system`, `radix-user-research`, `vetta-mes-modules`, `vetta-user-research`, `vetta-docusaurus`, `nos-if-prize`, `utfpr-moodle-sophia` | intl remote (Customer.io, Toybox, Mayflower). Usar `text_en`. |

Texto dos fatos vem do yaml. Tailor = ordem + quais ids puxar + resumo. Skills da vaga = interseção `job.keywords` ∩ (`profile.skills` ∪ `experience.keywords`), depois de resolver `aliases`.

## Intake a partir do digest

Quando o relatório diário apontar uma vaga para aplicar:

1. Criar `jobs/YYYY-MM-empresa-slug/`
2. Preencher `job.md`:
   - url, empresa, cargo, senioridade, remoto/híbrido
   - prazo
   - 3 must-haves copiados do JD (não inventar)
   - `keywords: []` — frases **exatas** copiadas do JD / digest
   - `keyword_hits: []` — preenchido no matching: quais `experience.id` + skills dispararam
   - template escolhido
3. Gerar `cv.md` (no v1: à mão, copiando o template e puxando bullets do profile cujo id está em `keyword_hits` **ou** no stack default do template)
4. `STATUS=draft` → revisar → `sent` ou `closed`

**Regra de matching:** uma keyword só aparece no CV se existir no profile (`skills.*` ou `experience[].keywords`), depois de `aliases`. Nunca stuffing de palavra da JD que o profile não tem.

Fora de escopo no v1: PDF, .docx, aplicar sozinho, scrapar JD, script obrigatório, gerador.

## Fases

1. **profile.yaml canônico** — datas **e** `experience[]` keypoints com `keywords` (não só datas). Extrair dos 6 CVs; métrica só se for a mesma na maioria das fontes. Bug Revolut (Radix 2023) ignorado. → [#8](https://github.com/maick2603/cv-2026/issues/8)
2. **Convenção de pastas** — migrar os 6 arquivos para `archive/` (feita no [PR #10](https://github.com/maick2603/cv-2026/pull/10)). → [#4](https://github.com/maick2603/cv-2026/issues/4)
3. **Quatro templates** — headings + ids default + skills via `keyword_hits`. → [#5](https://github.com/maick2603/cv-2026/issues/5)
4. **Checklist de intake** — digest → pasta `jobs/` com `keywords` / `keyword_hits`. → [#6](https://github.com/maick2603/cv-2026/issues/6)
5. **ATS + keyword matching** — checklist para o `cv.md` passar em parser ATS e só usar keywords que o profile tem. → [#11](https://github.com/maick2603/cv-2026/issues/11)
6. **(depois)** script mínimo `new-job.sh` só para carimbar a pasta vazia. → [#7](https://github.com/maick2603/cv-2026/issues/7)

## Exemplo de job.md

```yaml
company: Nasajon
role: Designer de Produto Sênior
url: https://nasajon.gupy.io/jobs/11638454
location: remoto BR, CLT
seniority: senior
template: pd-senior
apply_by: 2026-09-12
must_haves:
  - ERP / sistemas B2B complexos
  - pesquisa com usuário
  - alguma proximidade com código
keywords: []          # frases exatas copiadas do JD / digest
keyword_hits: []      # ids de experience + skills que dispararam no matching
status: draft
```

Matching (depois do intake): para cada item de `keywords`, resolver alias → canônico; se o canônico está em `skills` ou em algum `experience[].keywords`, anotar em `keyword_hits` (`skill:tokens` ou `experience:vetta-plasma-tokens`). O `cv.md` só imprime hits. Must-haves continuam obrigatórios na revisão humana — não são keywords.

## Regra de ouro

Se dois CVs discordam de uma data, o `profile.yaml` ganha. A versão de vaga está errada.
