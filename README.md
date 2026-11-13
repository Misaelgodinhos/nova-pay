# NOVA Pay 3.1 — Asaas Sandbox

Frontend: GitHub Pages. Backend: Supabase Edge Function `np-asaas-sandbox` no projeto `dthqzavxqoejqszsijqd`.

## Estado atual
- Login Supabase, saldo fictício e simulação local de Pix.
- Prévia de valor Asaas autenticada via Edge Function.
- **Não** existe conexão ativa com a API do Asaas, nem criação de clientes, cobranças, QR Codes, pagamentos, transferências ou webhooks.
- A função declara explicitamente `real_payments_enabled:false` e `payment_creation_enabled:false`.
- Os dados de outros projetos no Supabase não foram modificados nesta atualização.

## Preparação da integração
1. Criar conta de testes separada em https://sandbox.asaas.com/ (respeitando requisitos de cadastro e representação legal).
2. Guardar a chave do Sandbox somente em Supabase Edge Functions Secrets, com o nome `ASAAS_SANDBOX_API_KEY`. Não publicar no GitHub, HTML, logs ou conversas.
3. Base oficial de testes: `https://api-sandbox.asaas.com/v3`. Autenticação do Asaas usa header `access_token` e `User-Agent`.
4. Depois de configurar o segredo, desenvolver chamadas reais **somente ao Sandbox**, confirmação autenticada de webhooks, idempotência e trilha de auditoria.
5. Testar autorização, RLS, limites e concorrência antes de qualquer avanço.

Referências: https://docs.asaas.com/docs/sandbox e https://docs.asaas.com/docs/autenticacao-1
