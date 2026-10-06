# LP Varejo Aquila

Landing page da **Aquila Marketing Digital** — assessoria em Mercado Livre, Shopee e Amazon.

## Estrutura

- `index.html` — página única, autocontida (HTML + CSS + JS inline). Única dependência externa: Google Fonts (Sora + Inter).
- `vercel.json` — configuração de hospedagem estática na Vercel.

## Rodar localmente

Basta abrir `index.html` no navegador. Para servir via HTTP (recomendado para testar âncoras e fontes):

```bash
python -m http.server 8000
# http://localhost:8000
```

## Deploy

Hospedada na Vercel, conectada a este repositório. Todo push na branch `main` publica automaticamente em produção.
