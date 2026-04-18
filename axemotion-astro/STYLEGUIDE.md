# Axe Motion Automation — Style Guide

## Brand Identity

**Full name:** Axe Motion Automation
**Tagline:** Excellence in System Integration and Industrial Automation
**Tone:** Technical, precise, international. Confidence sem arrogância. Foco em resultados mensuráveis.

---

## 1. Colors

### Primary Palette

| Token          | Hex       | Uso                                              |
|----------------|-----------|--------------------------------------------------|
| `--navy`       | `#0D1B2A` | Cor dominante do brand. Fundo hero, títulos dark |
| `--blue`       | `#1C6EE8` | Destaque de marca (palavra "AUTOMATION" no logo) |
| `--white`      | `#FFFFFF` | Texto sobre fundo escuro, cards, botões          |

### Neutral Palette (UI)

| Token          | Hex       | Uso                                              |
|----------------|-----------|--------------------------------------------------|
| `--black`      | `#000000` | Títulos em seções claras                         |
| `--gray-dark`  | `#9F9F9F` | Labels, metadados, textos secundários            |
| `--gray-mid`   | `#D8D8D8` | Bordas de cards e separadores                   |
| `--gray-light` | `#F5F5F5` | Background de seções neutras                     |

### Backgrounds por seção

| Seção               | Background |
|---------------------|------------|
| Hero                | `#000000`  |
| Who We Are          | `#F5F5F5`  |
| International Stats | `#F5F5F5` com mapa sutil como textura de fundo |
| Cards / contadores  | `#FFFFFF`  |

---

## 2. Typography

### Fonte principal: **Manrope**

Fonte sans-serif geométrica e moderna. Usar em todos os elementos da interface.
Google Fonts: `https://fonts.google.com/specimen/Manrope`

### Escala tipográfica

| Papel              | Tamanho | Peso | Letter-spacing | Cor      |
|--------------------|---------|------|----------------|----------|
| Hero H1            | 70px    | 900  | -3px           | `#FFFFFF`|
| Section H2         | 52px    | 800  | -2px           | `#000000`|
| Section H2 (tablet)| 42px    | 800  | -1.2px         | `#000000`|
| Section H2 (mobile)| 38px    | 800  | —              | `#000000`|
| Counter número     | 52px    | 800  | -4.5px         | `#000000`|
| Body text          | 17px    | 500  | —              | `#9F9F9F`|
| Label / eyebrow    | 13px    | 600  | +0.04em        | `#9F9F9F`|
| Button             | 15px    | 800  | —              | variável |
| Contact value      | 22px    | 800  | -0.3px         | `#000000`|

### Regras tipográficas

- **Labels / eyebrows** (ex.: "Who We Are", "International Experience") são sempre `UPPERCASE`, 13px/600, cor `#9F9F9F`. Aparecem acima do título da seção.
- **Títulos de seção** usam letter-spacing negativo para dar densidade visual.
- **Body** nunca usa preto puro — sempre `#9F9F9F` para criar hierarquia com os títulos.
- Line-height de corpo: `1.6`. Títulos: `1.1`.

---

## 3. Logo

O logo tem três elementos:

1. **Símbolo** — ícone circular com movimento diagonal (estilização de um eixo / axe em rotação), branco sobre fundo escuro.
2. **"AXE MOTION"** — tipografia bold, uppercase, branca.
3. **"AUTOMATION"** — tipografia com tracking amplo, uppercase, na cor `#1C6EE8` (azul de destaque).

### Uso do logo

| Contexto              | Versão recomendada          |
|-----------------------|-----------------------------|
| Fundo escuro (navy, preto) | Logo completo branco + azul |
| Fundo claro           | Logo versão escura (navy)   |
| Ícone / favicon       | Apenas o símbolo circular   |

### O que não fazer

- Não alterar as proporções do símbolo.
- Não usar o azul `#1C6EE8` em "AXE MOTION" — ele é reservado exclusivamente para "AUTOMATION".
- Não colocar o logo sobre fundos de baixo contraste sem ajustar a versão.

---

## 4. Spacing

O projeto usa uma base de `10px` como unidade mínima.

| Escala | Valor  | Uso típico                           |
|--------|--------|--------------------------------------|
| xs     | 10px   | Gap entre colunas de grid de cards   |
| sm     | 20px   | Gap interno de componentes           |
| md     | 30px   | Gap entre itens de lista / grid      |
| lg     | 40px   | Padding horizontal de seções         |
| xl     | 50px   | Padding interno de cards             |
| 2xl    | 70px   | Padding vertical de logo cells       |
| 3xl    | 120px  | Padding vertical de seções desktop   |

### Padding de seções por breakpoint

| Seção           | Desktop        | Tablet         | Mobile        |
|-----------------|----------------|----------------|---------------|
| Hero (top/bot)  | 120px / 206px  | 80px / 160px   | 60px / 120px  |
| Who We Are      | 120px / 120px  | 80px / 80px    | 60px / 60px   |
| Stats           | 120px / 120px  | 80px / 80px    | 60px / 60px   |
| Contact strip   | 0px / 120px    | 0px / 80px     | 0px / 60px    |

---

## 5. Components

### 5.1 Button — Filled (White)

```
background: #FFFFFF
color: #000000
border: 1px solid #FFFFFF
border-radius: 10px
padding: 20px 40px
font: 15px / 800 Manrope
```

Hover: `background: #F0F0F0`

### 5.2 Button — Filled (Black)

```
background: #000000
color: #FFFFFF
border: 1px solid #000000
border-radius: 10px
padding: 20px 40px
font: 15px / 800 Manrope
```

Hover: `background: #9F9F9F; border-color: #9F9F9F`

### 5.3 Button — Ghost / Text Link

```
background: transparent
color: #000000
border: none
padding: 0
font: 15px / 800 Manrope
```

Acompanha um ícone de seta (→) à direita, com `gap: 5px`.
Hover: `color: #686868`

### 5.4 Card (genérico)

```
background: #FFFFFF
border-radius: 10px
box-shadow: 0px 1px 20px 0px rgba(0,0,0,0.04)
padding: 50px
```

### 5.5 Counter Card

Variação do card genérico. Layout centrado, dois elementos empilhados:

```
.counter-number  → 52px / 800 / letter-spacing -4.5px / #000000
.counter-label   → 17px / 500 / #9F9F9F
gap entre os dois: 12px
```

### 5.6 Logo Cell (grid de países)

```
background: #FFFFFF
border: 1px solid #D8D8D8
padding: 70px 0
display: flex / align-items: center / justify-content: center
```

### 5.7 Contact Icon Badge

Ícone circular com fundo preto, bordas `border-radius: 100px`, padding `21px`.
SVG fill: `#FFFFFF`. Tamanho do ícone: `30px`.

### 5.8 Eyebrow Label

```
font-size: 13px
font-weight: 600
text-transform: uppercase
color: #9F9F9F
line-height: 1
letter-spacing: 0.04em
```

Sempre aparece acima do título principal da seção.

---

## 6. Layout & Grid

### Container

```
max-width: 1290px
margin: 0 auto
padding-inline: 40px (desktop) / 40px (tablet) / 20px (mobile)
```

### Seções de duas colunas (ex.: Who We Are)

| Coluna        | Desktop | Tablet  |
|---------------|---------|---------|
| Título/Label  | 32%     | 100%    |
| Descrição     | 48%     | 100%    |
| Ação (CTA)    | 20%     | 100%    |

No tablet, as três colunas empilham (`flex-direction: column`).

### Grid de contadores

```
Desktop: repeat(4, 1fr)  gap: 30px
Tablet:  repeat(2, 1fr)  gap: 30px
Mobile:  1fr             gap: 30px
```

### Grid de logos

```
Desktop: repeat(3, 1fr)  gap: 30px
Mobile:  repeat(1, 1fr)  gap: 30px
```

### Contact Card (overlap)

O card de contato usa `margin-top: -86px` para sobrepor o limite entre a seção hero (preta) e a seção clara (`#F5F5F5`), criando um efeito de "flutuação".

```
Desktop: margin-top: -86px
Tablet:  margin-top: -80px
Mobile:  margin-top: -60px
```

---

## 7. Breakpoints

| Nome    | Valor     |
|---------|-----------|
| Mobile  | ≤ 767px   |
| Tablet  | ≤ 1024px  |
| Desktop | ≥ 768px   |

---

## 8. Motion & Interaction

### Fade-in on scroll

Todos os blocos de conteúdo entram com fade + translate Y suave ao aparecerem na viewport.

```
@keyframes fadeIn {
  from { opacity: 0; transform: translateY(16px); }
  to   { opacity: 1; transform: translateY(0); }
}
duration: 0.6s  easing: ease  trigger: IntersectionObserver
```

Elementos dentro de uma mesma seção usam `animation-delay` escalonado de `0.1s`.

### Animação de contadores

Contadores animam de `0` até o valor final quando entram na viewport.

```
duration: 2000ms
easing: ease-out cúbico  (1 - (1 - t)³)
trigger: IntersectionObserver
```

### Hover em botões

Transição de `0.2s` em `background-color`, `color` e `border-color`.

---

## 9. Imagery & Backgrounds

- **Hero background:** cor sólida `#000000`. Nenhuma imagem ou textura.
- **Stats background:** `#F5F5F5` com mapa-múndi sutil como textura (`opacity` baixa, sem interferir na legibilidade).
- **Logos de países:** imagens com fundo branco, exibidas em cells com borda `#D8D8D8`.
- **Imagens de produtos / robôs:** quando usadas, `object-fit: cover`, full-bleed dentro do container.

---

## 10. Países de Atuação

A marca opera em 4 países. Representar sempre com bandeira + nome em negrito:

| País          | Bandeira |
|---------------|----------|
| Brazil        | 🇧🇷       |
| Germany       | 🇩🇪       |
| United States | 🇺🇸       |
| Argentina     | 🇦🇷       |

Nos logo cells da página, usar imagens PNG das bandeiras (arquivos em `./assets/`).
Em badges/pills (versão compacta), usar `background: #F0F0F0`, `border-radius: 999px`, `padding: 12px 20px`, texto `font-weight: 800`.
