## 🧾 Descrição
Descreva claramente o problema e o que foi feito.  
Exemplo: Corrigida a validação de autenticação no endpoint /login.

## ✅ Tipo de mudança
Selecione uma opção:
- [ ] Bug fix (correção de erro)
- [ ] Nova feature
- [ ] Refatoração
- [ ] Ajustes de documentação ou CI/CD

## 🎯 Objetivo
Explique o objetivo desta alteração.
Ex: Garantir que o usuário receba uma mensagem adequada ao falhar login.

## 🧪 Como testar?
Passo a passo para validar:
1. `docker build . -t auth-service:local`
2. `docker run -p 5000:5000 auth-service:local`
3. POST no endpoint /login com credenciais inválidas
4. Verificar se o retorno HTTP é `401` com mensagem adequada

## 📎 Evidências (se aplicável)
Adicione prints, logs, vídeos ou dumps de resposta para facilitar revisão.

## 🔍 Checklist
- [ ] Testes automatizados foram atualizados/criados
- [ ] Pipeline CI passou com sucesso
- [ ] Não contém dados sensíveis no diff
