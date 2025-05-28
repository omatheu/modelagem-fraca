# Diagramas de Atividades
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação dos Diagramas de Atividades](#notação-dos-diagramas-de-atividades)
- [Diagramas de Atividades Principais](#diagramas-de-atividades-principais)
  - [Autenticação no Sistema](#autenticação-no-sistema)
  - [Registro e Atendimento de Solicitações](#registro-e-atendimento-de-solicitações)
  - [Gestão de Tarefas](#gestão-de-tarefas)
  - [Geração de Relatórios](#geração-de-relatórios)
  - [Gestão do Repositório de Materiais](#gestão-do-repositório-de-materiais)
- [Relação com Requisitos e Casos de Uso](#relação-com-requisitos-e-casos-de-uso)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Este documento apresenta os diagramas de atividades para os principais processos do Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza. Os diagramas de atividades são representações visuais do fluxo de trabalho que mostram a sequência de atividades de um processo do início ao fim, incluindo caminhos condicionais e iterações.

Ao contrário dos diagramas BPMN, que focam nos processos de negócio e nas interações entre diferentes participantes, os diagramas de atividades concentram-se no fluxo lógico e na sequência de operações dentro de um sistema. Eles são particularmente úteis para visualizar o comportamento dinâmico de um sistema, especialmente os fluxos de trabalho associados a casos de uso específicos.

## Notação dos Diagramas de Atividades

Os diagramas de atividades neste documento usam a notação UML (Unified Modeling Language) padrão, incluindo os seguintes elementos:

- **Nó Inicial**: Representa o ponto de partida de um fluxo de atividades (círculo preenchido)
- **Atividade**: Representa uma etapa ou ação no processo (retângulo com bordas arredondadas)
- **Transição**: Indica a direção do fluxo entre atividades (setas)
- **Decisão**: Representa pontos de ramificação condicionais (diamante)
- **Mesclagem**: Une fluxos alternativos (diamante)
- **Fork/Join**: Indica início e término de atividades concorrentes (barra sólida)
- **Nó Final**: Representa o término de um fluxo de atividades (círculo com ponto interno)
- **Partições (Swimlanes)**: Organiza atividades por responsável (zonas verticais ou horizontais)
- **Notas**: Fornecem informações adicionais (retângulo com canto dobrado)

## Diagramas de Atividades Principais

### Autenticação no Sistema

**Descrição**: Este diagrama representa o processo de autenticação dos usuários no sistema, incluindo recuperação de senha e bloqueio por tentativas inválidas.

**Diagrama** (representação textual):
```
[Início] → "Acessar página de login"
↓
"Acessar página de login" → "Inserir credenciais"
↓
"Inserir credenciais" → <Decisão> "Esqueceu a senha?"
↓                         ↓
"Não" → "Sistema valida credenciais"     "Sim" → "Solicitar recuperação de senha"
↓                                                ↓
"Sistema valida credenciais" → <Decisão> "Credenciais válidas?"   "Solicitar recuperação de senha" → "Informar email"
↓                               ↓                                                                     ↓
"Não" → "Incrementar contador de tentativas"                               "Informar email" → "Sistema envia email com link"
↓                                                                                              ↓
"Incrementar contador de tentativas" → <Decisão> "Excedeu limite?"         "Sistema envia email com link" → "Acessar link"
↓                                       ↓                                                                    ↓
"Não" → "Exibir mensagem de erro" → "Inserir credenciais"                  "Acessar link" → "Definir nova senha"
                                                                                             ↓
"Sim" → "Bloquear acesso temporariamente"                                  "Definir nova senha" → "Confirmar nova senha"
↓                                                                                                  ↓
"Bloquear acesso temporariamente" → "Exibir mensagem de bloqueio" → [Fim]  "Confirmar nova senha" → "Senha redefinida"
                                                                                                      ↓
"Sim" → "Carregar perfil do usuário"                                       "Senha redefinida" → [Fim]
↓
"Carregar perfil do usuário" → "Apresentar dashboard" → [Fim]
```

### Registro e Atendimento de Solicitações

**Descrição**: Este diagrama representa o fluxo completo do processo de registro e atendimento de solicitações na Assessoria de Inclusão, desde a identificação da necessidade até a conclusão do atendimento.

**Diagrama** (representação textual):
```
[Início] → <Partição: Coordenador de Unidade> "Identificar necessidade de atendimento"
↓
"Identificar necessidade de atendimento" → "Acessar sistema"
↓
"Acessar sistema" → "Preencher formulário de solicitação"
↓
"Preencher formulário de solicitação" → "Anexar documentos relevantes (opcional)"
↓
"Anexar documentos relevantes (opcional)" → "Submeter solicitação"
↓
"Submeter solicitação" → <Partição: Gestor da Assessoria> "Receber notificação de nova solicitação"
↓
"Receber notificação de nova solicitação" → "Analisar solicitação"
↓
"Analisar solicitação" → <Decisão> "Solicitação válida?"
↓                        ↓
"Não" → "Registrar motivo da recusa"     "Sim" → "Classificar por tipo e prioridade"
↓                                                  ↓
"Registrar motivo da recusa" → "Notificar solicitante"    "Classificar por tipo e prioridade" → "Atribuir responsável"
↓                                                                                               ↓
"Notificar solicitante" → [Fim]                           "Atribuir responsável" → <Partição: Técnico da Assessoria> "Receber atribuição"
                                                                                    ↓
                                                          "Receber atribuição" → "Analisar detalhes do caso"
                                                                                 ↓
                                                          "Analisar detalhes do caso" → <Decisão> "Informações suficientes?"
                                                                                        ↓                 ↓
                                "Sim" → "Planejar ações de atendimento"                "Não" → "Solicitar informações adicionais"
                                         ↓                                                      ↓
                                "Planejar ações de atendimento" → "Executar atendimento"        "Solicitar informações adicionais" → <Partição: Coordenador de Unidade> "Receber solicitação de informações"
                                                                   ↓                                                                  ↓
                                                                "Executar atendimento" → "Registrar ações realizadas"                "Receber solicitação de informações" → "Fornecer informações adicionais"
                                                                                          ↓                                                                                 ↓
                                                                "Registrar ações realizadas" → <Decisão> "Atendimento concluído?"    "Fornecer informações adicionais" → <Partição: Técnico da Assessoria> "Receber informações adicionais"
                                                                                               ↓                ↓                                                          ↓
                                                               "Não" → "Atualizar status"      "Sim" → "Documentar solução"         "Receber informações adicionais" → "Planejar ações de atendimento"
                                                                         ↓                              ↓
                                                               "Atualizar status" → "Executar atendimento"     "Documentar solução" → "Finalizar atendimento"
                                                                                                                                     ↓
                                                                                                               "Finalizar atendimento" → "Notificar conclusão"
                                                                                                                                          ↓
                                                                                                               "Notificar conclusão" → <Partição: Coordenador de Unidade> "Receber notificação de conclusão"
                                                                                                                                                                           ↓
                                                                                                                                        "Receber notificação de conclusão" → "Avaliar atendimento"
                                                                                                                                                                             ↓
                                                                                                                                                               "Avaliar atendimento" → [Fim]
```

### Gestão de Tarefas

**Descrição**: Este diagrama representa o processo de criação, atribuição, execução e finalização de tarefas relacionadas aos atendimentos da Assessoria de Inclusão.

**Diagrama** (representação textual):
```
[Início] → <Partição: Gestor da Assessoria> "Identificar necessidade de tarefa"
↓
"Identificar necessidade de tarefa" → <Decisão> "Vinculada a um atendimento?"
↓                                      ↓
"Sim" → "Selecionar atendimento"       "Não" → "Criar tarefa independente"
↓                                               ↓
"Selecionar atendimento" → "Criar nova tarefa"  "Criar tarefa independente" → "Definir detalhes da tarefa"
                           ↓                                                   ↓
                         "Criar nova tarefa" → "Definir detalhes da tarefa"
                                               ↓
"Definir detalhes da tarefa" → "Estabelecer prioridade e prazo"
↓
"Estabelecer prioridade e prazo" → "Selecionar responsável"
↓
"Selecionar responsável" → "Atribuir tarefa"
↓
"Atribuir tarefa" → <Partição: Técnico da Assessoria> "Receber notificação de nova tarefa"
↓
"Receber notificação de nova tarefa" → "Analisar tarefa atribuída"
↓
"Analisar tarefa atribuída" → "Iniciar execução"
↓
"Iniciar execução" → "Atualizar progresso periodicamente"
↓
"Atualizar progresso periodicamente" → <Decisão> "Tarefa concluída?"
↓                                       ↓
"Não" → "Continuar execução"            "Sim" → "Registrar conclusão"
↓                                                ↓
"Continuar execução" → "Atualizar progresso periodicamente"    "Registrar conclusão" → <Partição: Gestor da Assessoria> "Receber notificação de conclusão"
                                                                                       ↓
                                                               "Receber notificação de conclusão" → "Revisar tarefa concluída"
                                                                                                     ↓
                                                               "Revisar tarefa concluída" → <Decisão> "Aprovada?"
                                                                                            ↓              ↓
                                                               "Não" → "Registrar feedback"               "Sim" → "Finalizar tarefa"
                                                                        ↓                                          ↓
                                                               "Registrar feedback" → <Partição: Técnico da Assessoria> "Receber feedback"    "Finalizar tarefa" → [Fim]
                                                                                      ↓
                                                                                    "Receber feedback" → "Realizar ajustes"
                                                                                                          ↓
                                                                                    "Realizar ajustes" → "Registrar conclusão"
```

### Geração de Relatórios

**Descrição**: Este diagrama representa o processo de geração, visualização e exportação de relatórios no sistema, incluindo tanto relatórios predefinidos quanto personalizados.

**Diagrama** (representação textual):
```
[Início] → <Partição: Gestor ou Técnico> "Acessar módulo de relatórios"
↓
"Acessar módulo de relatórios" → <Decisão> "Tipo de relatório?"
↓                                  ↓
"Predefinido" → "Selecionar relatório da lista"    "Personalizado" → "Acessar construtor de relatórios"
↓                                                                     ↓
"Selecionar relatório da lista" → "Definir parâmetros"               "Acessar construtor de relatórios" → "Selecionar campos desejados"
                                   ↓                                                                       ↓
                                 "Definir parâmetros"                                                    "Selecionar campos desejados" → "Configurar filtros"
                                   ↓                                                                                                      ↓
                                 "Definir parâmetros" → "Gerar relatório"                                "Configurar filtros" → "Definir agrupamentos e ordenação"
                                                        ↓                                                                       ↓
                                                      "Gerar relatório" ← "Definir agrupamentos e ordenação"
                                                        ↓
                                                      <Decisão> "Dados disponíveis?"
                                                        ↓               ↓
                                                      "Não" → "Exibir mensagem de ausência de dados"    "Sim" → "Exibir relatório na tela"
                                                        ↓                                                        ↓
                                                      "Exibir mensagem de ausência de dados"                   "Exibir relatório na tela" → <Decisão> "Satisfeito com resultado?"
                                                        ↓                                                                                    ↓                ↓
                                                      [Fim]                                                                               "Não" → "Ajustar parâmetros ou campos"
                                                                                                                                                   ↓
                                                                                                       "Sim" → <Decisão> "Deseja exportar?"   "Ajustar parâmetros ou campos" → <Decisão> "Tipo de relatório?"
                                                                                                        ↓              ↓
                                                                                              "Não" → [Fim]         "Sim" → "Selecionar formato (PDF, Excel, CSV)"
                                                                                                                             ↓
                                                                                                                           "Selecionar formato (PDF, Excel, CSV)" → "Gerar arquivo para download"
                                                                                                                                                                     ↓
                                                                                                                                                                   "Gerar arquivo para download" → "Baixar arquivo"
                                                                                                                                                                                                   ↓
                                                                                                                                                                                                 "Baixar arquivo" → <Decisão> "Deseja salvar configuração do relatório?"
                                                                                                                                                                                                                     ↓               ↓
                                                                                                                                                                                                                   "Não" → [Fim]   "Sim" → "Nomear configuração"
                                                                                                                                                                                                                                             ↓
                                                                                                                                                                                                                                           "Nomear configuração" → "Salvar como modelo"
                                                                                                                                                                                                                                                                     ↓
                                                                                                                                                                                                                                                                   "Salvar como modelo" → [Fim]
```

### Gestão do Repositório de Materiais

**Descrição**: Este diagrama representa o processo de inclusão, categorização, aprovação e acesso aos materiais e recursos sobre inclusão no repositório do sistema.

**Diagrama** (representação textual):
```
[Início] → <Partição: Técnico ou Gestor> <Decisão> "Operação desejada?"
↓                                          ↓                 ↓
"Adicionar" → "Preparar material"      "Buscar" → "Acessar repositório"     "Atualizar" → "Localizar material existente"
↓                                        ↓                                     ↓
"Preparar material" → "Acessar formulário de cadastro"   "Acessar repositório" → "Definir critérios de busca"   "Localizar material existente" → "Solicitar edição"
↓                                                          ↓                                                        ↓
"Acessar formulário de cadastro" → "Preencher metadados"  "Definir critérios de busca" → "Executar busca"        "Solicitar edição" → <Decisão> "Permissão concedida?"
↓                                                           ↓                                                       ↓                   ↓
"Preencher metadados" → "Fazer upload do arquivo"         "Executar busca" → <Decisão> "Resultados encontrados?"  "Não" → [Fim]      "Sim" → "Modificar material"
↓                                                           ↓                 ↓                                                         ↓
"Fazer upload do arquivo" → "Definir permissões de acesso" "Sim" → "Visualizar lista de resultados"               "Modificar material" → "Documentar alterações"
↓                                                                   ↓                                                                    ↓
"Definir permissões de acesso" → "Submeter para aprovação"         "Visualizar lista de resultados" → "Selecionar material"            "Documentar alterações" → "Submeter atualização"
↓                                                                    ↓                                                                   ↓
"Submeter para aprovação" → <Partição: Gestor Aprovador> "Receber notificação de material para aprovação"         "Submeter atualização" → <Partição: Gestor Aprovador> "Receber notificação de atualização"
                                                          ↓                                                                                                              ↓
                                                        "Receber notificação de material para aprovação" → "Revisar material"                                        "Receber notificação de atualização" → "Revisar alterações"
                                                                                                            ↓                                                                                              ↓
                                                                                                          "Revisar material" → <Decisão> "Aprovado?"                                                     "Revisar alterações" → <Decisão> "Aprovado?"
                                                                                                            ↓                 ↓                                                                           ↓                    ↓
                                                                                                          "Não" → "Registrar motivo da rejeição"   "Sim" → "Aprovar publicação"                        "Não" → "Registrar motivo da rejeição"   "Sim" → "Aprovar atualização"
                                                                                                            ↓                                        ↓                                                   ↓                                        ↓
                                                                                                          "Registrar motivo da rejeição" → "Notificar autor"          "Aprovar publicação" → "Notificar autor"         "Registrar motivo da rejeição" → "Notificar autor"          "Aprovar atualização" → "Notificar autor"
                                                                                                            ↓                                        ↓                                                   ↓                                        ↓
                                                                                                          "Notificar autor" → <Partição: Técnico ou Gestor> "Receber notificação de rejeição"          "Notificar autor" → <Partição: Técnico ou Gestor> "Receber notificação de aprovação"
                                                                                                                                ↓                                                                                            ↓
                                                                                                                              "Receber notificação de rejeição" → <Decisão> "Fazer correções?"                           "Receber notificação de aprovação" → [Fim]
                                                                                                                                                                   ↓                ↓
                                                                                                                                                                 "Sim" → "Modificar material"   "Não" → [Fim]

"Não" → "Exibir mensagem de não encontrado" → [Fim]

"Selecionar material" → "Visualizar detalhes do material"
↓
"Visualizar detalhes do material" → <Decisão> "Ação desejada?"
↓                                    ↓                  ↓
"Visualizar" → "Abrir material"    "Baixar" → "Fazer download"    "Compartilhar" → "Selecionar destinatários"
↓                                    ↓                               ↓
"Abrir material" → [Fim]           "Fazer download" → [Fim]        "Selecionar destinatários" → "Enviar compartilhamento"
                                                                     ↓
                                                                   "Enviar compartilhamento" → [Fim]
```

## Relação com Requisitos e Casos de Uso

A tabela abaixo relaciona cada diagrama de atividades com os requisitos funcionais e casos de uso correspondentes:

| Diagrama de Atividades | Requisitos Funcionais | Casos de Uso |
|------------------------|------------------------|--------------|
| Autenticação no Sistema | RF-01.1: Login no Sistema<br>RF-01.3: Recuperação de Senha | UC-01: Autenticar no Sistema |
| Registro e Atendimento de Solicitações | RF-02.1: Registro de Atendimentos<br>RF-02.2: Categorização de Atendimentos<br>RF-02.3: Acompanhamento de Status<br>RF-02.4: Histórico de Atendimentos<br>RF-02.5: Atribuição de Responsáveis | UC-02: Registrar Atendimento<br>UC-04: Atribuir Responsável por Atendimento |
| Gestão de Tarefas | RF-03.2: Gestão de Tarefas<br>RF-03.3: Notificações e Alertas | HU-10: Gestão de Tarefas<br>HU-12: Atribuição de Responsabilidades |
| Geração de Relatórios | RF-04.1: Relatórios Predefinidos<br>RF-04.2: Gerador de Relatórios Customizados<br>RF-04.3: Exportação de Dados<br>RF-04.4: Visualizações Gráficas | UC-03: Gerar Relatório |
| Gestão do Repositório de Materiais | RF-05.2: Repositório de Materiais | UC-05: Gerenciar Repositório de Materiais |

## Considerações de Implementação

1. **Integração com o Fluxo de Trabalho**: Os diagramas de atividades devem ser considerados em conjunto com os diagramas BPMN para garantir consistência entre os processos de negócio e sua implementação técnica.

2. **Tratamento de Exceções**: Embora os diagramas representem os fluxos principais e alguns fluxos alternativos, a implementação deve considerar tratamentos adicionais de exceções e situações de erro.

3. **Transições de Estados**: As transições entre estados de atendimentos, tarefas e materiais devem ser cuidadosamente implementadas para garantir a integridade dos dados e o registro adequado do histórico.

4. **Notificações**: Um sistema robusto de notificações é essencial para garantir a fluidez dos processos e a comunicação apropriada entre as partes envolvidas.

5. **Personalização de Fluxos**: Considerar a possibilidade de permitir que Gestores da Assessoria possam personalizar certos aspectos dos fluxos de trabalho para atender necessidades específicas.

6. **Rastreabilidade**: Implementar mecanismos para rastrear cada etapa dos processos representados nos diagramas, permitindo auditoria e análise de gargalos.