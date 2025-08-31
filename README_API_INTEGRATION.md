
# 🔗 Guia de Integração de APIs - LimpaBrasil

Este documento explica como integrar APIs reais no sistema LimpaBrasil que está atualmente usando dados simulados.

## 📋 Estado Atual

O sistema está **totalmente funcional** usando simulações, mas preparado para integração com APIs reais através de:

- ✅ Configuração centralizada (`src/config/apiConfig.js`)
- ✅ Serviços base (`src/services/apiService.js`)
- ✅ Estrutura preparada para produção
- ✅ Fallbacks para desenvolvimento

## 🔧 Configuração de Variáveis de Ambiente

Crie um arquivo `.env` na raiz do projeto com as seguintes variáveis:

```env
# URLs das APIs
REACT_APP_API_URL=http://localhost:5000/api
REACT_APP_AUTH_API_URL=http://localhost:5000/api/auth
REACT_APP_PAYMENT_API_URL=https://api.stripe.com/v1

# Chaves de API
REACT_APP_GOOGLE_MAPS_KEY=sua_chave_google_maps_aqui
REACT_APP_STRIPE_KEY=pk_test_sua_chave_stripe_aqui
REACT_APP_TWILIO_KEY=sua_chave_twilio_aqui
REACT_APP_WHATSAPP_TOKEN=seu_token_whatsapp_aqui

# Credenciais de Email
REACT_APP_EMAIL_USER=admin@agendaaqui.online
REACT_APP_EMAIL_PASS=sua_senha_email_aqui

# OAuth
REACT_APP_GOOGLE_CLIENT_ID=seu_google_client_id
REACT_APP_FACEBOOK_APP_ID=seu_facebook_app_id
```

## 🎯 APIs Prioritárias para Integração

### 1. **Autenticação JWT** (Prioridade: ALTA)

**Localização:** `src/services/authService.js`

**Para ativar produção:**
```javascript
// Linha 46 - Descomente para usar API real
if (this.isProduction) {
  return await this.loginWithAPI(email, password);
}
```

**Endpoints necessários no backend:**
- `POST /api/auth/login`
- `POST /api/auth/register`
- `POST /api/auth/refresh`
- `POST /api/auth/reset-password`

### 2. **Sistema de Pagamento** (Prioridade: ALTA)

**Localização:** `src/services/paymentService.js`

**APIs recomendadas:**
- **Stripe** - Cartões internacionais
- **PagSeguro** - PIX e cartões nacionais
- **Mercado Pago** - Alternativa nacional

**Para ativar:**
```javascript
// Linha 16 - Já preparado para produção
if (this.isProduction) {
  const url = apiService.buildUrl('payment', 'createIntent');
  return await apiService.post(url, paymentData);
}
```

### 3. **Google Maps API** (Prioridade: MÉDIA)

**Localização:** `src/services/mapsService.js`

**APIs necessárias:**
- Geocoding API
- Distance Matrix API
- Places API

**Para ativar:**
```javascript
// Configure a chave no .env e o sistema ativará automaticamente
REACT_APP_GOOGLE_MAPS_KEY=sua_chave_aqui
```

### 4. **Sistema de Email** (Prioridade: MÉDIA)

**Localização:** `src/services/emailService.js`

**Para ativar produção:**
```javascript
// Linha 33 - Descomente para usar API real
if (this.isProduction) {
  return await this.sendEmailViaAPI(emailData);
}
```

**Backend necessário:**
- Endpoint: `POST /api/email/send`
- Integração com Titan Email ou SendGrid

## 🚀 Processo de Migração por Etapas

### **ETAPA 1: Backend e Autenticação**
1. Criar API backend (Node.js/Express recomendado)
2. Implementar endpoints de autenticação JWT
3. Configurar banco de dados
4. Ativar autenticação real no frontend

### **ETAPA 2: Sistema de Pagamento**
1. Criar conta no Stripe/PagSeguro
2. Implementar endpoints de pagamento no backend
3. Configurar webhooks
4. Ativar pagamentos reais no frontend

### **ETAPA 3: Geolocalização**
1. Ativar Google Maps API
2. Configurar billing no Google Cloud
3. Implementar cálculo de preços por distância

### **ETAPA 4: Comunicações**
1. Configurar envio real de emails
2. Integrar WhatsApp Business API
3. Implementar SMS via Twilio

## 📁 Estrutura de APIs no Backend

### Estrutura recomendada para o backend:

```
backend/
├── src/
│   ├── controllers/
│   │   ├── authController.js
│   │   ├── bookingController.js
│   │   ├── paymentController.js
│   │   └── emailController.js
│   ├── middleware/
│   │   ├── auth.js
│   │   └── validation.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Booking.js
│   │   └── Payment.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── bookings.js
│   │   └── payments.js
│   └── services/
│       ├── emailService.js
│       ├── paymentService.js
│       └── mapsService.js
```

## 🔄 Como Alternar Entre Simulado e Real

Cada serviço tem uma flag de controle:

```javascript
// Em src/config/apiConfig.js
const API_CONFIG = {
  isDevelopment: process.env.NODE_ENV === 'development',
  // ...
};

// Nos serviços
if (this.isProduction) {
  // Usar API real
} else {
  // Usar simulação
}
```

## 🧪 Testando a Integração

### 1. **Testar Autenticação:**
```javascript
// No console do navegador
const authService = await import('./src/services/authService.js');
const result = await authService.default.login('test@test.com', '123456');
console.log(result);
```

### 2. **Testar Pagamentos:**
```javascript
const paymentService = await import('./src/services/paymentService.js');
const intent = await paymentService.default.createPaymentIntent(100);
console.log(intent);
```

## ⚠️ Pontos de Atenção

### **Segurança:**
- Nunca exponha chaves secretas no frontend
- Use variáveis de ambiente para todas as credenciais
- Implemente rate limiting no backend
- Valide todos os dados no servidor

### **Performance:**
- Implemente cache para requisições de geolocalização
- Use pagination para listas grandes
- Configure timeouts apropriados

### **Monitoramento:**
- Implemente logs estruturados
- Configure alertas para falhas de API
- Monitore custos das APIs externas

## 🔗 Links Úteis

- [Stripe Documentation](https://stripe.com/docs)
- [Google Maps Platform](https://developers.google.com/maps)
- [Twilio SMS API](https://www.twilio.com/docs/sms)
- [WhatsApp Business API](https://developers.facebook.com/docs/whatsapp)

## 📞 Suporte

Para dúvidas sobre a integração:
1. Verifique a documentação do serviço específico
2. Teste em ambiente de desenvolvimento primeiro
3. Use os logs para debugging
4. Consulte a comunidade de cada API

---

**Status:** ✅ Sistema preparado para integração
**Próximo passo:** Escolher e implementar a primeira API (recomendado: Autenticação)
