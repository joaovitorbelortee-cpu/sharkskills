---
name: zap-defensive-workflow
description: "Workflow de Auditoria Defensiva Profissional usando OWASP ZAP e Eclipse para testes locais de resiliência web."
category: security
risk: safe
source: personal
---

# Workflow: Auditoria de Segurança Defensiva com OWASP ZAP

Este workflow foi desenhado para testar a resiliência das suas aplicações web (como o BotZap) identificando vulnerabilidades críticas antes que elas possam ser exploradas. O foco é a **detecção automatizada** e a **correção a nível de código**.

## Fase 1: Mapeamento e Varredura (OWASP ZAP)

O OWASP ZAP atuará como nosso scanner automatizado para encontrar as brechas.

### 1. Configuração do Contexto (Spidering)
Antes de atacar, o ZAP precisa entender o tamanho do seu projeto.
- **Ação:** Execute o *Spider* (e o *Ajax Spider* para SPAs/React) apontando para o seu `localhost`.
- **Objetivo:** Mapear todas as rotas, endpoints de API (como os webhooks do QStash) e páginas ocultas.

### 2. Configuração de Autenticação
Muitas falhas estão escondidas atrás da tela de login.
- **Ação:** Configure um "Contexto" no ZAP com credenciais de teste válidas. Forneça o token de sessão ou os scripts de login para que o ZAP consiga navegar como um usuário logado.

### 3. Active Scan (Varredura Ativa)
Aqui é onde o ZAP lança milhares de payloads contra a sua aplicação para identificar falhas.
- **O que ele testa:**
  - **Injeções (SQLi, NoSQLi):** Tenta manipular inputs para ver se o banco de dados retorna erros ou dados não autorizados.
  - **Cross-Site Scripting (XSS):** Injeta scripts nocivos em formulários para ver se a interface os reflete sem sanitização.
  - **Broken Access Control:** Tenta acessar rotas administrativas usando o token de um usuário comum (IDOR).
  - **Security Misconfigurations:** Verifica cabeçalhos de segurança ausentes e configurações de servidor expostas.

## Fase 2: Análise de Resultados e Triagem

Assim que o ZAP terminar de bombardear a aplicação local, ele gerará uma árvore de alertas.

1. **Priorização:** Filtre os alertas por **High** (Alto) e **Medium** (Médio).
2. **Reprodução:** Para cada alerta crítico (ex: um SQL Injection em uma query do painel), observe a requisição HTTP exata que o ZAP enviou e a resposta que o servidor devolveu.
3. **Validação:** Verifique se não é um Falso Positivo. Se a aplicação travou ou retornou uma stack trace do banco, a vulnerabilidade é real.

## Fase 3: Correção no Código (Eclipse / VS Code)

Com o relatório de vulnerabilidades em mãos, o trabalho passa para a sua IDE.

### Exemplo 1: Corrigindo SQL Injection
- **Problema encontrado pelo ZAP:** Concatenação de strings em queries do banco de dados.
- **Ação no Eclipse:** Localizar o controller vulnerável.
- **Correção:** Substituir queries brutas por **Prepared Statements** ou usar os métodos seguros do ORM (como o ActiveRecord no Rails ou Prisma no Node), garantindo que inputs do usuário sejam sempre sanitizados.

### Exemplo 2: Corrigindo XSS (Cross-Site Scripting)
- **Problema encontrado pelo ZAP:** O nome do Lead salvo pelo webhook está sendo renderizado no dashboard com scripts maliciosos.
- **Ação no Eclipse:** Localizar a view correspondente.
- **Correção:** Garantir que o framework de frontend esteja fazendo o *escape* automático das variáveis HTML, ou aplicar funções de sanitização estritas antes de salvar o dado no banco.

### Exemplo 3: Proteção de Webhooks
- **Problema encontrado pelo ZAP:** Qualquer pessoa pode fazer um POST para a rota do webhook e injetar dados no sistema.
- **Ação no Eclipse:** Localizar o `campaigns_controller.rb`.
- **Correção:** Implementar validação de assinatura (ex: `Upstash-Signature`) exigindo que um hash criptográfico válido acompanhe cada requisição.

## Fase 4: Re-teste (Validação Contínua)

Após corrigir o código no Eclipse e salvar as alterações:
1. Volte ao OWASP ZAP.
2. Selecione os alertas antigos e execute um **Retest**.
3. Se as proteções (como sanitização e checagem de assinatura) foram bem implementadas, o ZAP reportará que a vulnerabilidade foi mitigada.

> **Importante:** Este processo deve ser rodado **localmente** ou em um **ambiente de staging/homologação**, nunca em servidores de produção com dados reais de clientes, para evitar interrupções no serviço e corrupção de dados.
