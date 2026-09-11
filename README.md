# Desafio Prático: Implementação de Práticas DevOps — A Tech 

Oiê! este repositório foi criado para documentar a resolução do desafio prático proposto pela Rocketseat. O objetivo é aplicar os conceitos de **CALMS** e das **Três Maneiras** do DevOps para transformar o fluxo operacional de uma empresa fictícia, eliminando gargalos entre as equipes de desenvolvimento e operações.

Abaixo, o planejamento estruturado que desenvolvi para o cenário da "A Tech":

# Plano de Implementação de Práticas DevOps — A Tech

## 1. Contexto e Objetivo

A Tech mantém dois produtos com perfis diferentes: o **Sistema de Gestão de Vendas**, legado em Delphi, e a **Plataforma de E-commerce**, mais moderna e escalável. O ciclo de entrega atual é realizado de forma manual — desde o deploy até o monitoramento — o que gera atritos entre as equipes de Desenvolvimento (14 pessoas) e Operações (4 pessoas).

Este plano utiliza o framework **CALMS** e as **Três Maneiras do DevOps** para diagnosticar o processo de entrega e propor melhorias, priorizando a Plataforma de E-commerce como piloto por ser um ambiente mais adequado para iniciar a automação, enquanto o sistema legado possui maior dependência de conhecimento especializado.

---

## 2. Diagnóstico Cultural (C — Culture)

### 2.1 Processo identificado

O processo escolhido é o **ciclo de entrega e deploy da Plataforma de E-commerce**: desde a conclusão do desenvolvimento até o sistema estar validado e monitorado em produção.

### 2.2 Como funciona hoje

1. O time de Desenvolvimento finaliza um recurso e monta um pacote de implantação.
2. O pacote é repassado para a equipe de Operações.
3. Operações realiza o deploy manualmente em produção, sem um procedimento padronizado.
4. Somente depois do deploy em produção, Operações realiza testes manuais para verificar se tudo está funcionando.
5. Operações acompanha os logs do servidor manualmente para identificar possíveis falhas.

### 2.3 Pontos de atrito

* **Entrega "por cima do muro"**: Desenvolvimento prepara e entrega o pacote, enquanto Operações fica responsável pela execução do deploy, dificultando uma responsabilidade compartilhada pelo resultado.

* **Deploy sem padrão**: como não existe um procedimento documentado e automatizado, o resultado pode variar conforme a execução, aumentando o risco de falhas.

* **Teste depois do ar, não antes**: validar o sistema somente após o deploy em produção faz com que problemas sejam identificados mais tarde, podendo gerar pressão e conflitos entre as equipes.

* **Operações sobrecarregada**: apenas 4 profissionais são responsáveis pela infraestrutura e pelos dois sistemas, enquanto também enfrentam desafios relacionados à escalabilidade e ao desempenho.

* **Baixa visibilidade compartilhada**: sem dashboards ou alertas automáticos, os incidentes dependem de investigação manual dos logs, dificultando a identificação rápida dos problemas.

### 2.4 Oportunidades de melhoria

* Pipeline de entrega automatizado, com testes realizados **antes** do deploy.
* Ambiente de homologação/staging para validar as alterações antes da produção.
* Monitoramento e observabilidade com alertas automáticos.
* Documentação e responsabilidade compartilhadas entre Desenvolvimento e Operações.

---

## 3. Automação (A — Automation)

### 3.1 Solução proposta

| Etapa atual                        | Proposta de automação                                                                                              |
| ---------------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Empacotamento manual               | Build automatizado a partir do controle de versão (Git) a cada push ou pull request aprovado                       |
| Deploy manual sem padrão           | Pipeline de CI/CD (ex.: GitHub Actions ou Azure DevOps) com etapas fixas e repetíveis                              |
| Testes após o deploy               | Testes automatizados, como testes unitários, de integração e smoke tests, executados no pipeline antes da produção |
| Monitoramento manual de logs       | Observabilidade automatizada (ex.: Grafana + Prometheus ou Azure Monitor) com alertas configurados                 |
| Provisionamento manual de ambiente | Infraestrutura como Código (ex.: Terraform) para criar ambientes de forma mais consistente                         |

Para reduzir o risco dos deploys, também pode ser utilizada uma estratégia de liberação gradual, como **blue-green ou canary**, com possibilidade de rollback caso sejam identificados problemas após a publicação.

O **Sistema de Gestão de Vendas (Delphi)** ficará fora do escopo inicial da automação completa. Como apenas uma pessoa possui conhecimento em Delphi, alterações no processo de build e deploy poderiam aumentar a dependência desse profissional. Inicialmente, a proposta é automatizar o versionamento e o empacotamento dos artefatos do sistema legado, enquanto a equipe trabalha na disseminação desse conhecimento.

### 3.2 Plano de implementação por fases

**Fase 1 (0–30 dias) — Piloto na Plataforma de E-commerce**

Padronizar o repositório, criar o pipeline básico de build e testes automatizados e preparar o ambiente de staging.

**Fase 2 (30–60 dias) — Deploy assistido**

Automatizar o deploy para produção, inicialmente mantendo a aprovação manual de um responsável. Criar dashboards de monitoramento e configurar alertas básicos.

**Fase 3 (60–90 dias) — Deploy contínuo**

Ampliar a automação, incluindo testes de smoke após a publicação, rollback automático e Infraestrutura como Código para os ambientes de staging e produção. Também pode ser iniciado o piloto de automação do empacotamento do sistema legado.

**Contínuo**

Expandir a observabilidade, treinar os integrantes das equipes e revisar as métricas do processo a cada ciclo.

### 3.3 Minimizando a resistência

* Envolver a equipe de Operações desde o início do desenho da solução.
* Escolher uma pessoa de referência em cada equipe para auxiliar na adoção.
* Realizar workshops práticos utilizando o pipeline real.
* Começar pela Plataforma de E-commerce e deixar a automação completa do legado para uma etapa posterior.
* Reservar parte do tempo da equipe para a implementação e aprendizado das novas ferramentas.
* Compartilhar os ganhos obtidos para aumentar a confiança na mudança.

---

## 4. Lean (L — Lean)

O processo atual possui alguns desperdícios e gargalos, como o tempo de espera entre Desenvolvimento e Operações, tarefas manuais repetitivas e retrabalho causado por problemas encontrados somente depois do deploy.

A aplicação do Lean busca reduzir esses desperdícios por meio de **entregas menores, automação, testes antecipados e redução de etapas desnecessárias entre as equipes**.

---

## 5. Mensuração e Compartilhamento de Conhecimento (M e S)

### 5.1 Métricas de acompanhamento

As métricas abaixo permitem comparar a situação atual com os resultados após a implementação das melhorias:

| Métrica                            | Hoje                     | Meta                           |
| ---------------------------------- | ------------------------ | ------------------------------ |
| Tempo entre código pronto e deploy | 2 dias                   | Menos de 4 horas               |
| Taxa de sucesso do deploy          | 80%                      | 95% ou mais                    |
| Incidentes pós-deploy              | ~2 por semana            | Redução de pelo menos 50%      |
| MTTR (tempo médio de recuperação)  | 4 horas                  | Menos de 1 hora                |
| Cobertura de testes automatizados  | Praticamente inexistente | 70% ou mais nas áreas críticas |

### 5.2 Compartilhamento de conhecimento

* Manter uma documentação atualizada, incluindo procedimentos, runbooks e decisões técnicas, em um espaço compartilhado.
* Disponibilizar os dashboards de monitoramento para Desenvolvimento e Operações.
* Realizar encontros periódicos de demonstração e aprendizado entre as equipes.
* Realizar **postmortems sem culpa (blameless)** após incidentes relevantes, registrando os aprendizados.
* Criar um canal de comunicação para notificações do pipeline e discussões técnicas.

---

## 6. As Três Maneiras

### Primeira Maneira — Acelerar o Fluxo

* Fazer deploys menores e mais frequentes, evitando o acúmulo de grandes mudanças.
* Padronizar e automatizar o processo de deploy.
* Utilizar feature flags para separar o deploy do lançamento de uma funcionalidade para os usuários.
* Reduzir os handoffs desnecessários entre Desenvolvimento e Operações.

### Segunda Maneira — Ampliar o Feedback

* Realizar os testes mais cedo no ciclo de desenvolvimento, antes do deploy em produção.
* Fazer com que os alertas de monitoramento cheguem automaticamente ao time responsável pelo sistema.
* Compartilhar os aprendizados dos incidentes com as equipes envolvidas.
* Levar feedback de suporte e clientes para o backlog de melhorias.

### Terceira Maneira — Experimentar e Aprender

* Utilizar postmortems como fonte de aprendizado, sem procurar culpados.
* Reservar tempo para testar novas ferramentas em ambientes seguros, sem afetar a produção.
* Criar oportunidades de trabalho conjunto entre Desenvolvimento e Operações para compartilhar conhecimento.
* Valorizar os aprendizados obtidos com falhas, e não somente os resultados positivos.

---

## 7. Conclusão

O principal gargalo da A Tech está em um processo manual, sem padronização e com pouco feedback antecipado, fazendo com que parte dos problemas seja descoberta somente após o deploy. A automação do pipeline da Plataforma de E-commerce, acompanhada de métricas, compartilhamento de conhecimento e melhoria contínua, pode reduzir esses gargalos e melhorar a colaboração entre Desenvolvimento e Operações. Depois da validação desse processo, as práticas podem ser ampliadas gradualmente para o sistema legado.
