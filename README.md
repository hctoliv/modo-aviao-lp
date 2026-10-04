# Modo Avião — landing page

Landing page de apresentação da marca com CTA direto para a compra do ingresso no Sympla.

## Como abrir

Página estática, sem build. Abra `index.html` no navegador ou sirva a pasta:

```bash
python3 -m http.server 8777
```

## Estrutura

```
index.html        # a página inteira (HTML + CSS + JS inline)
assets/logo-modo-aviao.jpg   # o selo da marca (mesma arte do perfil do Instagram)
assets/img/                  # as 15 fotos do site (versões otimizadas, ~2,7 MB)
```

## Identidade

Paleta e tipografia extraídas da marca (logo, peças e site atual):

| Token | Hex | Uso |
|---|---|---|
| `--cream` | `#f8f3ec` | fundo base |
| `--card` | `#fdfaf4` | superfícies/cards |
| `--blush` | `#efe7da` | seções alternadas |
| `--border` | `#e3d9c8` | linhas |
| `--sand` | `#e8d58e` | amarelo do selo/logo |
| `--clay` | `#d8873a` | cor de ação (botões, destaques) |
| `--rose` | `#dfafb3` | apoio |
| `--sage` | `#8a9a7b` | botão do WhatsApp |
| `--coffee` | `#8c6a4e` | textos de apoio em caixa-alta |
| `--ink` | `#35312c` | texto principal |

Fontes: **Inter** (peso 400) na estrutura e **Cormorant Garamond** nos momentos de marca.

Referências de linguagem visual: **Aesop** (papel marfim, cantos retos, botão chapado
de 14px, grotesca peso 400, muito espaço em branco) e **Open** (títulos com entrelinha
curta, micro-rótulos em caixa-alta com tracking largo, texto estreito à esquerda com
imagem à direita, CTA com seta).

A serifada ficou reservada para o título do hero e os depoimentos. Todo o resto
(h2, subtítulos, numerais da agenda, menu) é Inter 400, como nas duas referências.

**Logotipo:** o selo circular do perfil do Instagram, em `assets/logo-modo-aviao.jpg`.
Usado no topo (50 px), no menu e no rodapé (72 px), e também como favicon. O arquivo
original tem 150 px, que é o máximo que o Instagram entrega sem login. Se você tiver o
logo em vetor ou em alta, é só substituir esse arquivo mantendo o mesmo nome.

Regras fixas desta página:

1. **Nenhuma imagem com texto/copy embutido.** Todo texto é HTML, para ficar legível,
   selecionável, traduzível e indexável.
2. **Nenhum travessão na copy.** Onde caberia um travessão, use ponto, vírgula,
   dois-pontos ou parênteses.
3. **A copy nunca fica sobre a foto.** O hero é dividido: texto em um painel marfim,
   foto ao lado. Isso garante contraste em qualquer foto que entrar no lugar.
4. **Cantos retos e sem sombra.** `--r: 0px`. Botões, chips, imagens e cards.
5. **Sempre declare `height:auto` em imagem com atributo `height`.** Sem isso o atributo
   HTML vira altura real e a foto estica (foi o bug que esticou a galeria no mobile).

Trocar a foto do hero: substitua `assets/img/hero.jpg` por uma imagem em retrato
(proporção aproximada 3:4). O enquadramento se ajusta por `object-position` no CSS
do bloco `.hero-media img`.

## Atualizar a agenda

Toda a agenda vem de um único array `EVENTOS` no final do `index.html`.
Cada item já carrega o link direto da página de compra no Sympla:

```js
{ d:"08", m:"OUT", dia:"Quinta-feira, 8 de outubro", hora:"19h30 às 22h30",
  titulo:"Pintura em Caneca", local:"Krog 721 · São Bernardo do Campo/SP",
  regiao:"sbc",                      // sbc | sa | sp  (usado no filtro)
  tags:["Público geral"],
  url:"https://www.sympla.com.br/evento/..." }
```

O **primeiro item da lista** é tratado como a próxima data: ele ganha o selo
"Próxima data" e os CTAs principais (topo, hero, final e barra fixa do mobile)
apontam automaticamente para a página de compra dele.

## Deploy

Pasta estática — sobe direto em Netlify, Vercel, Cloudflare Pages ou GitHub Pages.

## De onde vêm os números

Nada aqui é estimado. Tudo foi contado no histórico público da marca em 04/10/2026:

| Número | Fonte |
|---|---|
| **+30** encontros por mês | informado pela marca. Não verificável no que é público: a agenda aberta de outubro tem 17 datas e o histórico publicado soma 30 encontros desde jun/2025. O número presume a contagem dos eventos particulares, que não entram na agenda |
| **17** datas abertas em outubro | calculado em tempo real a partir do array `EVENTOS`, nunca desatualiza. O "abertas" é o que concilia esse número com o +30 mensal |
| **+100** pessoas na comunidade | número que a própria marca publica |
| **8** casas parceiras | Krog 721, De Sá, Mr. Texas, Perobah, Sweet Moments, Proce, Becos e Conect 1 |

> O site atual da marca exibe "+10 experiências realizadas". O histórico dele mesmo
> lista 30. Vale corrigir lá também.

## Carrossel do Instagram

As miniaturas ficam em `assets/insta/` e os links em `POSTS`, no fim do `index.html`.
Ficam locais de propósito: a URL do CDN do Instagram é assinada e expira em poucos dias,
então apontar direto para ela quebraria o carrossel sozinho.

Para atualizar, troque os arquivos e os links em `POSTS`.

Se quiser que ele se atualize sozinho, há dois caminhos:

1. **Instagram Graph API** — exige conta Business ou Creator ligada a uma página do
   Facebook, um app na Meta e um token de longa duração renovado a cada 60 dias.
   O endpoint é `/me/media?fields=id,media_url,permalink,caption,timestamp`.
   A API antiga, Basic Display, foi descontinuada em dezembro de 2024.
2. **Widget de terceiro** (LightWidget, EmbedSocial, Elfsight) — resolve em minutos,
   mas carrega script externo e a maioria cobra mensalidade.
