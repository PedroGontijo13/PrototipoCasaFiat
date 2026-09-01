# Casa do Fiat — handoff para Figma

## Como levar para o Figma (2 caminhos)

**1. Importar como camadas editáveis (recomendado)**
1. No Figma, instale o plugin **html.to.design**.
2. Rode o plugin → aba *URL* → cole o link do arquivo autocontido (o link expira em ~1h; peça um novo quando precisar).
3. Importe em **1440px** (desktop) e depois em **390px** (mobile) para gerar os dois frames.
4. O plugin cria frames, textos, autolayout e cores reais. Depois: converta os blocos repetidos (card de categoria, linha de motor, unidade) em **Components**, e salve os tokens abaixo como **Variables**.

**2. Import de imagem (referência visual)**
Exporte o PNG da página e cole no Figma como camada de referência para redesenhar por cima.

> As animações não vão junto (Figma não importa CSS animation). Elas estão descritas no fim deste doc para virarem protótipo com Smart Animate.

---

## Tokens

### Cores
| Nome | Hex | Uso |
| --- | --- | --- |
| ink/base | `#171512` | fundo principal |
| ink/deep | `#0f0e0c` | topbar, faixa de modelos, motores, rodapé |
| ink/surface | `#201d19` | painel de CTA, mat das fotos |
| paper | `#f8f4ee` | títulos |
| paper/70 | `rgba(242,237,228,.66)` | corpo de texto |
| paper/45 | `rgba(242,237,228,.45)` | metadados |
| line | `rgba(242,237,228,.14)` | hairlines e bordas |
| red | `#e11d38` | CTA, marcas, regras de destaque |
| red/hover | `#ff3049` | hover do CTA |
| gold | `#e1ad66` | kickers, links, ações secundárias |
| green | `#0e9e52` | faixa tricolor (italiana) |

### Tipografia
- **Bodoni Moda** — display/títulos. H1 `clamp(46–94px)/0.98`, peso 400, tracking −0.02em. H2 34–56px. H3 24–26px (peso 500).
- **Archivo** — corpo e interface. Corpo 15px/1.8 peso 300. Rótulos e botões: 10.5–14px, peso 600, `font-stretch 118%`, tracking 0.06–0.24em, CAIXA ALTA.
- Números sempre tabulares (`tnum`).

### Grid e espaçamento
- Container 1240px, gutter 32px (20px no mobile).
- Espaçamento vertical entre seções: 104px (64px mobile).
- Raio: 4px (botões/cards), 999px no botão flutuante.
- Cards e painéis: borda 1px `line`, sem preenchimento sólido.

### Componentes a criar no Figma
`Botão/Primário` · `Botão/Secundário` · `Botão/Ouro` · `Card de categoria` (ícone, número, título, texto, link) · `Linha de motor` · `Card de unidade` · `Bloco de estatística` · `Nav item` (com underline animado) · `Botão flutuante WhatsApp`.

---

## Animações (para protótipo no Figma)
| Elemento | Animação |
| --- | --- |
| Barra tricolor do topo | progresso conforme scroll |
| H1 | 3 linhas subindo em cascata (delay 50/170/290ms) |
| Halo vermelho do hero | pulso de opacidade 7s |
| Foto do hero | parallax leve + brilho diagonal atravessando (4.6s) |
| Faixa de modelos | marquee infinito 38s |
| Seções | fade + subida de 28px ao entrar na viewport |
| Estatísticas | contagem 0 → valor (1.5s, ease-out) |
| Cards | sobem 6px, borda fica vermelha, seta desloca |
| Foto do motor | flutuação vertical 5s |
| CTA principal | anel vermelho pulsando |
| Links internos | scroll suave com easing |
