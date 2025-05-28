# Business Process Model and Notation (BPMN)
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação BPMN Utilizada](#notação-bpmn-utilizada)
- [Processos Principais](#processos-principais)
  - [Processo de Registro e Atendimento de Solicitações](#processo-de-registro-e-atendimento-de-solicitações)
  - [Processo de Gestão de Tarefas](#processo-de-gestão-de-tarefas)
  - [Processo de Geração de Relatórios](#processo-de-geração-de-relatórios)
  - [Processo de Gestão do Repositório de Materiais](#processo-de-gestão-do-repositório-de-materiais)
- [Mapeamento entre Processos e Requisitos](#mapeamento-entre-processos-e-requisitos)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Este documento apresenta a modelagem dos principais processos de negócio relacionados ao Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza, utilizando a notação BPMN (Business Process Model and Notation). A modelagem de processos permite visualizar, analisar e otimizar os fluxos de trabalho que serão suportados pelo sistema, garantindo que a solução desenvolvida esteja alinhada com as necessidades operacionais da Assessoria de Inclusão.

Os diagramas BPMN apresentados neste documento representam uma visão detalhada dos processos que serão automatizados ou facilitados pelo sistema, identificando atores, atividades, decisões, eventos e fluxos de informação.

## Notação BPMN Utilizada

Para a representação dos processos, utilizamos os seguintes elementos da notação BPMN 2.0:

- **Pools e Lanes**: Representam organizações, departamentos ou papéis que participam do processo
- **Atividades**: Representam tarefas ou subprocessos realizados durante o processo
- **Eventos**: Indicam algo que ocorre durante o processo (início, fim, mensagens, temporizadores)
- **Gateways**: Representam pontos de decisão ou paralelismo no fluxo
- **Fluxos de Sequência**: Indicam a ordem das atividades
- **Fluxos de Mensagem**: Representam comunicação entre participantes
- **Artefatos**: Fornecem informações adicionais sobre o processo (anotações, grupos, objetos de dados)

## Processos Principais

### Processo de Registro e Atendimento de Solicitações

**Descrição**: Este processo modela o fluxo completo desde o registro de uma nova solicitação de atendimento até sua conclusão, incluindo todas as etapas intermediárias de análise, atribuição e acompanhamento.

**Diagrama BPMN** (representação textual):

```
Pool: Sistema de Gestão da Assessoria de Inclusão
  Lane: Coordenador de Unidade
    Evento de Início -> Atividade: Identificar necessidade de atendimento -> Atividade: Registrar solicitação no sistema -> Evento de Mensagem (envia)
    Evento de Mensagem (recebe) -> Atividade: Visualizar status do atendimento -> Atividade: Fornecer informações adicionais (quando solicitado) -> Evento de Mensagem (envia)
    Evento de Mensagem (recebe notificação de conclusão) -> Atividade: Avaliar atendimento -> Evento de Fim
  
  Lane: Gestor da Assessoria
    Evento de Mensagem (recebe nova solicitação) -> Atividade: Analisar solicitação -> Gateway (decisão): Solicitação válida?
    Gateway (Não) -> Atividade: Registrar motivo da recusa -> Evento de Mensagem (envia notificação)
    Gateway (Sim) -> Atividade: Classificar por tipo de necessidade e prioridade -> Atividade: Atribuir responsável -> Evento de Mensagem (envia atribuição)
    Evento de Mensagem (recebe atualizações) -> Atividade: Monitorar andamento dos atendimentos -> Evento Intermediário: Temporizador (verificação periódica)
  
  Lane: Técnico da Assessoria
    Evento de Mensagem (recebe atribuição) -> Atividade: Analisar detalhes do atendimento -> Gateway (decisão): Necessita mais informações?
    Gateway (Sim) -> Atividade: Solicitar informações adicionais -> Evento de Mensagem (envia solicitação)
    Gateway (Não) ou Evento de Mensagem (recebe informações) -> Atividade: Executar atendimento -> Atividade: Registrar ações realizadas
    Gateway (decisão): Atendimento concluído?
    Gateway (Não) -> Atividade: Atualizar status -> Evento de Mensagem (envia atualização)
    Gateway (Sim) -> Atividade: Documentar solução -> Atividade: Finalizar atendimento -> Evento de Mensagem (envia notificação de conclusão)
```

**Detalhamento**:

1. **Início do Processo**: Um Coordenador de Unidade identifica uma necessidade de atendimento relacionada à inclusão.

2. **Registro da Solicitação**: O Coordenador registra a solicitação no sistema, fornecendo informações como unidade, tipo de necessidade, descrição detalhada e prioridade sugerida.

3. **Análise Inicial**: O Gestor da Assessoria recebe a solicitação, verifica sua validade e, se necessário, reclassifica o tipo de necessidade e ajusta a prioridade conforme critérios estabelecidos.

4. **Atribuição de Responsável**: O Gestor atribui um Técnico responsável para o atendimento, que recebe uma notificação.

5. **Execução do Atendimento**: O Técnico analisa o caso e, se necessário, solicita informações adicionais. Após receber todas as informações necessárias, executa o atendimento, registrando todas as ações realizadas.

6. **Acompanhamento**: Durante o atendimento, o Técnico atualiza o status regularmente, e o Gestor monitora o andamento de todos os atendimentos.

7. **Conclusão**: Quando o atendimento é concluído, o Técnico documenta a solução implementada e finaliza o caso. O Coordenador da Unidade é notificado e pode avaliar o atendimento.

**Regras de Negócio Aplicadas**:
- RA-01: Registro de Atendimentos
- RA-02: Atribuição de Responsável
- RA-03: Priorização de Atendimentos
- RA-04: Prazos de Atendimento
- RA-06: Histórico de Alterações
- RFT-01: Estados de Atendimento
- RFT-04: Encerramento de Atendimentos

### Processo de Gestão de Tarefas

**Descrição**: Este processo modela o fluxo de criação, atribuição e acompanhamento de tarefas específicas relacionadas aos atendimentos da Assessoria de Inclusão.

**Diagrama BPMN** (representação textual):

```
Pool: Sistema de Gestão da Assessoria de Inclusão
  Lane: Gestor da Assessoria
    Evento de Início -> Atividade: Identificar necessidade de tarefa -> Atividade: Criar tarefa -> Atividade: Definir prioridade e prazo -> Atividade: Atribuir responsável -> Evento de Mensagem (envia atribuição)
    Evento Intermediário: Temporizador (verificação periódica) -> Atividade: Verificar tarefas próximas do prazo -> Gateway: Tarefas com prazo crítico?
    Gateway (Sim) -> Atividade: Enviar lembrete -> Evento de Mensagem (envia lembrete)
    Evento de Mensagem (recebe atualização) -> Atividade: Revisar tarefa concluída -> Gateway: Aprovada?
    Gateway (Não) -> Atividade: Registrar feedback -> Evento de Mensagem (envia feedback)
    Gateway (Sim) -> Atividade: Finalizar tarefa -> Evento de Fim
  
  Lane: Técnico da Assessoria
    Evento de Mensagem (recebe atribuição) -> Atividade: Analisar tarefa -> Atividade: Executar tarefa
    Evento de Mensagem (recebe lembrete, opcional) -> Atividade: Atualizar progresso -> Gateway: Concluída?
    Gateway (Não) -> Evento Intermediário: Temporizador -> Atividade: Continuar execução
    Gateway (Sim) -> Atividade: Registrar conclusão -> Evento de Mensagem (envia atualização)
    Evento de Mensagem (recebe feedback, condicional) -> Atividade: Ajustar conforme feedback -> Volta para Atividade: Executar tarefa
```

**Detalhamento**:

1. **Criação de Tarefa**: O Gestor da Assessoria identifica a necessidade de uma tarefa específica, que pode estar relacionada a um atendimento ou ser uma atividade independente. Ele cria a tarefa no sistema, definindo descrição, prioridade, prazo e atribuindo um responsável.

2. **Recebimento e Execução**: O Técnico responsável recebe a notificação de nova tarefa, analisa os detalhes e inicia a execução.

3. **Monitoramento**: O sistema monitora automaticamente os prazos das tarefas e envia lembretes quando estão próximas do vencimento. O Técnico atualiza o progresso periodicamente.

4. **Conclusão**: Quando a tarefa é concluída, o Técnico registra a conclusão no sistema. O Gestor recebe uma notificação para revisar o trabalho realizado.

5. **Aprovação**: O Gestor revisa a tarefa concluída e pode aprová-la ou solicitar ajustes. Se aprovada, a tarefa é finalizada. Caso contrário, o Técnico recebe feedback e faz os ajustes necessários.

**Regras de Negócio Aplicadas**:
- RFT-05: Vinculação de Tarefas
- RFT-03: Notificações Automáticas
- RUP-03: Atribuição de Responsáveis

### Processo de Geração de Relatórios

**Descrição**: Este processo modela o fluxo de geração, personalização e distribuição de relatórios sobre as atividades da Assessoria de Inclusão.

**Diagrama BPMN** (representação textual):

```
Pool: Sistema de Gestão da Assessoria de Inclusão
  Lane: Gestor da Assessoria
    Evento de Início -> Gateway: Tipo de relatório?
    Gateway (Predefinido) -> Atividade: Selecionar relatório predefinido -> Atividade: Definir parâmetros (período, filtros)
    Gateway (Personalizado) -> Atividade: Acessar construtor de relatórios -> Atividade: Selecionar campos -> Atividade: Definir filtros e agrupamentos
    Ambos caminhos -> Atividade: Gerar relatório -> Atividade: Visualizar relatório -> Gateway: Ajustes necessários?
    Gateway (Sim) -> Volta para Gateway: Tipo de relatório?
    Gateway (Não) -> Gateway: Exportar?
    Gateway (Sim) -> Atividade: Selecionar formato de exportação -> Atividade: Baixar arquivo
    Gateway (Não) ou após download -> Gateway: Compartilhar?
    Gateway (Sim) -> Atividade: Selecionar destinatários -> Atividade: Adicionar comentários -> Atividade: Enviar relatório -> Evento de Mensagem (envia)
    Gateway (Não) ou após envio -> Evento de Fim
  
  Lane: Sistema
    Evento Intermediário: Temporizador (relatório programado) -> Atividade: Gerar relatório automático -> Atividade: Salvar no sistema -> Gateway: Envio automático configurado?
    Gateway (Sim) -> Atividade: Distribuir para destinatários configurados -> Evento de Mensagem (envia)
    Gateway (Não) -> Evento de Fim
```

**Detalhamento**:

1. **Iniciação da Geração de Relatório**: O Gestor da Assessoria inicia o processo selecionando entre um relatório predefinido ou um relatório personalizado.

2. **Configuração do Relatório**: 
   - Para relatórios predefinidos, o Gestor seleciona o tipo de relatório desejado e define parâmetros como período e filtros específicos.
   - Para relatórios personalizados, o Gestor utiliza o construtor de relatórios para selecionar campos, definir filtros, agrupamentos e ordenações.

3. **Geração e Visualização**: O sistema processa a solicitação e apresenta o relatório na tela. O Gestor pode visualizar e decidir se são necessários ajustes.

4. **Exportação**: O Gestor pode optar por exportar o relatório em diferentes formatos (PDF, Excel, CSV) para uso externo.

5. **Compartilhamento**: O relatório pode ser compartilhado com outros usuários do sistema, incluindo comentários adicionais.

6. **Automatização**: O sistema também pode gerar relatórios automaticamente conforme programação, salvando-os no sistema e opcionalmente distribuindo para destinatários configurados.

**Regras de Negócio Aplicadas**:
- RRE-01: Periodicidade de Relatórios
- RRE-02: Indicadores de Performance
- RRE-03: Exportação de Dados
- RCP-02: Anonimização em Relatórios

### Processo de Gestão do Repositório de Materiais

**Descrição**: Este processo modela o fluxo de inclusão, categorização, acesso e atualização de materiais e recursos sobre inclusão no repositório do sistema.

**Diagrama BPMN** (representação textual):

```
Pool: Sistema de Gestão da Assessoria de Inclusão
  Lane: Técnico ou Gestor da Assessoria
    Evento de Início -> Gateway: Tipo de operação?
    
    Gateway (Adicionar Material) -> Atividade: Preparar material -> Atividade: Acessar formulário de cadastro -> Atividade: Preencher metadados (título, descrição, categoria) -> Atividade: Fazer upload do arquivo -> Atividade: Definir permissões de acesso -> Atividade: Submeter material -> Evento de Mensagem (envia para aprovação)
    
    Gateway (Buscar Material) -> Atividade: Acessar repositório -> Atividade: Aplicar filtros/busca -> Atividade: Visualizar resultados -> Gateway: Material encontrado?
    Gateway (Não) -> Evento de Fim
    Gateway (Sim) -> Atividade: Acessar material -> Gateway: Ação no material?
    Gateway (Download) -> Atividade: Baixar material -> Evento de Fim
    Gateway (Compartilhar) -> Atividade: Selecionar destinatários -> Atividade: Enviar link -> Evento de Mensagem (envia link)
    
    Gateway (Atualizar Material) -> Atividade: Localizar material existente -> Atividade: Solicitar edição -> Atividade: Modificar metadados ou arquivo -> Atividade: Registrar motivo da alteração -> Atividade: Submeter atualização -> Evento de Mensagem (envia para aprovação)
  
  Lane: Gestor da Assessoria (Aprovador)
    Evento de Mensagem (recebe submissão ou atualização) -> Atividade: Revisar material -> Gateway: Aprovado?
    Gateway (Não) -> Atividade: Registrar feedback -> Evento de Mensagem (envia feedback)
    Gateway (Sim) -> Atividade: Aprovar publicação -> Evento de Mensagem (notifica submissor)
    
  Lane: Sistema
    Evento de Mensagem (recebe aprovação) -> Atividade: Publicar no repositório -> Atividade: Indexar material para busca -> Atividade: Aplicar configurações de acesso -> Evento de Fim
```

**Detalhamento**:

1. **Adição de Material**:
   - Um Técnico ou Gestor prepara um material para inclusão no repositório.
   - Submete o material com metadados completos (título, descrição, categoria, tags) e define as permissões de acesso.
   - O material é enviado para aprovação por um Gestor designado como aprovador.
   - Após aprovação, o sistema publica o material no repositório.

2. **Busca e Acesso**:
   - Usuários acessam o repositório e utilizam filtros ou busca textual para localizar materiais.
   - Ao encontrar o material desejado, podem visualizá-lo, baixá-lo ou compartilhá-lo com outros usuários do sistema.

3. **Atualização de Material**:
   - O responsável localiza o material existente que precisa ser atualizado.
   - Solicita permissão de edição, faz as modificações necessárias e registra o motivo da alteração.
   - A atualização é enviada para aprovação e, quando aprovada, substitui a versão anterior no repositório.

4. **Controle de Versões**:
   - O sistema mantém um histórico de versões para cada material, permitindo consultar versões anteriores quando necessário.

**Regras de Negócio Aplicadas**:
- RGI-02: Repositório de Materiais
- RGI-03: Versão de Materiais
- RCP-01: Tratamento de Dados Pessoais (para materiais que contenham informações sensíveis)

## Mapeamento entre Processos e Requisitos

A tabela abaixo relaciona os processos modelados com os requisitos funcionais definidos no documento de Especificação de Requisitos de Software (SRS):

| Processo | Requisitos Funcionais Relacionados |
|---------|-----------------------------------|
| Registro e Atendimento de Solicitações | RF-02.1: Registro de Atendimentos<br>RF-02.2: Categorização de Atendimentos<br>RF-02.3: Acompanhamento de Status<br>RF-02.4: Histórico de Atendimentos<br>RF-02.5: Atribuição de Responsáveis |
| Gestão de Tarefas | RF-03.2: Gestão de Tarefas<br>RF-03.3: Notificações e Alertas |
| Geração de Relatórios | RF-04.1: Relatórios Predefinidos<br>RF-04.2: Gerador de Relatórios Customizados<br>RF-04.3: Exportação de Dados<br>RF-04.4: Visualizações Gráficas |
| Gestão do Repositório de Materiais | RF-05.2: Repositório de Materiais |

## Considerações de Implementação

1. **Automação de Notificações**: Os processos modelados incluem diversos pontos onde notificações automáticas são essenciais para o fluxo adequado. A implementação deve garantir que essas notificações sejam confiáveis e tempestivas.

2. **Rastreabilidade**: É fundamental manter a rastreabilidade completa das atividades realizadas em cada processo, registrando os responsáveis, datas e alterações para fins de auditoria e histórico.

3. **Flexibilidade de Fluxos**: Embora os processos estejam bem definidos, a implementação deve permitir certa flexibilidade para acomodar exceções e casos especiais que possam surgir na operação real da Assessoria de Inclusão.

4. **Integração entre Processos**: Os processos modelados não são isolados e frequentemente interagem entre si. Por exemplo, tarefas são frequentemente criadas a partir de atendimentos, e relatórios podem ser gerados sobre qualquer aspecto dos demais processos.

5. **Dashboard de Acompanhamento**: Recomenda-se a implementação de dashboards específicos para cada perfil de usuário, permitindo o acompanhamento visual dos processos em que estão envolvidos.

6. **Conformidade com Regras de Negócio**: A implementação deve garantir que todas as regras de negócio documentadas sejam aplicadas nos pontos apropriados dos processos modelados.