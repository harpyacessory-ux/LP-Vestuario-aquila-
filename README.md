# LP Vestuário Aquila

Landing page da **Aquila Marketing Digital** — assessoria em Mercado Livre, Shopee e Amazon
para **donos de loja de vestuário** que vendem nos marketplaces.

## Público-alvo

Lojistas de moda e vestuário: loja física que migrou para o online, moda feminina/fitness/íntima,
multimarcas e revenda, confecção e marca própria. O copy é construído em torno das dores
específicas do nicho — grade de tamanhos, variações de cor e tamanho, devolução por tamanho
errado, tabela de medidas, coleção encalhada e margem por peça.

## Estrutura

- `index.html` — página única, autocontida (HTML + CSS + JS inline). Única dependência externa: Google Fonts (Sora + Inter).
- `vercel.json` — configuração de hospedagem estática na Vercel.

## Antes de publicar

Ajuste o bloco `CONFIG` no final de `index.html`:

| Campo | O que é |
|---|---|
| `whatsapp` | 55 + DDD + número, só dígitos (hoje está com um placeholder) |
| `deadline` | Fim da campanha, usado no contador regressivo |
| `vagasTotais` / `vagasRestantes` | Alimentam a barra de vagas |
| `msgPadrao` | Mensagem que abre no WhatsApp |

Os **depoimentos** na seção de resultados são citações reais de clientes. Ao trocá-los por
depoimentos de lojas de vestuário, mantenha o texto como o cliente escreveu e repita o mesmo
bloco duas vezes — a duplicação é o que faz o marquee rodar sem emenda.

## Rodar localmente

Basta abrir `index.html` no navegador. Para servir via HTTP (recomendado para testar âncoras e fontes):

```bash
python -m http.server 8000
# http://localhost:8000
```

## Deploy

Hospedada na Vercel, conectada a este repositório. Todo push na branch `main` publica automaticamente em produção.

## Tratamento da foto da seção "Para quem é"

`assets/lojista-vestuario.webp` é uma composição de dois arquivos que ficam na raiz
(fora do git, por serem PNGs de 2 MB):

- `Confident Marketplace Seller with Packing Supplies.png` — fornece o cenário
  (arara, caixas, bancada). É clareado, e a faixa central, onde havia outra
  pessoa, recebe desfoque forte para não aparecer "fantasma" atrás da lojista.
- `Smiling shop owner transparent cutout.png` — o recorte da lojista, que entra
  por cima em cor original.

Depois, as quatro bordas recebem esmaecimento em alpha e o resultado vira webp
com canal alfa. Para refazer:

```bash
ffmpeg -i "Confident Marketplace Seller with Packing Supplies.png" \
  -i "Smiling shop owner transparent cutout.png" -filter_complex "\
[0:v]curves=all='0/0.70 0.3/0.85 0.7/0.95 1/1',eq=saturation=0.85,format=gbrp,split[a][b];\
[a]gblur=sigma=2.5[s];[b]gblur=sigma=70[h];\
[s][h]blend=all_expr='A*(1-clip((X-480)/90,0,1)*clip((1290-X)/90,0,1))+B*clip((X-480)/90,0,1)*clip((1290-X)/90,0,1)',format=rgba[bg];\
[1:v]scale=-1:1100[fg];[bg][fg]overlay=504:70,crop=1336:1024:200:0,format=rgba,\
geq=r='r(X,Y)':g='g(X,Y)':b='b(X,Y)':a='255*min(1,min(min(X/380,Y/70),min((W-1-X)/120,(H-1-Y)/160)))'" \
  -frames:v 1 tmp.png
ffmpeg -i tmp.png -c:v libwebp -pix_fmt yuva420p -quality 86 \
  -compression_level 6 assets/lojista-vestuario.webp
```
