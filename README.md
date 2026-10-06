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

`assets/lojista-vestuario.webp` é uma composição de dois recortes com fundo
transparente, que ficam na raiz (fora do git, por serem PNGs de 2 MB):

- `Apparel packing station with boxes.png` — o cenário (caixas, arara, bancada).
- `Smiling shop owner transparent cutout.png` — a lojista, que entra na frente.

Os dois são montados sobre a cor de fundo da seção (`#F3F6FD`); topo, direita e
base recebem esmaecimento em alpha. O esmaecimento da esquerda é feito por
`mask-image` no CSS (ver comentário em `.pq-foto`). Para refazer:

```bash
ffmpeg -i "Apparel packing station with boxes.png" \
  -i "Smiling shop owner transparent cutout.png" -filter_complex "\
color=c=0xF3F6FD:s=1336x1024,format=rgba[bg];[0:v]scale=1336:-1[sc];\
[1:v]scale=-1:1100[fg];[bg][sc]overlay=0:H-h[t];[t][fg]overlay=434:70,format=rgba,\
geq=r='r(X,Y)':g='g(X,Y)':b='b(X,Y)':a='255*min(1,min(Y/70,min((W-1-X)/120,(H-1-Y)/160)))'" \
  -frames:v 1 tmp.png
ffmpeg -i tmp.png -c:v libwebp -pix_fmt yuva420p -quality 86 \
  -compression_level 6 assets/lojista-vestuario.webp
```
