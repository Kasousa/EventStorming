# 📙 Trabalho de Domain-Driven Design (DDD)

## Projeto: Plataforma de Detecção Precoce e Monitoramento de Risco para Doença Renal Crônica (DRC)
Autores: Andressa Lemes, Kaique Santos, Larissa Silva Ramos, Leonardo Almeida do Carmo

---

# 📈 Parte 3 — Event Storming do Domínio de DRC

## **1️⃣ Preparação (5-10 min)**
### Contexto do Processo
A dinâmica foca em compreender o ciclo completo da **detecção precoce e acompanhamento de risco da DRC**, desde a coleta de dados clínicos até a geração de alertas e engajamento do paciente.

Fluxo macro:
1. Paciente realiza exames laboratoriais ou consultas de rotina.
2. Sistema coleta e integra dados (laboratórios, e-SUS, DATASUS).
3. Algoritmo avalia risco (ScoreRisco) e identifica casos suspeitos.
4. Gera alertas e prioriza acompanhamento.
5. Notifica profissionais e pacientes.
6. Atualiza indicadores epidemiológicos e relatórios.

---

## **2️⃣ Mapeamento de Eventos de Domínio (15-20 min)**
### Pergunta-chave: *O que acontece no processo? (sempre no passado)*
- **Paciente Cadastrado** (registro no sistema com consentimento LGPD)
- **Exame Solicitado** (profissional solicita creatinina e urina)
- **Exame Realizado** (laboratório disponibiliza resultado)
- **Score de Risco Calculado** (modelo preditivo executado)
- **Paciente Classificado como Alto Risco** (estratificação concluída)
- **Alerta Clínico Gerado** (score acima do limiar de segurança)
- **Notificação Enviada ao Profissional de Saúde** (com recomendação)
- **Plano de Cuidado Criado** (médico define conduta personalizada)
- **Paciente Notificado** (mensagem de orientação enviada)
- **Indicadores Epidemiológicos Atualizados** (dados integrados ao BI)

---

## **3️⃣ Identificação de Comandos e Atores (10-15 min)**
### Pergunta-chave: *O que causou esse evento?*

| **Comando** | **Ator Responsável** | **Evento Gerado** |
|--------------|------------------------|------------------|
| Cadastrar Paciente | Agente de Saúde / Sistema de Cadastro | Paciente Cadastrado |
| Solicitar Exame | Médico / Sistema de Rastreamento | Exame Solicitado |
| Registrar Resultado | Laboratório / Sistema FHIR | Exame Realizado |
| Calcular Score de Risco | Motor de IA / Sistema Preditivo | Score de Risco Calculado |
| Classificar Risco | Sistema de Triagem | Paciente Classificado como Alto Risco |
| Gerar Alerta | Sistema de Casos Clínicos | Alerta Clínico Gerado |
| Enviar Notificação | Sistema de Engajamento | Notificação Enviada ao Profissional de Saúde |
| Criar Plano de Cuidado | Médico da Atenção Básica | Plano de Cuidado Criado |
| Enviar Lembrete | Sistema de Engajamento | Paciente Notificado |
| Atualizar BI | Sistema de Relatórios | Indicadores Epidemiológicos Atualizados |

---

## **4️⃣ Regras e Políticas de Negócio (10-15 min)**
### Pergunta-chave: *Quais regras precisam ser seguidas nesse processo?*

- **Regra 1:** O paciente deve consentir explicitamente o uso dos dados (LGPD) antes de qualquer processamento.
- **Regra 2:** O score de risco é calculado somente se houver exames de creatinina e urina recentes.
- **Regra 3:** Um paciente só é classificado como alto risco se o score > 0.7 ou se TFG < 60.
- **Regra 4:** Alertas clínicos devem ser revisados pelo profissional de saúde antes de notificar o paciente.
- **Regra 5:** O plano de cuidado deve conter pelo menos uma ação de seguimento (nova coleta ou consulta).
- **Regra 6:** Indicadores só são atualizados após confirmação de consistência dos dados (validação de integridade).

---

## **5️⃣ Identificação dos Bounded Contexts (10 min)**
### Pergunta-chave: *Quem é responsável por cada parte do processo?*

| **Bounded Context** | **Comandos Principais** | **Eventos** | **Responsável / Sistema** |
|----------------------|--------------------------|-------------|------------------------------|
| **Cadastro & LGPD** | Cadastrar Paciente | Paciente Cadastrado | Sistema de Identidade / Consentimento |
| **Rastreamento & Exames** | Solicitar Exame, Registrar Resultado | Exame Solicitado, Exame Realizado | Orquestrador de Rastreamento |
| **Risco & Triagem** | Calcular Score, Classificar Risco | Score de Risco Calculado, Paciente Classificado | Motor de IA e Regras Clínicas |
| **Casos Clínicos** | Gerar Alerta, Criar Plano | Alerta Clínico Gerado, Plano de Cuidado Criado | Sistema de Casos e Alertas |
| **Engajamento** | Enviar Notificação, Enviar Lembrete | Notificação Enviada, Paciente Notificado | Sistema de Engajamento Multicanal |
| **Relatórios & KPIs** | Atualizar BI | Indicadores Atualizados | Data Lake / BigQuery + Looker |

---

## **6️⃣ Discussão e Refinamento (15 min)**
Durante a sessão de refinamento, o grupo analisou os pontos de integração e priorizou os eventos de maior impacto para o fluxo de valor.

**Principais insights:**
- A detecção precoce depende da integração automatizada entre laboratórios (FHIR) e o motor de risco.
- Os alertas clínicos funcionam como o elo entre o sistema preditivo e o acompanhamento humano.
- O Event Storming revelou a necessidade de separar claramente os contextos de *Engajamento* e *Casos Clínicos* para evitar acoplamento.
- O uso de mensageria assíncrona (Kafka/EventBridge) foi sugerido para coordenar eventos entre Risco → Rastreamento → Casos.
- KPIs agregados no Looker são a principal fonte para feedback de políticas de prevenção.

---

**Conclusão:**
O Event Storming consolidou a compreensão coletiva do domínio e alinhou as fronteiras entre os contextos principais da plataforma (Risco, Rastreamento, Casos, Engajamento e Relatórios), fortalecendo o design baseado em eventos e a comunicação ubíqua entre equipes técnicas e clínicas.

