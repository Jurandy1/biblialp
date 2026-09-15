# Checkout Cakto — como publicar

Arquivo: `cakto/checkout-config.json` (config do Checkout Builder com `desktop` + `mobile`,
banner de topo → bloco `checkout` → banner de rodapé, cores/fonte da LP, chat WhatsApp e exit popup).

## 1. Suba as imagens
Hospede `assets/banner-top.png` e `assets/banner-bottom.png` (ou faça upload na biblioteca da Cakto)
e troque `https://SEU-DOMINIO/...` pelas URLs finais nos 4 lugares do JSON.

## 2. Preencha os identificadores
- `offers`: id da oferta (ex.: `"zyb57dg"`).
- `chat.attributes.accountId`: número/ID do WhatsApp.

## 3. Descubra o id do checkout
```
curl "https://api.cakto.com.br/public_api/products/{PRODUCT_PK}/checkouts/" \
  -H "Authorization: Bearer $CAKTO_TOKEN"
```

## 4. Aplique a personalização
O corpo do PUT é `{ name, config, default, visits }` — o conteúdo de `checkout-config.json`
entra em `config` (os campos `name`/`default`/`offers` já estão no arquivo; mova-os conforme o corpo):

```
curl -X PUT "https://api.cakto.com.br/public_api/products/{PRODUCT_PK}/checkouts/{CHECKOUT_ID}/" \
  -H "Authorization: Bearer $CAKTO_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"name\":\"MD15 - Checkout Principal\",\"default\":true,\"config\":$(cat cakto/checkout-config.json)}"
```

## Observações
- Escopo necessário: `write products` (o token vem do fluxo OAuth com client id/secret).
- Nunca versione token, client id ou secret neste projeto — use variável de ambiente (`$CAKTO_TOKEN`).
  As credenciais enviadas no chat ficaram fora dos arquivos de propósito; se já circularam, gere novas.
- Os atributos do bloco `image` seguem o padrão do Checkout Builder (`src`/`alt`/`width`/`link`);
  se a sua conta usar nomes diferentes, monte um banner pelo builder visual e compare com este JSON.
- O upsell do Planner MD15 é configurado como **order bump / oferta adicional** no produto da Cakto —
  ele aparece dentro do bloco `checkout`, não como componente separado.
