---
name: infinity-ugc
description: Wizard interativo pra criar UGC perfeito com ugcvideo.ai (Character + Script + Product Ad + Kling 2.5 / Seedance 2.0). Gera 5 prompts prontos pra copiar/colar nas ferramentas + 3 hooks A/B/C. Use quando o usuário pedir "criar UGC", "/infinity-ugc", "ugcvideo", "UGC com IA", ou quiser gerar criativos UGC sem aparecer em vídeo.
user-invokable: true
---

# Infinity UGC — Wizard de UGC Perfeito

Skill standalone que guia o usuário em 5 passos pra criar um UGC completo
(roteiro + avatar + vídeo) usando prompts copy-pasteáveis em:

- **ugcvideo.ai** → AI Influencer (Character Generator) + AI Script Writer + Product Ad Generator
- **Kling 2.5** ou **Seedance 2.0** → animação das cenas

**Diferencial vs. AGENTE-CRIATIVO do Geek OS:**
- AGENTE-CRIATIVO gera só o roteiro textual como parte de um kit de criativos Meta
- Infinity UGC gera o **pacote operacional completo** (5 prompts copy-pasteáveis + 3 hooks A/B/C)
- Infinity UGC é **standalone**: roda sozinha, não depende de briefing-ativo.md
- Infinity UGC é **distribuível**: vive em repo público, instalável em qualquer agent

**Não faz integração com API/MCP.** Gera prompts prontos pra copiar e colar.

---

## Comportamento geral

Quando o usuário invocar `/infinity-ugc` (ou "criar UGC", "ugcvideo", etc.):

1. **Cumprimenta** brevemente e pede o **nome do projeto** (slug kebab-case, ex: `skincare-mulheres-30`)
2. **Cria a pasta** `./ugc-[slug]/` no diretório atual (cwd do agent)
3. **Executa os 5 passos em sequência**, salvando cada output em arquivo separado
4. **Pode ser interrompida e retomada** — cada passo grava em arquivo separado, então basta dizer "continuar infinity-ugc do projeto [slug]"
5. **Modo quick:** se invocado como `/infinity-ugc quick` ou se o usuário pedir "só o personagem + script", roda só Passos 1 e 2 e salva em `00-quick.md`
6. **Ao final**, mostra mensagem de conclusão com os créditos

**Se o slug já existir**, pergunta: sobrescrever ou criar `ugc-[slug]-v2/`?
**Se o diretório atual não for writeable**, avisa e pede outro path.

---

## Passo 1 — Character (Persona)

**Objetivo:** definir o avatar que vai aparecer no vídeo.

**Inputs pedidos ao usuário** (um por vez, com sugestões contextuais quando o usuário não souber):

1. **Gênero:** mulher / homem / não-binário / sem preferência (se "sem preferência", sugere os mais comuns pro nicho)
2. **Faixa etária:** 18-25 / 26-35 / 36-45 / 46-55 / 55+
3. **Etnia:** se não souber, sugere 3 opções genéricas (latina / branca / negra) e deixa o usuário refinar depois
4. **Vibe:** girl next door / expert / mãe empreendedora / fitness coach / corporativo acessível / livre (descrição livre)
5. **Natural imperfections?** recomendável **sim** — a skill explica: "imperfeições naturais (sardas, pequenas cicatrizes, assimetrias) aumentam a credibilidade do UGC em ~40% porque o cérebro do espectador não detecta 'perfeição de IA'. Só desative se o nicho for beauty/luxo onde pele perfeita é esperado."

**Output:** salva `ugc-[slug]/01-character.md` com esse conteúdo:

```markdown
# Character · [slug]

## Inputs definidos
- Gênero: [valor]
- Idade: [faixa]
- Etnia: [valor]
- Vibe: [valor]
- Natural imperfections: sim/não

## Prompt pra colar no ugcvideo.ai → AI Influencer → Character Generator

[cole o prompt abaixo inteiro no Character Generator do ugcvideo.ai]

[PROMPT GERADO AQUI — ver regra abaixo]

## Próximo passo
Abra ugcvideo.ai → AI Influencer → Character Generator → cole o prompt
acima → salve o character. Use esse character em todos os passos seguintes.
```

### Regra de geração do prompt (Passo 1)

O prompt deve ser em **inglês** (a ferramenta responde melhor), com **80-120 palavras**, contendo nesta ordem:

1. **Persona base:** "A [idade] years old [etnia] [gênero] who looks like a real person, not a model"
2. **Atributos físicos** coerentes com etnia + idade + vibe (cabelo, formato de rosto, cor de pele)
3. **Expressão padrão:** "with a [vibe-adjetivo] expression, looking directly at the camera"
4. **Roupa sugerida** coerente com o nicho (ex: "wearing a casual t-shirt" pra girl next door, "wearing a blazer" pra corporativo)
5. **Fundo:** "on a neutral blurred background" (sempre neutro pra product ad)
6. **Natural imperfections:** se ativado, adicionar "with subtle natural imperfections like [exemplos: light freckles / small scar on chin / slight asymmetry]"

**Exemplo de prompt gerado (não copiar literalmente — adaptar pro caso):**
```
A 32 years old latina woman with medium brown wavy hair, oval face, warm brown skin, wearing a casual beige sweater, with a warm and approachable girl-next-door expression, looking directly at the camera, on a neutral blurred cream background, with subtle natural imperfections like light freckles across the cheeks and a tiny mole near the jawline.
```

---

## Passo 2 — Script Writer (Discurso)

**Objetivo:** gerar o roteiro falado do UGC.

**Inputs pedidos ao usuário:**

1. **Nome do produto** + link ou descrição curta (1 linha)
2. **Tipo:** físico / digital (app, SaaS) / serviço / infoproduto (curso, mentoria)
3. **Público-alvo específico** (ex: "mães 28-40 que vendem infoproduto", "donos de barbearia em cidades do interior")
4. **Tom:** casual e próximo / confiante e direto / engraçado / inspiracional
5. **Idioma do vídeo:** pt-BR / en / es
6. **Duração alvo:** 15s / 30s / 60s (isso define quantas cenas serão geradas no Passo 4)
7. **Hook style** (opcional): se não escolher, a skill recomenda automaticamente o mais forte pro nicho. Opções: pergunta provocativa / afirmação contraintuitiva / promessa específica / antes-e-depois verbal

**Output:** salva `ugc-[slug]/02-script.md` com esse conteúdo:

```markdown
# Script · [slug]

## Inputs definidos
- Produto: [nome] — [link ou descrição]
- Tipo: [físico/digital/serviço/infoproduto]
- Público: [descrição]
- Tom: [valor]
- Idioma: [pt-BR/en/es]
- Duração: [15/30/60]s
- Hook style: [escolhido ou auto-recomendado]

## Prompt pra colar no ugcvideo.ai → AI Script Writer

[cole o bloco abaixo inteiro]

PRODUCT: [nome do produto]
AUDIENCE: [público-alvo]
TONE: [tom escolhido]
LANGUAGE: [idioma]
DURATION: [duração em segundos]s
HOOK_STYLE: [hook escolhido]
GESTURE: [ver regra abaixo]

## Próximo passo
No ugcvideo.ai, abra o Script Writer, cole o prompt acima e gere o
roteiro. Salve o texto e use como base nos Passos 3 e 4.
```

### Regra de geração do GESTURE (Passo 2)

Escolha **um gesto** de acordo com tipo de produto + tom:

| Tipo de produto | Tom | Gesture sugerido |
|---|---|---|
| Físico | qualquer | segurando produto na altura do peito, mostrando pra câmera |
| Digital (app/SaaS) | casual | apontando pra tela imaginária ao lado |
| Serviço | confiante | mãos expressivas, gesticulando enquanto fala |
| Infoproduto | inspiracional | mãos no peito, tom de testemunho |
| Qualquer | engraçado | expressões exageradas com as mãos |

---

## Passo 3 — Product Ad (Frame Inicial)

**Objetivo:** gerar o frame inicial do vídeo (imagem estática do character com o produto, que será animada no Passo 4).

**Inputs pedidos ao usuário:**

1. **Tipo de cena:** segurando produto / usando produto / ao lado do produto / close-up do produto com mão visível
2. **Foto do produto:** se o usuário tiver, a skill pede o caminho e orienta a subir no ugcvideo.ai. Se não tiver, a skill orienta a gerar uma imagem simples do produto (Pillow/SVG local, ou qualquer imagem que o usuário achar).

**Output:** salva `ugc-[slug]/03-product-ad.md` com esse conteúdo:

```markdown
# Product Ad · [slug]

## Inputs definidos
- Cena: [tipo]
- Produto: [nome] (do Passo 2)
- Character: [nome/definição] (do Passo 1)

## Prompt pra colar no ugcvideo.ai → Product Ad Generator

[cole o prompt abaixo inteiro]

[PROMPT GERADO AQUI — ver regra abaixo]

## Workflow recomendado
1. Suba a foto do produto no Product Ad Generator
2. Selecione o character criado no Passo 1
3. Cole o prompt acima
4. Gere 3-5 variações
5. Escolha a que tiver melhor enquadramento + expressão natural
6. Essa imagem vira o start frame da animação (Passo 4)
```

### Regra de geração do prompt (Passo 3)

O prompt deve ser em **inglês**, com **100-150 palavras**, contendo:

1. **Reference:** "Using the character [detalhes do Passo 1] and the uploaded product image"
2. **Cena:** tipo definido pelo usuário
3. **Iluminação:** "soft natural lighting from the front-left"
4. **Fundo:** coerente com o nicho (ex: cozinha clean pra skincare, escritório moderno pra SaaS)
5. **Ângulo de câmera:** "medium close-up, eye-level"
6. **Expressão facial:** coerente com vibe do Passo 1
7. **Composição:** "product visible in [foreground/character's hand/background], well-framed"

---

## Passo 4 — Cenas + Animação

**Objetivo:** gerar prompts pra cada cena do vídeo, com recomendação de IA (Kling 2.5 vs Seedance 2.0).

**Input automático:** reutiliza character (Passo 1) + roteiro (Passo 2) + frame (Passo 3). Não pede novos inputs — gera sozinho baseado na duração definida no Passo 2.

**Lógica de duração:**

| Duração do Passo 2 | Nº de cenas | Estrutura |
|---|---|---|
| 15s | 2 | Hook (0-3s) + CTA (3-15s) |
| 30s | 3 | Hook (0-3s) + Demo (3-22s) + CTA (22-30s) |
| 60s | 4 | Hook (0-3s) + Demo (3-40s) + Prova (40-55s) + CTA (55-60s) |

**Recomendação de IA por cena:**

- **Kling 2.5** → pra cenas com movimento de corpo, câmera, produto (mais cinematográfico, ideal pra Hook e cenas de demonstração com produto)
- **Seedance 2.0** → pra talking head, foco em fala/gesto fluido (mais natural pra spokesperson, ideal pra Demo e CTA quando há diálogo forte)

A skill recomenda automaticamente baseado no tipo de cena, mas o usuário pode trocar.

**Output:** salva `ugc-[slug]/04-cenas-animacao.md` com esse conteúdo:

```markdown
# Cenas + Animação · [slug]

## Duração-alvo (do Passo 2): [15/30/60]s → [2/3/4] cenas

## IA recomendada: [Kling 2.5 / Seedance 2.0 / mix]

---

## Cena 1 — Hook (0-3s)
**Plataforma:** [Kling/Seedance — ver regra]
**Start frame:** a imagem escolhida do Passo 3
**Prompt visual:**
```
[descrição detalhada da cena — ver regra abaixo]
```
**Fala (do roteiro do Passo 2):** "[linha de hook]"
**Duração:** 3s

---

[repete para Cena 2, 3, 4 conforme duração]

---

## Workflow de montagem
1. Gere cada cena separadamente na IA escolhida
2. Exporte os 3-4 clipes em mp4
3. Una no CapCut / DaVinci / editor nativo do ugcvideo.ai
4. Adicione legendas automáticas (estilo Reels/TikTok)
5. Adicione trilha sonora baixa de fundo (opcional)
6. Exporte em 9:16 vertical, 1080x1920, 30fps
```

### Regra de escolha Kling vs Seedance

| Tipo de cena | Recomendação |
|---|---|
| Hook com expressão intrigada + pointing | Seedance (fala facial mais natural) |
| Demo segurando produto com gesto | Kling (movimento de corpo + produto) |
| Prova com depoimento | Seedance (talking head) |
| CTA apontando pra link | Seedance (fala + gesto direcional) |
| Cena com produto em uso visível | Kling (produto + movimento) |

### Regra de geração do prompt visual (Passo 4)

Cada prompt visual deve ter **40-60 palavras em inglês**, contendo:

1. **Action:** o que a pessoa está fazendo (fala, aponta, segura produto)
2. **Emotion:** expressão facial coerente com a linha do roteiro
3. **Camera movement:** "static camera" / "slow push-in" / "subtle handheld"
4. **Lighting:** "soft natural light from front" (padrão)
5. **Background:** "same background as start frame"
6. **Motion constraint:** "subtle and natural motion, no exaggerated movements"

---

## Passo 5 — 3 Hooks A/B/C

**Objetivo:** dar 3 variações de hook (primeiros 3 segundos) pra testar no Meta Ads / TikTok.

**Input automático:** pega produto + público + tom dos Passos 1 e 2. Não pede novos inputs.

**Output:** salva `ugc-[slug]/05-hooks-ab.md` com 3 hooks em estilos diferentes:

```markdown
# 3 Hooks A/B/C · [slug]

Pra testar no Meta Ads / TikTok. Use cada hook como a Cena 1 do vídeo,
mantendo as cenas seguintes iguais.

## Hook A — [estilo]
"[texto do hook]"
**Por que funciona:** [explicação de 1 linha]
**Substitui a fala da Cena 1 por:** "[novo texto]"

## Hook B — [estilo]
"[texto do hook]"
**Por que funciona:** [explicação]
**Substitui a fala da Cena 1 por:** "[novo texto]"

## Hook C — [estilo]
"[texto do hook]"
**Por que funciona:** [explicação]
**Substitui a fala da Cena 1 por:** "[novo texto]"

## Como testar
1. Gere 3 versões do vídeo, cada uma com um hook diferente
2. Suba os 3 como criativos separados no Meta Ads / TikTok Ads
3. Rode por 48-72h com mesmo budget
4. Pause os 2 piores, escale o vencedor
```

### Regra de geração dos 3 hooks

Os 3 hooks devem cobrir estilos diferentes, escolhidos deste pool:

| Estilo | Exemplo |
|---|---|
| Pergunta provocativa | "E se você pudesse [resultado] em [tempo]?" |
| Afirmação contraintuitiva | "[Coisa óbvia] tá errado. Vou te mostrar o porquê." |
| Promessa específica | "[Número] [pessoas] já conseguiram [resultado] com isso." |
| Antes e depois verbal | "Eu era [problema] há [tempo]. Hoje eu [resultado]." |
| Story hook | "Ontem eu tava [situação] quando descobri [produto]." |
| Urgência | "Se você [condição], você precisa ver isso AGORA." |

A skill escolhe 3 estilos diferentes do pool baseado no produto + público.

---

## Mensagem final (pós-Passo 5)

Ao completar os 5 passos, a skill mostra:

```
✅ UGC [slug] completo!

5 prompts prontos pra copiar e colar em:
- ugcvideo.ai (Character Generator, Script Writer, Product Ad Generator)
- Kling 2.5 ou Seedance 2.0 (animação das cenas)

Arquivos gerados em ./ugc-[slug]/:
- 01-character.md
- 02-script.md
- 03-product-ad.md
- 04-cenas-animacao.md
- 05-hooks-ab.md

💡 Créditos ugcvideo.ai: use o link https://www.ugcvideo.ai/?ref=GEEK
pra ganhar 60 créditos grátis (30 no cadastro + 30 na 1ª geração)

🩵 Feito com carinho por Andrey — Geek nos Negócios
📸 Me segue no Instagram: @geeknosnegocios → https://instagram.com/geeknosnegocios
```

---

## Retomada (interrupção)

Se o usuário disser "continuar infinity-ugc do projeto [slug]" ou similar:

1. Verifica se `./ugc-[slug]/` existe
2. Lê os arquivos `0X-*.md` já presentes
3. Identifica o último passo completo
4. Retoma a partir do próximo passo

Se o usuário disser "refazer passo X do projeto [slug]":

1. Lê os inputs do(s) passo(s) anterior(es) pra manter coerência
2. Re-executa só o passo pedido
3. Sobrescreve o arquivo correspondente

---

## Modo quick

Se invocado como `/infinity-ugc quick` ou "só personagem + script":

- Executa só Passos 1 e 2
- Salva tudo em `ugc-[slug]/00-quick.md` (arquivo único)
- Não executa Passos 3, 4, 5
- Mensagem final adaptada: "Aqui estão seus prompts de Character + Script. Depois é só rodar `/infinity-ugc` completo pra gerar o pacote todo."
