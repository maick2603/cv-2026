# Plano: CVs por vaga no GitHub (v1)

Repo: [maick2603/cv-2026](https://github.com/maick2603/cv-2026)  
Owner: **Maick Fonseca Maia** — Product Designer, Curitiba.

Este documento é o plano-pai. Issues de fase devem apontar para ele. **v1 não inclui gerador, renderer, CI, PDF, auto-apply nem scraping de JDs.**

---

## 1. Problema

Hoje cada candidatura vira um Markdown solto na raiz (ou em pastas acidentais). O padrão informal `{Role}-{Company}.md` e barras no nome do cargo (`UX/UI`, `Analista_UX/UI`) geram pastas sem querer e caracteres invisíveis nos nomes.

Os **fatos já divergem** entre os seis arquivos:

| Arquivo atual | Datas Radix / Vetta | Notas |
|---|---|---|
| `Product_Designer_(Platform)-Revolut.md` | Radix **mar/2023–presente**; Vetta até **mar/2023** | **Bug.** Datas erradas. |
| `UX_Designer-CI&T.md` | Radix mar/2026–presente; Vetta set/2021–mar/2026 | Alinhado ao canônico. |
| `UX_Designer_Pleno-Globo.md` | idem | Nome contém U+200B (zero-width space) em “Pleno”. |
| `UX_Designer_Pl 135300-GFT_Technologies.md` | idem | Nome contém U+202F no lugar de `eno-`. |
| `UX/UI_Designer_Pleno-PremierSoft.md` | idem | A “pasta” `UX/` veio da barra em `UX/UI`. |
| `Analista_UX/UI_-_Pleno-Jobgether.md` | idem | A pasta `Analista_UX/` veio da barra no cargo. |

Métricas também oscilam (ex.: consistência do DS 40% vs 30%; NPS, eficiência operacional, taxa de abandono só em alguns arquivos). Sem uma fonte única, cada digest diário tende a **inventar ou reescrever história** para caber na vaga.

O digest traz vagas PD/UX/Senior (Brasil remoto/híbrido Curitiba+SP e intl remote). O que falta é um fluxo GitHub-native: fatos canônicos → template → pasta da vaga → CV sob medida, **sem** duplicar a linha do tempo.

---

## 2. Árvore proposta

```
profile.yaml                 # única fonte de verdade (fatos)
templates/
  pd-senior.md
  pd-pleno.md
  design-system.md
  ux-en.md
jobs/
  _example/                  # stub de campos (não é vaga real)
    job.md
    STATUS
  YYYY-MM-empresa-slug/
    job.md                   # URL, empresa, senioridade, must-haves, apply-by
    cv.md                    # output sob medida
    STATUS                   # draft | sent | closed
archive/                     # os 6 CVs atuais, depois da migrate (fase 2)
README.md
PLAN.md
```

Não criar `jobs/` por cargo (`UX/`, `Analista_UX/`). Slug só com `[a-z0-9-]` (empresa + mês). Exemplo: `2026-08-revolut/`.

### `profile.yaml`

Chaves canônicas (identidade, datas de cargo, educação, idiomas, stack). **Nada de métrica não verificada.** Bullets e números só entram aqui depois de reconciliados na issue da fase 1. Stub vazio de chaves: [`profile.yaml`](./profile.yaml).

### `templates/`

Quatro recortes do mesmo histórico, não quatro biografias:

| Template | Quando usar |
|---|---|
| `pd-senior.md` | Product Designer sênior / liderança de produto |
| `pd-pleno.md` | Product Designer / UX pleno (maioria do digest BR) |
| `design-system.md` | Ênfase em tokens, DS, Docusaurus, handoff |
| `ux-en.md` | Vagas intl remote em inglês |

O template define **ordem das seções, resumo e quais bullets do profile enfatizar**. Não redefine datas.

### `jobs/YYYY-MM-empresa-slug/`

- **`job.md`**: metadados da vaga copiados do digest (URL, empresa, senioridade, 3 must-haves, apply-by, template escolhido). Stub: [`jobs/_example/job.md`](./jobs/_example/job.md).
- **`cv.md`**: CV gerado **na mão** na v1 (copiar template + reordenar bullets do `profile.yaml`). Sem renderer.
- **`STATUS`**: um de `draft` \| `sent` \| `closed` (arquivo de uma linha).

### `archive/`

Destino dos seis arquivos atuais na fase 2. **Nesta PR os corpos dos CVs não mudam.**

---

## 3. Regras (não negociáveis na v1)

1. **Nunca inventar datas nem métricas** num CV de vaga. Se o digest pede “+30% conversion” e isso não está no `profile.yaml`, não escrever.
2. **Só `profile.yaml` é fonte de verdade.** Templates e `cv.md` citam, reordenam ou omitem. Não criam fatos novos.
3. **Tailor = reordenar / enfatizar / trocar o resumo.** Não reescrever a história (cargos, períodos, empresas, prêmios).
4. Datas canônicas (consenso dos arquivos corretos + pedido deste plano):
   - **Radix** — UX/UI Designer, remote — **mar/2026 – presente**
   - **Vetta** — Product Designer, remote — **set/2021 – mar/2026**
   - **UTFPR** — Web Designer — **fev/2019 – set/2021**
   - **Nós (Nosso Olhar Solidário)** — Product Designer voluntário — **nov/2020 – set/2021** (iF Social Impact 2021)
   - **UTFPR** — Bacharelado em Design — **2018 – 2022**
5. Inglês: avançado. Stack canônica: Figma, tokens/DS, Docusaurus, HTML/CSS/JS/React/Tailwind.
6. O CV Revolut (`Product_Designer_(Platform)-Revolut.md`) **não** deve ser usado como fonte de datas.

---

## 4. Intake a partir do digest

Fluxo manual (checklist detalhado na issue da fase 4):

1. No digest, copiar **URL da vaga** + **3 must-haves** (não o JD inteiro; sem scraping).
2. Criar `jobs/YYYY-MM-empresa-slug/` (mês = mês do digest ou do apply-by).
3. Preencher `job.md` (ver stub) e `STATUS` = `draft`.
4. Escolher **um** template (`pd-senior` \| `pd-pleno` \| `design-system` \| `ux-en`).
5. Copiar o template para `cv.md` e **só então** tailor: resumo + ordem/ênfase dos bullets que já existem no `profile.yaml`.
6. Quando enviar: `STATUS` → `sent`. Quando a vaga fechar ou for recusada: `closed`.

Não implementar script nesta fase. Script opcional de “stamp folder” é a fase 5.

---

## 5. Fora de escopo (v1)

- PDF / export visual
- Auto-apply
- Scraping ou fetch do texto da JD
- Renderer, gerador, GitHub Action, CI
- Reescrever os seis CVs atuais (só mencioná-los e, na fase 2, movê-los para `archive/`)

---

## 6. Fases (issues)

Pai: [#9 Epic](https://github.com/maick2603/cv-2026/issues/9). Cada filha aponta para este `PLAN.md`. Títulos prefixados (`[phase-N]`); labels do repo: `documentation` / `enhancement` (não foi possível criar labels novas com o token deste agente).

| Fase | Issue |
|---|---|
| 1 profile.yaml | [#8](https://github.com/maick2603/cv-2026/issues/8) |
| 2 pastas + migrate | [#4](https://github.com/maick2603/cv-2026/issues/4) |
| 3 templates | [#5](https://github.com/maick2603/cv-2026/issues/5) |
| 4 intake | [#6](https://github.com/maick2603/cv-2026/issues/6) |
| 5 stamp (opcional) | [#7](https://github.com/maick2603/cv-2026/issues/7) |

### Fase 1 — `profile.yaml` canônico ([#8](https://github.com/maick2603/cv-2026/issues/8))

Extrair identidade, cargos, **datas reconciliadas** (corrigir o bug Revolut), educação, idiomas, stack. Listar bullets que aparecem em vários CVs; **métricas só entram se forem as mesmas em todas as fontes ou se houver evidência**. O que divergir fica de fora ou marcado `unverified: true` até o Maick confirmar — nunca no `cv.md`.

### Fase 2 — Convenção de pastas + migrate ([#4](https://github.com/maick2603/cv-2026/issues/4))

Criar `templates/`, `jobs/`, `archive/`. Mover os seis arquivos para `archive/` **sem editar o corpo**. Tabela de mapeamento (destino sugerido):

| Arquivo atual | Destino em `archive/` |
|---|---|
| `Product_Designer_(Platform)-Revolut.md` | `archive/2026-revolut-platform.md` |
| `UX_Designer-CI&T.md` | `archive/2026-cit-ux.md` |
| `UX_Designer_Pleno-Globo.md` | `archive/2026-globo-ux-pleno.md` |
| `UX_Designer_Pl 135300-GFT_Technologies.md` | `archive/2026-gft-ux-pleno.md` |
| `UX/UI_Designer_Pleno-PremierSoft.md` | `archive/2026-premiersoft-ux-pleno.md` |
| `Analista_UX/UI_-_Pleno-Jobgether.md` | `archive/2026-jobgether-analista-ux.md` |

Depois da migrate, apagar as pastas acidentais `UX/` e `Analista_UX/` se ficarem vazias. ASCII only nos nomes novos.

### Fase 3 — Quatro templates ([#5](https://github.com/maick2603/cv-2026/issues/5))

Redigir `pd-senior.md`, `pd-pleno.md`, `design-system.md`, `ux-en.md` **somente** com fatos do `profile.yaml`. Diferenças: resumo, ordem de seções (ex.: DS no topo no `design-system`), idioma no `ux-en`, densidade de bullets no sênior vs pleno.

### Fase 4 — Checklist de intake ([#6](https://github.com/maick2603/cv-2026/issues/6))

Documento curto (pode viver neste PLAN ou em `jobs/README.md`): como uma linha do digest vira uma pasta `jobs/…`. Campos obrigatórios, escolha de template, regra de ouro (não inventar), e quando mudar `STATUS`.

### Fase 5 — Opcional, depois ([#7](https://github.com/maick2603/cv-2026/issues/7))

Script mínimo que cria `jobs/YYYY-MM-slug/` a partir de um template (copiar arquivos, preencher placeholders). **Não** gera o texto do CV. Sem CI.

---

## 7. Critério de pronto desta PR (plano)

- [x] `PLAN.md` (este arquivo)
- [x] `README.md` apontando para o plano e a árvore
- [x] Stubs: `profile.yaml` (chaves, sem métricas inventadas) e `jobs/_example/job.md`
- [x] Issues de fase abertas neste repo, linkadas no corpo do PR (#9 pai; #8 #4 #5 #6 #7)
