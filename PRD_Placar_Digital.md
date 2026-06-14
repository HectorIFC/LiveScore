# PRD — Placar Digital de Futebol (Chá de Fraldas)

## 1. Visão Geral

**Produto:** Placar eletrônico digital para partida de futebol amistosa, exibido durante um chá de fraldas.

**Objetivo:** Reproduzir digitalmente, em um único arquivo HTML, a experiência de um placar eletrônico físico (estilo LED, como o da imagem de referência), com gols, faltas, cronômetro, sons por ação e um ranking acumulado de times.

**Contexto de uso:** Evento social. Operado por uma pessoa (o "marcador") que controla o placar enquanto o jogo acontece. Público assiste pela tela.

---

## 2. Restrições Técnicas (não negociáveis)

| # | Restrição |
|---|-----------|
| R1 | Tudo em **um único arquivo `.html`** (HTML + CSS + JS embarcados, sem dependências externas, sem build). |
| R2 | **Sem barra de scroll** em nenhum dispositivo. Layout deve caber 100% na viewport (`100vh`/`100vw`). |
| R3 | **Responsivo** — funciona em Smart TV, notebook e celular, em orientação paisagem e retrato. |
| R4 | **Cores do Brasil** — verde, amarelo e azul como paleta principal. |
| R5 | **Multi-idioma** — Português (padrão), Inglês e Espanhol, alternável em tempo real. |
| R6 | **Sons por ação** — cada evento dispara um som distinto, gerados via Web Audio API (sem arquivos externos, respeitando R1). |
| R7 | Estilo visual **LED/7-segmentos** inspirado na imagem de referência. |

---

## 3. Layout do Placar (estilo da imagem)

Estrutura espelhando o painel físico de referência:

```
┌─────────────────────────────────────────────────────────┐
│   SET/FALTAS        TEMPO          SET/FALTAS             │
│      [0]            [20:00]            [0]                 │
│                                                           │
│   TIME A           PERÍODO           TIME B               │
│     [00]             [0]              [00]                │
└─────────────────────────────────────────────────────────┘
```

- **TEMPO:** cronômetro central grande (MM:SS).
- **TIME A / TIME B:** placar de gols (00–99) de cada lado.
- **SET/FALTAS:** contador de faltas por time (acima de cada placar, como na imagem).
- **PERÍODO:** indicador de tempo do jogo (1, 2...).
- Dígitos no estilo **LED vermelho 7-segmentos** sobre fundo escuro, labels em ciano/branco — fiel à referência.
- Aplicar a paleta brasileira em molduras, botões, fundo e detalhes (verde/amarelo/azul), mantendo os dígitos no vermelho LED característico.

---

## 4. Funcionalidades

### 4.1 Cronômetro (TEMPO)
- Tempo configurável (padrão sugerido: 20:00, como na imagem).
- Botões: **Iniciar / Pausar / Resetar**.
- Contagem regressiva. Ao chegar em 00:00 → dispara evento "fim de partida".
- Avanço de **PERÍODO** manual.

### 4.2 Gols
- Botão **+1 gol** e **−1 gol** para cada time (o −1 corrige erros).
- Gol do Time A → som A.
- Gol do Time B → som B (distinto do A).

### 4.3 Faltas (SET/FALTAS)
- Botão **+1 falta** e **−1 falta** por time.
- Falta → som de falta (mesmo som para ambos os times, OU um som por time — ver questão em aberto Q1).

### 4.4 Controle de Partida
- **Iniciar partida:** zera placar/faltas, inicia cronômetro, dispara som de início.
- **Encerrar partida:** para cronômetro, dispara som de fim, e **registra o resultado no leaderboard**.

### 4.5 Cadastro de times da partida
- Antes de iniciar, definir **nome/sigla** do Time A e Time B (ex: "BRA", "ARG").
- Sigla em formato curto (3–4 letras) para o leaderboard.

### 4.6 Sons (Web Audio API)
| Evento | Som |
|--------|-----|
| Gol Time A | Som 1 (ex: tom ascendente) |
| Gol Time B | Som 2 (ex: tom diferente) |
| Início de partida | Som de apito de início |
| Fim de partida | Som de apito final / buzina |
| Falta | Som curto de alerta |

> Todos sintetizados em runtime via `AudioContext` — nenhum arquivo de áudio externo, mantendo R1.
> Nota: navegadores exigem interação do usuário antes de tocar áudio. O primeiro clique (ex: "Iniciar") destrava o `AudioContext`.

---

## 5. Leaderboard (Ranking Acumulado)

Cada partida encerrada **alimenta** uma tabela acumulada de times.

### Colunas
| Coluna | Descrição |
|--------|-----------|
| **Time** | Sigla do time (ex: BRA) |
| **Gols** | Somatório de gols do time em **todas** as partidas jogadas |

### Regras
- Ordenação: do time que **mais** fez gols para o que **menos** fez (decrescente).
- Times são acumulados por sigla: se "BRA" jogar 3 vezes, os gols somam.
- Persistência: manter em memória durante a sessão. (Ver Q2 — não usar `localStorage` em ambiente de artifact; em arquivo HTML standalone aberto direto no navegador, `localStorage` funciona e dá persistência entre recargas — recomendado para o uso real.)

### Exemplo
| Time | Gols |
|------|------|
| BRA | 9 |
| ARG | 5 |
| URU | 2 |

---

## 6. Internacionalização (i18n)

Seletor de idioma sempre visível (PT / EN / ES). Troca instantânea de todos os textos da interface.

| Chave | PT | EN | ES |
|-------|----|----|----|
| Tempo | TEMPO | TIME | TIEMPO |
| Período | PERÍODO | PERIOD | PERÍODO |
| Faltas | SET/FALTAS | SET/FOULS | SET/FALTAS |
| Iniciar | Iniciar | Start | Iniciar |
| Pausar | Pausar | Pause | Pausar |
| Encerrar | Encerrar | End | Finalizar |
| Resetar | Resetar | Reset | Reiniciar |
| Gol | Gol | Goal | Gol |
| Falta | Falta | Foul | Falta |
| Ranking | Ranking | Leaderboard | Clasificación |

---

## 7. Responsividade & Telas-Alvo

| Dispositivo | Comportamento |
|-------------|---------------|
| **Smart TV** | Layout amplo, dígitos enormes, controles podem ficar discretos (modo apresentação). |
| **Notebook** | Layout paisagem completo com painel de controle. |
| **Celular** | Layout adaptado (vertical), dígitos legíveis, botões grandes tocáveis (touch). |

- Usar unidades fluidas (`vw`, `vh`, `clamp()`) para escalar dígitos.
- Garantir `overflow: hidden` no `body` (R2).
- Botões com alvo de toque ≥ 44px no mobile.

---

## 8. Estrutura de Telas (UI)

1. **Painel do Placar** (principal) — sempre visível, ocupa a viewport.
2. **Barra de Controle** — botões de gol/falta/cronômetro/idioma. Pode ser oculta/exibida (modo "limpo" para exibição na TV).
3. **Leaderboard** — visível em painel lateral ou modal acionável.
4. **Configuração rápida** — definir siglas dos times antes da partida.

---

## 9. Questões em Aberto (preciso confirmar com você)

- **Q1.** Som de falta: um único som para os dois times, ou um som distinto por time? (Você listou "som quando cada um dos times fizer uma falta" — interpreto que pode ser um som por time. Confirma?)
- **Q2.** Tempo padrão da partida: 20:00 (como na imagem) ou outro valor?
- **Q3.** O leaderboard deve persistir se a página for recarregada (via `localStorage`), ou pode zerar a cada abertura?
- **Q4.** Vai querer botão para limpar/resetar o leaderboard inteiro?

---

## 10. Critérios de Aceite

- [ ] Abre como arquivo único, sem internet, em TV/notebook/celular.
- [ ] Nenhuma barra de scroll aparece em qualquer tela.
- [ ] Cores do Brasil presentes na identidade visual.
- [ ] Três idiomas funcionam e trocam em tempo real.
- [ ] Cada um dos 5+ eventos toca um som distinto.
- [ ] Visual fiel ao painel LED da referência.
- [ ] Gols e faltas incrementam/decrementam por time.
- [ ] Cronômetro inicia, pausa, reseta e dispara fim.
- [ ] Encerrar partida grava no leaderboard.
- [ ] Leaderboard soma gols por sigla e ordena do maior para o menor.
