# 🧪 Guia de Testes - Sistema Coleta Seletiva Conectada

## 📋 Pré-requisitos
- Sistema rodando: `streamlit run app.py`
- Senhas atualizadas: **1234** para todos os usuários

---

## ✅ TESTE 1: Login com Nova Senha

### Objetivo
Verificar se todas as senhas foram atualizadas corretamente.

### Passos:
1. Acesse o sistema
2. Teste login com os seguintes usuários:

**Morador:**
- Email: `morador@teste.com`
- Senha: `1234`
- ✅ Deve funcionar

**Catador:**
- Email: `catador@teste.com`
- Senha: `1234`
- ✅ Deve funcionar

**Administrador:**
- Email: `admin@prefeitura.itajuba.gov.br`
- Senha: `1234`
- ✅ Deve funcionar

---

## ✅ TESTE 2: Solicitação "Disponível para Todos"

### Objetivo
Verificar se moradores podem solicitar coletas para todos os catadores.

### Passos:
1. Login como morador: `morador@teste.com` / `1234`
2. Menu lateral → **"Solicitar Coleta"**
3. Verificar se aparece no topo da lista:
   ```
   🌍 Disponível para Todos os Catadores
   • Todos os bairros
   ```
4. Expandir este card
5. Preencher o formulário:
   - **Data:** Escolha uma data futura
   - **Endereço:** Digite um endereço qualquer
   - **Horário:** Manhã (8h às 12h)
   - **Tipos de Material:** Selecione Papel e Plástico
   - **Quantidade:** 5 kg
   - **Observações:** "Teste de coleta para todos"
6. Clicar em **"📞 Solicitar Coleta"**
7. Verificar mensagens:
   - ✅ "🎉 Solicitação enviada com sucesso!"
   - ✅ "📢 Sua solicitação foi disponibilizada para todos os catadores do seu bairro."
   - ✅ "🔔 Assim que um catador aceitar, você receberá uma notificação."
8. Fazer logout

### Resultado Esperado:
- Coleta criada com `catador_id = None`
- Notificações enviadas para **todos os catadores** que atendem o bairro Centro

---

## ✅ TESTE 3: Catador Visualiza Coleta Disponível

### Objetivo
Verificar se catadores veem coletas disponíveis para todos.

### Passos:
1. Login como catador: `catador@teste.com` / `1234`
2. Menu lateral → **"Solicitações Disponíveis"**
3. Verificar se a coleta criada no Teste 2 aparece
4. Verificar informações:
   - Nome do morador
   - Data solicitada
   - Materiais
   - Quantidade estimada
5. NÃO aceitar ainda (apenas visualizar)
6. Fazer logout

### Resultado Esperado:
- ✅ Coleta aparece na lista de "Solicitações Disponíveis"

---

## ✅ TESTE 4: Catador Aceita Coleta

### Objetivo
Verificar se catador pode aceitar coleta e morador recebe notificação.

### Passos:
1. Login como catador: `catador@teste.com` / `1234`
2. Menu lateral → **"Solicitações Disponíveis"**
3. Encontrar a coleta do Teste 2
4. Clicar em **"Aceitar solicitação #X"**
5. Verificar mensagem: "Solicitação aceita com sucesso!"
6. Ir para **"Minhas Coletas"**
7. Verificar se a coleta aparece em "Aceitas"
8. Fazer logout

### Resultado Esperado:
- ✅ Coleta aceita
- ✅ Status mudou para "agendada"
- ✅ `catador_id` foi atribuído
- ✅ Notificação enviada ao morador

---

## ✅ TESTE 5: Morador Recebe Notificação de Aceite

### Objetivo
Verificar se morador recebe notificação quando catador aceita.

### Passos:
1. Login como morador: `morador@teste.com` / `1234`
2. Na página **"Início"**, rolar até **"Notificações"**
3. Verificar se há notificação nova:
   ```
   ✅ Solicitação de coleta aceita!
   O catador Benedito Luiz aceitou sua solicitação de coleta 
   para o dia DD/MM/YYYY às Manhã (8h às 12h).
   ```
4. Ir para **"Minhas Coletas"**
5. Verificar se a coleta aparece com status "agendada"

### Resultado Esperado:
- ✅ Notificação recebida
- ✅ Coleta com status "agendada"
- ✅ Nome do catador visível

---

## ✅ TESTE 6: Solicitação para Catador Específico

### Objetivo
Verificar se morador pode solicitar coleta para catador específico.

### Passos:
1. Login como morador: `morador@teste.com` / `1234`
2. Menu lateral → **"Solicitar Coleta"**
3. **NÃO** escolher "Disponível para Todos"
4. Rolar para baixo e escolher um catador específico (ex: "Ana Santos")
5. Expandir o card de "Ana Santos"
6. Preencher o formulário:
   - **Data:** Escolha uma data futura
   - **Endereço:** Digite um endereço qualquer
   - **Horário:** Tarde (12h às 18h)
   - **Tipos de Material:** Selecione Metal e Vidro
   - **Quantidade:** 3 kg
   - **Observações:** "Teste para catador específico"
7. Clicar em **"📞 Solicitar Coleta"**
8. Verificar mensagens:
   - ✅ "🎉 Solicitação enviada com sucesso!"
   - ✅ "📬 Sua solicitação foi enviada para Ana Santos."
   - ✅ "🔔 Você será notificado quando o catador responder."
9. Fazer logout

### Resultado Esperado:
- Coleta criada com `catador_id = 7` (ID de Ana Santos)
- Notificação enviada **APENAS** para Ana Santos

---

## ✅ TESTE 7: Catador Específico Vê Solicitação

### Objetivo
Verificar se apenas o catador escolhido vê a solicitação.

### Passos:
1. Login como catador: `ana@teste.com` / `1234`
2. Verificar **"Notificações"** no início
3. Verificar se há notificação:
   ```
   Nova solicitação de coleta
   Maria das Graças solicitou uma coleta para o dia...
   ```
4. Menu lateral → **"Solicitações Disponíveis"**
5. Verificar se a coleta aparece
6. Fazer logout
7. Login como outro catador: `catador@teste.com` / `1234`
8. Verificar **"Solicitações Disponíveis"**
9. Verificar que esta coleta **NÃO aparece** (pois foi para Ana)

### Resultado Esperado:
- ✅ Ana Santos vê a coleta
- ✅ Outros catadores **NÃO** veem esta coleta específica
- ✅ Sistema diferencia corretamente

---

## ✅ TESTE 8: Catador Recusa Coleta

### Objetivo
Verificar se morador recebe notificação quando catador recusa.

### Passos:
1. Login como catador: `ana@teste.com` / `1234`
2. Menu lateral → **"Solicitações Disponíveis"**
3. Encontrar a coleta do Teste 6
4. Clicar em **"Recusar solicitação #X"**
5. Verificar mensagem: "Solicitação recusada."
6. Fazer logout
7. Login como morador: `morador@teste.com` / `1234`
8. Verificar notificações
9. Verificar se há notificação:
   ```
   ❌ Solicitação de coleta recusada
   O catador Ana Santos não pôde aceitar sua solicitação...
   ```

### Resultado Esperado:
- ✅ Coleta recusada
- ✅ Status mudou para "recusada"
- ✅ Morador recebeu notificação

---

## ✅ TESTE 9: Múltiplos Catadores Veem Coleta Geral

### Objetivo
Verificar se todos os catadores do bairro veem coletas "Disponível para Todos".

### Passos:
1. Criar nova coleta "Disponível para Todos" (repetir Teste 2)
2. Login como diferentes catadores:
   - `catador@teste.com` / `1234` (Benedito Luiz - atende Centro)
   - `carlos@teste.com` / `1234` (Carlos Ferreira - atende Centro)
3. Para cada catador:
   - Ir em "Solicitações Disponíveis"
   - Verificar se a coleta aparece
4. Primeiro catador que aceitar, **pega a coleta**
5. Outros catadores não devem mais vê-la

### Resultado Esperado:
- ✅ Todos catadores do bairro veem a coleta
- ✅ Primeiro que aceita, pega
- ✅ Outros não veem mais após aceite

---

## 🎯 Checklist Final

| Teste | Descrição | Status |
|-------|-----------|--------|
| 1 | Login com senha 1234 | ⬜ |
| 2 | Solicitação "Disponível para Todos" | ⬜ |
| 3 | Catador visualiza coleta disponível | ⬜ |
| 4 | Catador aceita coleta | ⬜ |
| 5 | Morador recebe notificação de aceite | ⬜ |
| 6 | Solicitação para catador específico | ⬜ |
| 7 | Apenas catador escolhido vê solicitação | ⬜ |
| 8 | Catador recusa e morador recebe notificação | ⬜ |
| 9 | Múltiplos catadores veem coleta geral | ⬜ |

---

## 🐛 Problemas Comuns

### Problema: Coleta não aparece para catador
**Causa:** Catador não atende o bairro da coleta  
**Solução:** Verificar `areas_atuacao` do catador no arquivo `usuarios.csv`

### Problema: Notificação não aparece
**Causa:** Notificação pode estar marcada como lida  
**Solução:** Verificar arquivo `notificacoes.csv` - coluna `lida`

### Problema: Erro ao aceitar coleta
**Causa:** Coleta pode já ter sido aceita por outro catador  
**Solução:** Atualizar a página (F5)

---

## 📊 Logs para Depuração

Se algo não funcionar, verifique:

1. **Terminal onde Streamlit está rodando** - Erros aparecem ali
2. **Arquivo `coletas.csv`** - Verificar `status` e `catador_id`
3. **Arquivo `notificacoes.csv`** - Verificar se notificações foram criadas

---

**Boa sorte nos testes! 🚀**
