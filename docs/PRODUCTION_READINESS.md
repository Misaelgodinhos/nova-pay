# NOVA Pay — Preparação para produção (receber e enviar Pix)

## Estado: BLOQUEADO PARA PRODUÇÃO

Nenhuma credencial de produção, host de produção, webhook de produção ou endpoint de transferência real está habilitado. **Nunca** interpretar uma prévia, QR Code ou status do Sandbox como liquidação bancária.

## Arquitetura planejada

- Frontend GitHub Pages: solicita operações; jamais recebe credenciais do Asaas.
- Supabase Auth: autentica usuários; operações financeiras exigirão autenticação reforçada e autorização no servidor.
- Backend Edge Functions: exclusivamente ambiente Sandbox nesta fase; futura integração de produção em serviço segregado e sujeito a aprovação explícita.
- Banco: np_payment_intents, np_transfer_intents, np_provider_events, np_security_audit; RLS de leitura por proprietário e gravações somente pelo backend.
- Recebimento: criar cobrança -> QR -> webhook autenticado -> verificar estado diretamente com provedor -> conciliar -> registrar status; nunca creditar saldo baseado apenas no navegador.
- Envio: destinatário validado -> confirmação explícita -> limites e antifraude -> idempotência persistida -> aprovação adicional -> execução pelo provedor -> conciliação; **não implementado**.

## Homologação obrigatória

- Cadastro e aprovação de conta Asaas de produção, incluindo representante legal e elegibilidade.
- Confirmar com Asaas o modelo comercial e regulatório permitido para carteira/terceiros; API de transferências não concede automaticamente licença para custodiar recursos.
- Segregação de ambientes e credenciais; segredo nunca no GitHub nem no frontend.
- Webhook com segredo de autenticação, proteção contra replay, deduplicação e confirmação por API; retries idempotentes.
- Proteção contra envios duplicados, autenticação reforçada, rate limit, auditoria imutável, reconciliação diária e monitoramento.
- Testes de falha, reversão, indisponibilidade, concorrência, saldo, transferências recusadas, cobrança paga e não paga.
- Revisão de privacidade, LGPD, termos, suporte e responsabilidades operacionais.
- Aprovação formal para habilitar produção em mudança separada e revisada.

## Referências oficiais

- https://docs.asaas.com/docs/sandbox
- https://docs.asaas.com/docs/testando-transfer%C3%AAncias
- https://docs.asaas.com/docs/receba-eventos-do-asaas-no-seu-endpoint-de-webhook
- https://docs.asaas.com/docs/mecanismo-para-validacao-de-saque-via-webhooks
