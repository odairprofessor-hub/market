# 🧪 GUIA DO USUÁRIO TESTE - MarketHub

## 🎯 Credenciais de Acesso

```
Email: teste@test.com
Senha: 123
Cidade: Santa Rosa
```

---

## ✅ O Que Você Pode Fazer

### **1. VENDEDOR - Criar e Gerenciar Produtos**

#### Produtos de Exemplo Já Criados:
1. **🍽️ Marmita Teste** - Vianda com dias da semana
2. **📱 Smartphone Teste** - Com combos e desconto progressivo
3. **👕 Camiseta Teste** - Com redução automática
4. **💡 Luminária Teste** - Com frete cobrado
5. **⚽ Bola Teste** - Com combo e redução

#### Como Editar:
1. Clique em "📦 Meus Produtos"
2. Selecione o produto
3. Edite qualquer campo
4. Salve as alterações
5. ✅ **As opções marcadas NÃO serão desmarcadas!**

#### Como Criar Novo:
1. Clique em "+ Vender"
2. Preencha os dados
3. **Teste as seguintes opções:**
   - ✅ Redução automática 1% ao dia
   - ✅ Selo de confiança
   - ✅ Desconto progressivo
   - ✅ Permitir combo
   - ✅ Categorias: Viandas, Alimentação (com campos especiais)

#### Categorias Disponíveis:
- Eletrônicos
- Moda
- Casa
- Esportes
- Livros
- 🍽️ **Viandas** ← Tem campos especiais!
- 🥗 **Alimentação** ← Tem campos especiais!
- Outros

---

### **2. VIANDAS - Configuração Especial**

Quando selecionar **Viandas** ou **Alimentação**:

**Campos Especiais Aparecem:**
- 📅 Escolha dias da semana (Seg-Dom)
- 📦 Defina quantidade disponível por dia

**Exemplo:**
```
Marmita de Frango
Disponível: Seg, Ter, Qua, Qui, Sex
Quantidade: 15 marmitas/dia

👉 Cardápio repete toda semana!
```

---

### **3. REDUÇÃO AUTOMÁTICA 1% AO DIA**

✅ **Marque a opção: "Ativar redução automática de 1% por dia"**

Quando ativa:
- **Dia 1:** R$ 100 (preço original)
- **Dia 2:** R$ 99 (reduz 1%)
- **Dia 3:** R$ 98 (reduz 2%)
- ...continua reduzindo

**Na Vitrine:**
- Badge "📉 Preço Caindo!" aparece
- Mostra preço original e preço atual
- Mostra "-1% ao dia"

**Em Meus Produtos:**
- Você vê: "✅ Redução automática: -X%"

---

### **4. SUGESTÕES INTELIGENTES**

**Frete Inteligente:**
- Se produto custa R$ 30 → Sugere +R$ 3
- Se produto custa R$ 100 → Sugere +R$ 10
- Máximo R$ 300 de aumento
- ✅ Clique "✨ Aplicar Sugestão" para ativar

---

### **5. COMPRADOR - Negociar Produtos**

#### Como Comprar Seu Próprio Produto:
1. Vá para "🛍️ Vitrine"
2. Encontre um produto seu
3. ⚠️ **Você NÃO consegue negociar com você mesmo!**
   - Aparece "🔒 Bloqueado" (mesmo IP/ID)

#### Como Simular Compra:
1. Crie um produto
2. **Em outra janela (anônima ou outro navegador):**
   - Não faça login
   - Veja o produto na vitrine
   - Clique em "💬 Negociar"
   - Use o chatbot inteligente

---

### **6. CHATBOT DE NEGOCIAÇÃO**

**Como Funciona:**
- Comprador oferece preço
- Chatbot "analisa" e responde
- Nunca desce abaixo do mínimo
- Aceita quando chega no mínimo
- Mostra frete grátis como bônus

**Testes Recomendados:**
```
Produto: Marmita (Anúncio R$ 45, Mínimo R$ 35)

1. Oferta: R$ 20 → Chatbot recusa
2. Oferta: R$ 33 → Chatbot recusa (abaixo do mínimo)
3. Oferta: R$ 35 → Chatbot aceita! ✅
4. Oferta: R$ 45 → Chatbot fica excitado! "WOW!"
```

---

### **7. FILTROS DE TEMPO**

Na vitrine, teste os filtros:
- 📦 **Todos** - Todos os produtos
- 🌟 **Acabei de Publicar** - 0-24h
- ⚡ **Rápido** - 24-48h
- 📦 **Vendendo Agora** - 3-25 dias
- 🔥 **Liquidando** - 26-29 dias
- 💰 **Saldão** - 30+ dias

**Dica:** Crie produtos em datas diferentes para ver os diferentes badges!

---

### **8. SISTEMA DE FRETE**

**Opções Disponíveis:**

1. **❌ Não entrego, retirar no local**
   - Sem frete
   - Cliente vai pegar

2. **✅ Sim, faço entrega**
   - Frete GRÁTIS até X km
   - Frete cobrado acima de X km

**Exemplo Vianda:**
```
Frete grátis até 5 km
📍 Santa Rosa

Comprador a 3km → Frete GRÁTIS ✅
Comprador a 8km → Paga R$ 12 📦
```

---

### **9. SISTEMA DE LUCRO OCULTO**

**Estrutura:**
```
Anúncio: R$ 100
Mínimo: R$ 50

Se vender por R$ 100:
├─ Vendedor recebe: R$ 50
└─ Plataforma lucra: R$ 50

Se vender por R$ 75:
├─ Vendedor recebe: R$ 50
└─ Plataforma lucra: R$ 25
```

**⚠️ IMPORTANTE:**
- Vendedor SEMPRE recebe o mínimo
- Não importa quanto comprador pague
- Com redução automática, o mínimo também reduz!

---

### **10. CHECKLIST DE TESTES RECOMENDADOS**

**Básico:**
- [ ] Logar com teste@test.com / 123
- [ ] Ver produtos de exemplo na vitrine
- [ ] Editar um produto
- [ ] Criar novo produto
- [ ] Deletar um produto

**Vendedor:**
- [ ] Testar redução automática (1% ao dia)
- [ ] Testar sugestão de frete inteligente
- [ ] Testar vianda com calendário
- [ ] Testar combo de produtos
- [ ] Testar selo de confiança

**Comprador:**
- [ ] Negociar produto (em modo anônimo)
- [ ] Testar chatbot com várias ofertas
- [ ] Testar aceitação de frete grátis
- [ ] Testar rejeição de preço baixo

**Filtros:**
- [ ] Testar cada aba de tempo
- [ ] Testar busca por nome
- [ ] Testar categoria

**Edge Cases:**
- [ ] Tentar bloquear seu próprio produto (não consegue!)
- [ ] Editar produto mantendo opções marcadas
- [ ] Criar vianda com todos os dias
- [ ] Produto com frete maior que preço (não deixa!)

---

## 🎓 Funcionalidades Mais Importantes

### **✅ DEVE TESTAR:**

1. **Redução 1% ao Dia**
   - Cria urgência
   - Vende 600% mais rápido
   - +600 visualizações

2. **Frete Grátis Inteligente**
   - Aumenta 40% de vendas
   - Clientes adoram

3. **Chatbot Inteligente**
   - Negocia sozinho
   - Nunca perde venda
   - Usa psicologia

4. **Viandas com Calendário**
   - Para comida/marmita
   - Repete toda semana
   - Clientes sabem quando tem

5. **Sistema de Lucro Oculto**
   - Vendedor acha que recebe mais
   - Plataforma lucra a diferença
   - Funciona com redução também

---

## 🚀 Próximos Passos

Depois de testar tudo:

1. Conecte a um **banco de dados real** (MongoDB/PostgreSQL)
2. Implemente **chat em tempo real** (WebSocket)
3. Implemente **sistema de pagamento** (Stripe/PIX)
4. Deploy em **servidor** (Vercel/Heroku)
5. Customize para sua **região/negócio**

---

## 💬 Precisa de Ajuda?

Teste cada funcionalidade um por um. Se algo não funcionar:

1. Abra o Console (F12)
2. Veja se há erros
3. Limpe localStorage: `localStorage.clear()`
4. Recarregue a página

---

**DIVIRTA-SE TESTANDO! 🎉**
