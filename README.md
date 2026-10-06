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

`assets/lojista-vestuario.webp` sangra sobre o fundo claro sem moldura. Isso só
funciona porque o arquivo recebe três tratamentos antes de entrar no site: as
sombras são levantadas (o original tem fundo escuro), as quatro bordas ganham
esmaecimento em alpha e o resultado vira webp com canal alfa.

Para refazer a partir de um novo original:

```bash
FX="crop=960:1024:576:0,curves=all='0/0.50 0.25/0.70 0.6/0.88 1/1',format=rgba,\
geq=r='r(X,Y)':g='g(X,Y)':b='b(X,Y)':\
a='255*min(1,min(min(X/260,Y/130),min((W-1-X)/150,(H-1-Y)/130)))'"

ffmpeg -i original.png -vf "$FX" -frames:v 1 tmp.png
ffmpeg -i tmp.png -c:v libwebp -pix_fmt yuva420p -quality 86 \
  -compression_level 6 assets/lojista-vestuario.webp
```

Ajuste o `crop` ao enquadramento do novo arquivo. Se a foto for trocada sem esse
tratamento, aparece um retângulo de borda dura na seção.
