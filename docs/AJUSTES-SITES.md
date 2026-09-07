# Ajustes — Sites Casa do Fiat & Jota Auto Peças

> Documento de trabalho. Última atualização: 2026-09-06.
> Fonte do pedido: briefing do cliente ("PROMPT DE AJUSTES — Site Casa do Fiat / Jota Auto Peças").

---

## 0. Status da implementação (2026-09-06)

**Casa do Fiat — aplicado** em `Casa do Fiat - Home.dc.html` e `index.html` (sincronizados):

| Ajuste | Feito | Observação |
| --- | --- | --- |
| 1. Tipografia | ✅ | Bodoni Moda + Archivo → **Inter** (link Google Fonts, tokens `--serif`/`--sans`, regra h1–h4, removidos `font-stretch`). Confirmar Inter com o cliente (P5). |
| 2. Remover UNIDADES | ✅ | Bloco, array `units`, links do menu e do rodapé, refs a `#unidades` no CSS/JS — todos removidos. |
| 2. Remover "Seja Parceiro" | ✅ n/a | Não existe na versão escura/`index.html`. Continua na `v1 claro` (ver item 1 / P7). |
| 3. Como funciona | ✅ | Novos 3 passos (contato → entrevista robô CRM → equipe responde) + subtítulo explicando que o CRM serve para agilizar. |
| 4. Faça seu orçamento | ✅ | Banner vermelho "MANDA O MODELO E O ANO. A GENTE ACHA A PEÇA." + subtítulo + botão "FAÇA SEU ORÇAMENTO". Bloco UNIDADES → **contatos por setor** (Vendas / Financeiro / Setor de Compras) + e-mail + horários. Telefones de Financeiro e Compras são placeholder `(31) 0000-0000` (P4). |
| 5. Galeria com abas | ✅ estrutura | Nova `<section id="estoque">` "Três galpões, o mesmo quarteirão." com 3 abas (Nosso Estoque / Galpão E-commerce / Galpão de Desmonte), troca via `state.galAtiva`. Fotos = `image-slot` placeholder (P3). Link "Estoque" no menu e no rodapé. |
| 6. Logos das marcas | ✅ estrutura | Faixa "Estoque amplo das três linhas Stellantis" com 3 `image-slot` (Fiat nova / Jeep cromada / RAM cromada) logo abaixo do hero. Textos "FIAT & JEEP" → "FIAT · JEEP · RAM" (topbar, header, rodapé). Marquee ganhou RAM 700, RAM 1500, Commander, Gladiator. Aguardando arquivos dos logos (P2). |
| 7. Jota Auto Peças | ⏳ | Fora deste repo. Pendente (P1). |

**Verificado no navegador:** página renderiza sem erros de console; abas da galeria alternam corretamente; banner vermelho e contatos por setor OK; Inter carregando.

**Falta fechar (Casa do Fiat):** trocar placeholders por assets reais (P2/P3/P4), regenerar `export/casa-do-fiat-home-standalone.html`, decidir sobre a `v1 claro` (P7), revisão fina de mobile.

---

## 1. Contexto e escopo

Todas as alterações abaixo devem ser aplicadas em **dois sites**:

| Site | Status no repositório | Arquivos |
| --- | --- | --- |
| **Casa do Fiat** | Existe neste repo | `Casa do Fiat - Home.dc.html` (versão escura — é a publicada), `index.html` (cópia idêntica servida no GitHub Pages), `Casa do Fiat - Home v1 claro.dc.html` (versão clara, protótipo antigo), `export/casa-do-fiat-home-standalone.html` (build autocontido para handoff) |
| **Jota Auto Peças** | **NÃO existe neste repo** | — pendente: cliente vai indicar o repositório/arquivos, ou decidiremos criar a partir da estrutura da Casa do Fiat |

Observações de arquitetura (Casa do Fiat):

- São páginas **Claude Design** (`.dc.html`): markup + um componente no bloco `<script type="text/x-dc">` no fim do arquivo, com `state`, `renderVals()` e diretivas `sc-for` / `sc-if`.
- Imagens entram por `<image-slot id="..." shape="rect" placeholder="...">` (ver `image-slot.js`).
- `index.html` precisa ser mantido **em sincronia** com `Casa do Fiat - Home.dc.html` (hoje são byte a byte iguais) — toda alteração no `.dc.html` deve ser replicada em `index.html`, ou gerada a partir dele.
- A versão "v1 claro" está desatualizada em relação à escura. **Decisão pendente:** atualizar as duas ou aposentar a clara. Recomendação: aposentar a `v1 claro` e trabalhar só na escura + `index.html`.

---

## 2. Ajuste 1 — Tipografia

**Estado atual:** `Bodoni Moda` (títulos, serifada) + `Archivo` (texto/UI), carregadas via Google Fonts.
Tokens: `--serif: "Bodoni Moda", ...` e `--sans: "Archivo", ...`.

**Alvo:** trocar por uma sans-serif moderna e comum, aplicada de forma consistente em **todo** o site (títulos e corpo).

- Fonte escolhida: **Inter** (recomendada) — alternativas aceitas: Montserrat, Poppins.
  - _Decisão pendente: confirmar Inter._
- Um único família para títulos e texto, variando só peso/tamanho.

**Onde mexer (Casa do Fiat):**

1. `<link>` do Google Fonts (linha ~15 do `.dc.html`): trocar `Bodoni+Moda` + `Archivo` por `Inter:wght@400;500;600;700`.
2. Tokens em `:root`: `--serif` e `--sans` → ambos apontando para Inter (ou renomear para `--font`).
3. Regra `h1,h2,h3,h4 { font-family: var(--serif) }` → passa a usar a mesma família; ajustar `font-weight` dos títulos (ex.: 600–700) e remover `font-optical-sizing`.
4. Remover `font-stretch: 118%` / `font-stretch: 112%` dos rótulos (era recurso do Archivo; Inter não tem eixo de largura) — compensar com `letter-spacing`.
5. Buscas a fazer no arquivo: `Bodoni Moda`, `Archivo`, `font-stretch`, `font-optical-sizing`, `var(--serif)`, `var(--sans)`, `var(--font-heading)`, `var(--font-body)` (este último na versão clara e no standalone).
6. Repetir em `index.html`, no standalone e (se mantida) na versão clara.

**Impacto visual:** títulos perdem o contraste serifado — revisar tamanhos/pesos dos H1/H2 do hero e das seções para manter hierarquia.

---

## 3. Ajuste 2 — Remover seções/elementos

### 2a. Seção "UNIDADES"
Listagem das outras lojas (Matriz Monsenhor Messias, Loja Peças Usadas, Centro de Motores).

**Onde está (Casa do Fiat escura):**
- Bloco `<div id="unidades" class="cdf-pad">` dentro da `<section id="orcamento">` (coluna direita), linhas ~390–400.
- Array `units: [...]` no `renderVals()`, linhas ~646–650.
- Link no menu: `<a class="cdf-navlink" href="#unidades">Unidades</a>` (linha ~203).
- Link no rodapé: `<a href="#unidades">Unidades</a>` (linha ~424).
- Scrollspy/CSS que referenciam `#unidades`: seletor `div[id="unidades"]` (linha ~34), `querySelectorAll("section[id], #unidades")` (linhas ~519), `#orcamento > div > div:first-child` no responsivo (linha ~88).

**Ação:** remover o bloco, o array `units`, os dois links de navegação e as referências a `#unidades` no CSS/JS. O espaço da coluna direita da seção orçamento será **reaproveitado** pelos contatos por setor (ver Ajuste 4).

### 2b. Seção / botão "Seja Parceiro"

- **Versão escura (`Casa do Fiat - Home.dc.html` / `index.html`): não existe.** Nada a fazer aqui.
- **Versão clara (`Casa do Fiat - Home v1 claro.dc.html`):** existe — `<section id="parceiro">` (linha ~235) e link `<a href="#parceiro">Seja Parceiro</a>` no menu (linha ~104). Remover ambos (ou aposentar o arquivo inteiro, conforme decisão do item 1).
- **Jota Auto Peças:** verificar e remover quando tivermos o arquivo.

---

## 4. Ajuste 3 — Seção "COMO FUNCIONA"

**Manter a seção**, atualizar o conteúdo dos 3 passos para refletir o processo real (contato → entrevista com robô do CRM → resposta da equipe).

**Onde está (Casa do Fiat escura):**
- `<section id="como-funciona">`, linhas ~342–366.
- Array `steps: [...]` no `renderVals()`, linhas ~641–645.
- Também há um link "Como funciona" no menu e no rodapé — mantidos.

**Novo conteúdo dos passos:**

| # | Título (sugestão) | Texto |
| --- | --- | --- |
| PASSO 01 | Você faz o contato | Chama no WhatsApp informando **modelo, ano e a peça** que precisa. |
| PASSO 02 | Entrevista rápida com o robô do CRM | Um atendimento automatizado coleta o máximo de informação — **códigos de peça, fotos, detalhes do veículo** — pra agilizar a busca. Leva poucos minutos. |
| PASSO 03 | A equipe responde | Retornamos com **preço, prazo e frete**. |

**Texto de apoio da seção (subtítulo):** deixar claro que o fluxo via CRM/robô existe **para tornar o atendimento mais rápido e prático**, não para burocratizar. Ajustar o parágrafo atual ("Três passos, sem cadastro e sem formulário longo...") e a linha "Resposta em até 24h úteis..." conforme o processo real.

**Pendências:**
- Confirmar textos exatos com o cliente.
- O link do CTA "Começar meu orçamento" continua indo pro WhatsApp (`waLink`).

---

## 5. Ajuste 4 — Seção "FAÇA SEU ORÇAMENTO" (CTA principal)

Substituir o layout atual pelo **estilo banner vermelho**, que o cliente aprovou como chamada principal.

**Onde está (Casa do Fiat escura):** `<section id="orcamento">`, linhas ~368–402. Hoje é um painel 2 colunas: à esquerda o CTA, à direita o bloco "UNIDADES".

**Alvo:**

1. **Banner vermelho** (fundo `--rosso` / `#e11d38`, largura total do painel) com:
   - Chamada: **"MANDA O MODELO E O ANO. A GENTE ACHA A PEÇA."**
   - Subtítulo: **"Atendimento pelo WhatsApp com orçamento no mesmo dia"**
   - Botão: **"FAÇA SEU ORÇAMENTO"** → `waLink`
2. **No lugar do bloco UNIDADES**, colocar **contatos por setor**:
   - `VENDAS — (31) ____` _(TBD — cliente vai fornecer)_
   - `FINANCEIRO — (31) ____` _(TBD)_
   - `SETOR DE COMPRAS — (31) ____` _(TBD)_
3. **Manter:** e-mail de contato (`vendas@casadofiat.com.br`) e horários (Seg–Sex 8h–18h · Sáb 8h–12h).

**Implementação:**
- Novo array no `renderVals()`, ex. `setores: [{ nome, telefone, href }]`, substituindo `units`.
- Ajustar o CSS responsivo que hoje cita `#orcamento > div > div:first-child`.
- Aplicar a mesma chamada/estilo no **hero** se o cliente quiser consistência (a chamada atual do hero é "Referência em peças Fiat novas e usadas") — _decisão pendente_.
- A pílula flutuante (`.cdf-fab` "Falar agora") e a topbar continuam.

**Pendências:** os 3 telefones por setor; confirmar se o e-mail/horário mudam.

---

## 6. Ajuste 5 — Nova seção de Galeria / Estoque (com abas)

Criar uma seção com **abas (tabs)** exibindo fotos reais. Não existe hoje uma seção "Nosso Estoque" com abas — será criada do zero (o briefing usa "Nosso Estoque existente" como referência de estilo; o que existe é só a foto única em `#motores` e menções em texto).

**Abas:**

| Aba | Conteúdo |
| --- | --- |
| **NOSSO ESTOQUE** | Fotos do estoque de peças |
| **GALPÃO E-COMMERCE** | Fotos do galpão de e-commerce |
| **GALPÃO DE DESMONTE** | Fotos do galpão de desmonte |

**Mensagem-chave da seção:** todos ficam **no mesmo quarteirão** — reforçar força e estrutura da operação (frase de destaque + talvez um selo "tudo no mesmo quarteirão").

**Implementação (Casa do Fiat):**
- Nova `<section id="estoque">` (ou `#galeria`), inserida provavelmente após `#motores` ou após `#produtos`.
- Estado no componente: `state = { ..., abaAtiva: 0 }`; array `galerias: [{ label, fotos: [image-slot...] }]`.
- Botões de aba trocam `abaAtiva`; grid de `image-slot` renderizado por `sc-for` a partir da aba ativa.
- Adicionar link no menu e no rodapé ("Estoque").
- **Fotos: placeholders agora** — `image-slot` vazios rotulados (ex.: "Estoque — prateleira 01"); cliente sobe as fotos depois via `uploads/`.
- Reaproveitar animações existentes (`data-reveal`, hover de card).

**Escopo:** aplicar **na Casa do Fiat e na Jota Auto Peças**.

**Pendências:** fotos reais; posição exata da seção; nome do link no menu.

---

## 7. Ajuste 6 — Logos das marcas

**Estado atual:** não há logos de marca renderizadas — só a barra tricolor italiana e o texto. Menções a "FIAT & JEEP" em vários pontos (topbar, header sublabel, rodapé).

**Alvo:** exibir **3 logos juntas em destaque** (Fiat + Jeep + RAM — grupo Stellantis), comunicando estoque amplo das três linhas.

| Marca | Versão pedida |
| --- | --- |
| **Fiat** | Logo **nova**: wordmark "FIAT" em caixa alta + barras com a bandeira da Itália, fundo escuro |
| **Jeep** | Versão **cromada / metalizada** |
| **RAM** | Versão **cromada / metalizada com escudo** |

**Implementação:**
- Nova faixa/bloco "Trabalhamos com" (provável: logo após o hero ou no rodapé), com os 3 logos alinhados, altura consistente, em `image-slot` ou SVG.
- Atualizar textos de "FIAT & JEEP" para "**FIAT · JEEP · RAM**" na topbar, no sublabel do header e no rodapé.
- Atualizar `models`/`engines` se o cliente quiser incluir modelos Jeep/RAM na marquee.

**Pendências (bloqueador):** **cliente vai enviar os arquivos dos 3 logos.** Até lá, usar slots marcados `FIAT NOVA`, `JEEP CROMADA`, `RAM CROMADA`.

---

## 8. Ajuste 7 — Escopo: replicar na Jota Auto Peças

Todos os ajustes 1–6 valem para os **dois sites**.

- **Casa do Fiat:** implementar direto neste repo.
- **Jota Auto Peças:** aguardando o cliente indicar o repositório/arquivos. Quando chegar:
  - Ajustes 1, 3, 4 (tipografia, "como funciona", banner) → adaptar textos à identidade da Jota.
  - Ajustes 2, 5, 6 (remoções, galeria com abas, logos Fiat/Jeep/RAM) → aplicar igual.
  - Confirmar telefones/e-mail próprios da Jota.

---

## 9. Pendências — dados que o cliente precisa fornecer

| # | Item | Status |
| --- | --- | --- |
| P1 | Repositório / arquivos do site **Jota Auto Peças** | Aguardando |
| P2 | Arquivos dos **3 logos** (Fiat nova, Jeep cromada, RAM cromada) — PNG/SVG | Aguardando ("vou mandar depois") |
| P3 | **Fotos** das 3 abas de galeria (estoque, galpão e-commerce, galpão desmonte) | Placeholders por enquanto |
| P4 | **Telefones por setor**: Vendas, Financeiro, Setor de Compras | Placeholder `(31) ____` |
| P5 | Confirmar **fonte** (Inter recomendada) | Aguardando |
| P6 | Textos finais da seção "Como funciona" (fluxo do robô/CRM) | Rascunho neste doc |
| P7 | Decisão sobre a versão **"v1 claro"**: atualizar ou aposentar | Recomendação: aposentar |
| P8 | Endereço/quarteirão exato para a frase "todos no mesmo quarteirão" | Aguardando |

---

## 10. Checklist de implementação — Casa do Fiat

- [x] **Tipografia**: trocar fontes (link, tokens, regras h1–h4, remover `font-stretch`) no `.dc.html` + `index.html`
- [x] **Remover UNIDADES**: bloco, array `units`, links (menu + rodapé), refs CSS/JS a `#unidades`
- [ ] **Remover "Seja Parceiro"**: só na versão clara (ou aposentar o arquivo) — pendente decisão P7
- [x] **Como funciona**: reescrever os 3 `steps` + subtítulo da seção
- [x] **Faça seu orçamento**: banner vermelho ("MANDA O MODELO E O ANO...") + subtítulo + botão
- [x] **Contatos por setor**: substituir bloco UNIDADES por Vendas/Financeiro/Compras + e-mail + horários (tel. Financeiro/Compras = placeholder)
- [x] **Galeria com abas**: nova seção `#estoque`, estado `galAtiva`, 3 abas, image-slots placeholder, link no menu/rodapé
- [x] **Logos das marcas**: faixa com Fiat/Jeep/RAM (placeholders), textos "FIAT & JEEP" → "FIAT · JEEP · RAM"
- [x] **Sincronizar `index.html`** com o `.dc.html`
- [ ] Trocar `image-slot` placeholder pelos assets reais (logos P2, fotos P3) e telefones reais (P4)
- [ ] Revisar responsivo (mobile) de todas as seções alteradas
- [ ] Regenerar `export/casa-do-fiat-home-standalone.html`
- [ ] Decidir/atualizar `Casa do Fiat - Home v1 claro.dc.html` (P7)

### Referência rápida — onde ficou cada coisa (`Casa do Fiat - Home.dc.html`)

| Elemento | Localização |
| --- | --- |
| Fonte Inter | `<link>` no `<helmet>`; tokens `--serif`/`--sans` em `:root`; regra `h1,h2,h3,h4` |
| Faixa de logos | `<section aria-label="Marcas com estoque amplo">` logo após `#top` — slots `cdf-brand-fiat/jeep/ram` |
| Galeria com abas | `<section id="estoque">` entre `#motores` e `#como-funciona`; CSS `.cdf-tab`/`.cdf-tab.is-active`; estado `galAtiva`, flags `galIs0/1/2`, array `galTabs` |
| Banner vermelho | `<section id="orcamento">` → `.cdf-orc-banner` |
| Contatos por setor | `.cdf-setores` dentro de `#orcamento`; array `setores` no `renderVals()` |
| Passos "Como funciona" | array `steps` no `renderVals()` |

## 11. Checklist — Jota Auto Peças

- [ ] Obter acesso ao projeto (P1)
- [ ] Repetir todos os itens acima, adaptando textos/contatos à identidade da Jota
