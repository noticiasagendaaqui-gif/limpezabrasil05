
# 💳 **INTEGRAÇÃO MERCADO PAGO - LIMPABRASIL**

## 🚀 **VISÃO GERAL**

Este documento descreve a implementação completa do gateway de pagamento Mercado Pago no sistema LimpaBrasil, incluindo pagamentos únicos e preparação para assinaturas recorrentes.

---

## 📋 **FUNCIONALIDADES IMPLEMENTADAS**

### **✅ Pagamentos Únicos por Serviço**
- Cartão de crédito/débito
- PIX com QR Code
- Validação de dados em tempo real
- Processamento seguro

### **✅ Interface Administrativa**
- Dashboard com estatísticas
- Configuração do gateway
- Histórico de transações
- Teste de conectividade

### **✅ Desenvolvimento & Produção**
- Modo Sandbox para testes
- Simulações realistas
- Logs detalhados
- Tratamento de erros

### **🚧 Preparado para Futuro**
- Estrutura de assinaturas
- Pagamentos recorrentes
- Gestão de planos
- Webhooks configurados

---

## 🔧 **CONFIGURAÇÃO**

### **1. Variáveis de Ambiente**

Adicione ao arquivo `.env`:

```bash
# Mercado Pago - Produção
REACT_APP_MERCADO_PAGO_PUBLIC_KEY=APP_USR-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
REACT_APP_MERCADO_PAGO_ACCESS_TOKEN=APP_USR-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

# Mercado Pago - Sandbox (Testes)
REACT_APP_MERCADO_PAGO_PUBLIC_KEY_SANDBOX=TEST-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
REACT_APP_MERCADO_PAGO_ACCESS_TOKEN_SANDBOX=TEST-xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

# URLs
REACT_APP_MERCADO_PAGO_URL=https://api.mercadopago.com
```

### **2. Obtendo as Chaves**

1. **Acesse:** [Mercado Pago Developers](https://www.mercadopago.com.br/developers)
2. **Crie uma aplicação** ou use uma existente
3. **Copie as chaves** de Produção e Sandbox
4. **Configure o webhook** para receber notificações

### **3. Webhook Configuration**

Configure o webhook no painel do Mercado Pago:
- **URL:** `https://seudominio.com/api/webhooks/mercadopago`
- **Eventos:** `payment`, `merchant_order`

---

## 🎯 **COMO USAR**

### **Para Administradores:**

1. **Acesse:** Painel Admin → Gateway de Pagamento
2. **Configure:** Chaves do Mercado Pago
3. **Teste:** Conexão com o botão "Testar Conexão"
4. **Monitor:** Transações em tempo real

### **Para Clientes:**

1. **Escolha** um serviço
2. **Clique** em "Contratar Serviço"
3. **Selecione** método de pagamento
4. **Preencha** dados do cartão ou use PIX
5. **Confirme** o pagamento

---

## 🔄 **FLUXO DE PAGAMENTO**

### **Cartão de Crédito/Débito:**
```
Cliente escolhe serviço → 
Modal de pagamento → 
Dados do cartão → 
Processamento → 
Aprovação/Recusa → 
Confirmação
```

### **PIX:**
```
Cliente escolhe PIX → 
Geração do QR Code → 
Cliente escaneia → 
Pagamento via app → 
Confirmação automática
```

---

## 📊 **ESTRUTURA DE DADOS**

### **Pagamento de Serviço:**
```javascript
{
  amount: 150.00,
  description: "Limpeza Residencial",
  bookingId: "booking_123",
  payer: {
    email: "cliente@email.com",
    firstName: "João",
    lastName: "Silva",
    identificationType: "CPF",
    identificationNumber: "12345678901"
  }
}
```

### **Resposta do Mercado Pago:**
```javascript
{
  id: 123456789,
  status: "approved",
  transaction_amount: 150.00,
  payment_method_id: "visa",
  date_created: "2025-01-23T10:30:00.000Z",
  external_reference: "booking_123"
}
```

---

## 🛠️ **DESENVOLVIMENTO**

### **Simulações Ativas:**
- ✅ 85% dos pagamentos aprovados
- ✅ PIX com QR Code simulado
- ✅ Delay realista de API
- ✅ Diferentes status de pagamento

### **Testes Disponíveis:**
```javascript
// Cartões de Teste
Visa: 4509 9535 6623 3704
Mastercard: 5031 7557 3453 0604
American Express: 3711 803032 57522

// PIX de Teste
Sempre aprovado em ambiente sandbox
```

---

## 🔐 **SEGURANÇA**

### **Implementado:**
- ✅ Chaves públicas para frontend
- ✅ Access tokens seguros no backend
- ✅ Validação de dados no cliente
- ✅ Processamento server-side
- ✅ Webhooks para confirmação

### **Recomendações:**
- 🔒 Nunca exponha access tokens no frontend
- 🔒 Use HTTPS em produção
- 🔒 Valide webhooks com assinatura
- 🔒 Implemente rate limiting
- 🔒 Log todas as transações

---

## 📈 **ASSINATURAS (FUTURO)**

### **Planejado para V2:**
```javascript
// Estrutura preparada
mercadoPagoService.createSubscriptionPlan({
  frequency: 1, // mensal
  amount: 99.90,
  description: "Plano Mensal LimpaBrasil",
  planId: "plan_premium"
});
```

### **Funcionalidades Futuras:**
- 🔄 Cobrança automática mensal/anual
- 📊 Dashboard de receita recorrente
- 🔔 Notificações de inadimplência
- ⬆️ Upgrade/downgrade de planos
- 📈 Métricas MRR/ARR

---

## 🚨 **TROUBLESHOOTING**

### **Erro: "Chave inválida"**
- Verifique se as chaves estão corretas
- Confirme se está usando sandbox/produção corretamente

### **Pagamento não aprova**
- Teste com cartões válidos
- Verifique configuração de webhooks
- Analise logs no painel MP

### **PIX não gera QR Code**
- Confirme conectividade com API
- Verifique dados do pagador
- Teste em ambiente sandbox

---

## 📞 **SUPORTE**

### **Mercado Pago:**
- 📘 [Documentação Oficial](https://www.mercadopago.com.br/developers/pt/docs)
- 💬 [Fórum de Desenvolvedores](https://forum.mercadopago.com.br/)
- 📧 [Suporte Técnico](https://www.mercadopago.com.br/developers/pt/support)

### **Sistema LimpaBrasil:**
- 🐛 Reporte bugs via Issues do GitHub
- 💡 Sugestões de melhorias
- 🚀 Contribuições são bem-vindas

---

## 📝 **CHANGELOG**

### **V1.8.0 - 23/01/2025**
- ✅ Integração completa Mercado Pago
- ✅ Pagamentos cartão e PIX
- ✅ Interface administrativa
- ✅ Simulações para desenvolvimento
- ✅ Preparação para assinaturas

---

**🎉 Sistema pronto para produção! Configure suas chaves e comece a receber pagamentos.**
