# PLAN.md — CVs por vaga (cv-2026)

## Problema

Hoje cada candidatura é um markdown solto (`UX_Designer-CI&T.md`, `Product_Designer_(Platform)-Revolut.md`, …). O conteúdo é quase o mesmo, mas as **datas e métricas já divergiram** (a versão Revolut coloca Radix em mar/2023 e encurta a Vetta). Não tem pasta por vaga, nem JD, nem status. O digest diário de vagas não tem para onde aterrissar.

## Fonte da verdade

Um único `profile.yaml` com o que **não pode ser reescrito** numa customização:

- contato, cidade, idiomas
- datas canônicas
  - Radix — UX/UI Designer, remoto — mar/2026–presente
  - Vetta — Product Designer, remoto — set/2021–mar/2026
  - UTFPR — Web Designer — fev/2019–set/2021
  - Nós — PD voluntário — nov/2020–set/2021 (iF Social Impact 2021)
  - Bacharelado Design, UTFPR — 2018–2022
- bullets e métricas (25% tempo de módulo, tokens, Docusaurus, MES, etc.)
- stack: Figma, tokens/DS, HTML/CSS/JS/React/Tailwind, inglês avançado

Customizar = **reordenar e enfatizar** bullets + trocar o resumo. Nunca inventar data, cargo ou número. Se o fato mudou, edita o `profile.yaml`.

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
      job.md      # URL, empresa, senioridade, 3 must-haves, prazo
      cv.md       # saída, gerada a partir do template + profile
      STATUS      # draft | sent | closed
    2026-08-mayflower-ds/
      ...
  archive/        # os 6 CVs atuais, depois da migração
  PLAN.md
  README.md
```

## Templates

Quatro bases, não um CV por empresa:

| Template | Quando usar |
|---|---|
| `pd-senior.md` | Nasajon, FCamara, INGENIOUS, Customer.io |
| `pd-pleno.md` | Starian, PicPay, Wave, Globo/GFT-like |
| `design-system.md` | Mayflower, alt.bank (ênfase tokens/variables) |
| `ux-en.md` | vagas internacionais (Customer.io, Toybox, Mayflower) |

Cada template só define **ordem das seções e quais bullets do profile puxar**. Texto dos fatos vem do yaml.

## Intake a partir do digest

Quando o relatório diário apontar uma vaga para aplicar:

1. Criar `jobs/YYYY-MM-empresa-slug/`
2. Preencher `job.md`:
   - url, empresa, cargo, senioridade, remoto/híbrido
   - prazo
   - 3 must-haves copiados do JD (não inventar)
   - template escolhido
3. Gerar `cv.md` (no v1: à mão, copiando o template e puxando bullets do profile)
4. `STATUS=draft` → revisar → `sent` ou `closed`

Fora de escopo no v1: PDF, aplicar sozinho, scrapar JD, script obrigatório.

## Fases

1. **profile.yaml canônico** — extrair dos 6 CVs, resolver o bug de datas da Revolut
2. **Convenção de pastas** — migrar os 6 arquivos para `archive/` com uma tabela de mapeamento no README
3. **Quatro templates**
4. **Checklist de intake** — como uma linha do digest vira uma pasta em `jobs/`
5. **(depois)** script mínimo `new-job.sh` só para carimbar a pasta vazia

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
status: draft
```

## Regra de ouro

Se dois CVs discordam de uma data, o `profile.yaml` ganha. A versão de vaga está errada.
