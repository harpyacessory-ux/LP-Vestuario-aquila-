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
