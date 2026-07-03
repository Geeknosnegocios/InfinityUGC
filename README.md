# 🎬 Infinity UGC

> Crie UGCs perfeitos com IA em 4 passos — sem aparecer, sem gravar, sem editar.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)
[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](./SKILL.md)

Wizard interativo que guia você em 5 passos pra criar um UGC profissional
usando **ugcvideo.ai** + **Kling 2.5** + **Seedance 2.0**.

Gera prompts **prontos pra copiar e colar** em cada ferramenta. Sem API, sem
configuração complexa, sem aparecer em vídeo.

---

## O que é

Infinity UGC é uma **skill de agent** (um arquivo de instrução que qualquer
agent de IA consegue ler) que te guia em 5 passos pra gerar um UGC completo:

1. **Character** — define o avatar (persona) que vai aparecer no vídeo
2. **Script** — gera o roteiro falado
3. **Product Ad** — cria o frame inicial do vídeo
4. **Cenas + Animação** — divide o vídeo em cenas e recomenda qual IA de animação usar pra cada uma
5. **3 Hooks A/B/C** — gera 3 variações do hook (primeiros 3s) pra testar no Meta Ads / TikTok

Cada passo gera um arquivo `.md` com o prompt **exato** que você cola na
ferramenta correspondente (ugcvideo.ai, Kling, Seedance).

**Diferencial vs. outros criadores de UGC:**
- Sem aparecer em vídeo — avatar de IA
- Sem gravar áudio — script de IA
- Sem editar — prompts copy-pasteáveis
- Sem depender de API key — funciona em qualquer agent

---

## Pré-requisitos

- **Conta gratuita em ugcvideo.ai** — use o link
  👉 **https://www.ugcvideo.ai/?ref=GEEK** 👈
  pra ganhar **60 créditos grátis** (30 no cadastro + 30 na 1ª geração ≈ 3 UGCs completos)
- **Um agent que rode skills** — ver instalação abaixo (Claude Code, Codex, Gemini, Opus 4.8, GLM)

---

## Instalação

### Claude Code (recomendado)

```bash
git clone https://github.com/Geeknosnegocios/InfinityUGC.git \
  ~/.claude/skills/infinity-ugc
```

Pronto. Na próxima conversa, rode:

```
/infinity-ugc
```

### OpenAI Codex

Codex usa `AGENTS.md` ao invés de skills. Clone e copie o conteúdo:

```bash
git clone https://github.com/Geeknosnegocios/InfinityUGC.git
cat InfinityUGC/SKILL.md >> AGENTS.md
```

Depois é só falar no chat: "criar UGC" ou "infinity-ugc".

### Gemini CLI

```bash
git clone https://github.com/Geeknosnegocios/InfinityUGC.git \
  ~/.gemini/skills/infinity-ugc
```

Depois: `/infinity-ugc` ou "criar UGC".

### Claude (claude.ai / Opus 4.8 com projeto customizado)

No [claude.ai](https://claude.ai), crie um novo projeto e faça upload do
arquivo [`SKILL.md`](./SKILL.md) como "Project Instructions".

Depois é só conversar: "criar um UGC" ou "/infinity-ugc".

### GLM (ChatGLM) / outros agents

Qualquer agent aceita prompts customizados. Copie o conteúdo de
[`SKILL.md`](./SKILL.md) e cole no system prompt (ou "Custom Instructions" /
"Project Knowledge" / equivalente da sua plataforma).

Depois chame com o gatilho que configurar ("criar UGC", "infinity-ugc", etc.).

---

## Como usar

1. **Rode o comando** `/infinity-ugc` (ou fale "criar UGC")
2. **Dê um nome pro projeto** — ex: `skincare-mulheres-30` (kebab-case)
3. **Responda as 5 perguntas do wizard** (uma por vez, em cada passo)
4. **A skill gera** a pasta `ugc-[nome]/` com 5 arquivos `.md` + checklist
5. **Abra ugcvideo.ai** e cole cada prompt na ferramenta correspondente
6. **Gere o UGC** e suba no Meta Ads / TikTok / Reels

### Modo quick (só Character + Script)

```
/infinity-ugc quick
```

Gera só os primeiros 2 passos — ideal pra testar se a skill funciona antes
de gerar o pacote completo.

### Retomando um projeto interrompido

Se parou no meio, basta dizer:

```
continuar infinity-ugc do projeto skincare-mulheres-30
```

A skill retoma do último passo completo.

### Refazendo só um passo

```
refazer passo 3 do projeto skincare-mulheres-30
```

A skill relê os inputs dos passos anteriores pra manter coerência.

---

## Créditos

Feito com 🩵 por **Andrey — Geek nos Negócios**

📸 Siga no Instagram: [@geeknosnegocios](https://instagram.com/geeknosnegocios)

💡 Ganhe 60 créditos grátis no ugcvideo.ai:
**https://www.ugcvideo.ai/?ref=GEEK**

---

## Licença

[MIT](./LICENSE) — use, modifique, distribua à vontade. Se fizer algo legal
com a skill, me conta no Instagram 😉
