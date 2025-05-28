# Diagramas de Estados
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação dos Diagramas de Estados](#notação-dos-diagramas-de-estados)
- [Diagramas de Estados das Entidades Principais](#diagramas-de-estados-das-entidades-principais)
  - [Estados de Atendimento](#estados-de-atendimento)
  - [Estados de Tarefa](#estados-de-tarefa)
  - [Estados de Material no Repositório](#estados-de-material-no-repositório)
  - [Estados de Usuário](#estados-de-usuário)
- [Transições e Regras de Negócio](#transições-e-regras-de-negócio)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Este documento apresenta os diagramas de estados para as principais entidades do Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza. Os diagramas de estados são representações visuais que mostram os diferentes estados em que uma entidade pode existir durante seu ciclo de vida, bem como as transições entre esses estados.

Enquanto os diagramas de atividades e BPMN focam nos fluxos de trabalho e processos, os diagramas de estados concentram-se na evolução do estado interno dos objetos do sistema. Eles são particularmente úteis para visualizar o comportamento de entidades que passam por múltiplos estados ao longo de seu ciclo de vida, como atendimentos, tarefas, materiais e usuários.

## Notação dos Diagramas de Estados

Os diagramas de estados neste documento usam a notação UML (Unified Modeling Language) padrão, incluindo os seguintes elementos:

- **Estado**: Representa uma condição ou situação durante a vida de um objeto (retângulo com bordas arredondadas)
- **Estado Inicial**: Representa o ponto de partida (círculo preenchido)
- **Estado Final**: Representa o término do ciclo de vida (círculo com ponto interno)
- **Transição**: Indica a mudança de um estado para outro (seta)
- **Evento**: Causa que desencadeia uma transição (rótulo na seta)
- **Guarda**: Condição que deve ser verdadeira para que a transição ocorra [entre colchetes]
- **Ação**: Comportamento executado durante uma transição (após o símbolo /)
- **Estado Composto**: Estado que contém sub-estados (estado com outros estados dentro)

## Diagramas de Estados das Entidades Principais

### Estados de Atendimento

**Descrição**: Este diagrama representa os diferentes estados pelos quais um atendimento passa desde sua criação até seu encerramento.

**Diagrama** (representação textual):
```
[Estado Inicial] → "Registrado"
                      ↓
"Registrado" -evento: "Gestor analisa"→ "Em Análise"
                      ↓
"Em Análise" -evento: "Recusado"→ "Cancelado"
                      ↓
"Em Análise" -evento: "Aprovado"→ "Atribuído"
                      ↓
"Atribuído" -evento: "Técnico inicia atendimento"→ "Em Atendimento"
                      ↓
"Em Atendimento" -evento: "Solicitar informações adicionais"→ "Aguardando Informações"
                      ↓
"Aguardando Informações" -evento: "Receber informações"→ "Em Atendimento"
                      ↓
"Em Atendimento" -evento: "Finalizar atendimento"→ "Concluído"
                      ↓
"Concluído" -evento: "Reabrir" [dentro de 30 dias]→ "Em Atendimento"
                      ↓
"Concluído" ou "Cancelado" → [Estado Final]
```

**Detalhamento dos Estados**:

1. **Registrado**: Estado inicial após a criação de um atendimento por um Coordenador de Unidade. Contém as informações básicas da solicitação.

2. **Em Análise**: O atendimento está sendo analisado por um Gestor da Assessoria, que verifica sua validade, classifica e determina prioridade.

3. **Cancelado**: O atendimento foi considerado inválido ou improcedente e não prosseguirá.

4. **Atribuído**: O atendimento foi designado a um Técnico responsável, mas o trabalho ainda não foi iniciado.

5. **Em Atendimento**: O Técnico está ativamente trabalhando no caso, implementando soluções e realizando ações.

6. **Aguardando Informações**: O atendimento está temporariamente pausado aguardando informações adicionais do solicitante ou de terceiros.

7. **Concluído**: O atendimento foi finalizado com sucesso e a solução foi documentada.

### Estados de Tarefa

**Descrição**: Este diagrama representa os diferentes estados pelos quais uma tarefa passa desde sua criação até sua conclusão.

**Diagrama** (representação textual):
```
[Estado Inicial] → "Criada"
                     ↓
"Criada" -evento: "Atribuir responsável"→ "Atribuída"
                     ↓
"Atribuída" -evento: "Iniciar execução"→ "Em Andamento"
                     ↓
"Em Andamento" -evento: "Pausar" [justificativa necessária]→ "Suspensa"
                     ↓
"Suspensa" -evento: "Retomar"→ "Em Andamento"
                     ↓
"Em Andamento" -evento: "Registrar conclusão"→ "Aguardando Aprovação"
                     ↓
"Aguardando Aprovação" -evento: "Rejeitar"→ "Em Andamento"
                     ↓
"Aguardando Aprovação" -evento: "Aprovar"→ "Concluída"
                     ↓
"Concluída" → [Estado Final]

"Criada" ou "Atribuída" ou "Em Andamento" ou "Suspensa" -evento: "Cancelar"→ "Cancelada" → [Estado Final]
```

**Detalhamento dos Estados**:

1. **Criada**: Estado inicial após a criação de uma tarefa pelo Gestor. Contém a descrição, prioridade e prazo.

2. **Atribuída**: A tarefa foi designada a um Técnico responsável, mas o trabalho ainda não foi iniciado.

3. **Em Andamento**: O Técnico está ativamente trabalhando na tarefa.

4. **Suspensa**: A tarefa está temporariamente pausada por algum motivo (dependência de outra tarefa, indisponibilidade de recursos, etc.).

5. **Aguardando Aprovação**: O Técnico concluiu a tarefa, mas aguarda a verificação e aprovação do Gestor.

6. **Concluída**: A tarefa foi completada e aprovada pelo Gestor.

7. **Cancelada**: A tarefa foi cancelada e não será mais executada.

### Estados de Material no Repositório

**Descrição**: Este diagrama representa os diferentes estados pelos quais um material no repositório passa desde sua submissão até sua publicação ou remoção.

**Diagrama** (representação textual):
```
[Estado Inicial] → "Rascunho"
                      ↓
"Rascunho" -evento: "Submeter para aprovação"→ "Aguardando Aprovação"
                      ↓
"Aguardando Aprovação" -evento: "Rejeitar"→ "Rejeitado"
                      ↓
"Rejeitado" -evento: "Revisar e resubmeter"→ "Aguardando Aprovação"
                      ↓
"Aguardando Aprovação" -evento: "Aprovar"→ "Publicado"
                      ↓
"Publicado" -evento: "Solicitar atualização"→ "Em Atualização"
                      ↓
"Em Atualização" -evento: "Submeter atualização"→ "Aguardando Aprovação de Atualização"
                      ↓
"Aguardando Aprovação de Atualização" -evento: "Aprovar atualização"→ "Publicado" (nova versão)
                      ↓
"Aguardando Aprovação de Atualização" -evento: "Rejeitar atualização"→ "Publicado" (versão anterior)
                      ↓
"Publicado" -evento: "Marcar como obsoleto"→ "Obsoleto"
                      ↓
"Obsoleto" ou "Rejeitado" -evento: "Arquivar"→ "Arquivado" → [Estado Final]
```

**Detalhamento dos Estados**:

1. **Rascunho**: Estado inicial quando um material está sendo preparado, mas ainda não foi submetido para aprovação.

2. **Aguardando Aprovação**: O material foi submetido e aguarda revisão e aprovação de um Gestor.

3. **Rejeitado**: O material não foi aprovado devido a problemas de qualidade, relevância ou outro motivo.

4. **Publicado**: O material foi aprovado e está disponível para acesso pelos usuários do sistema.

5. **Em Atualização**: O material publicado está sendo modificado ou atualizado.

6. **Aguardando Aprovação de Atualização**: Uma atualização do material foi submetida e aguarda aprovação.

7. **Obsoleto**: O material está desatualizado, mas mantido no sistema para referência histórica.

8. **Arquivado**: O material não está mais disponível para uso regular, mas é mantido para fins de auditoria.

### Estados de Usuário

**Descrição**: Este diagrama representa os diferentes estados pelos quais um usuário passa desde seu cadastro até sua eventual inativação.

**Diagrama** (representação textual):
```
[Estado Inicial] → "Cadastrado"
                     ↓
"Cadastrado" -evento: "Ativar conta"→ "Ativo"
                     ↓
"Ativo" -evento: "Bloquear temporariamente" [múltiplas tentativas de login inválidas]→ "Bloqueado"
                     ↓
"Bloqueado" -evento: "Desbloquear" [tempo expirado ou ação administrativa]→ "Ativo"
                     ↓
"Ativo" -evento: "Inativar" [desligamento ou transferência]→ "Inativo"
                     ↓
"Inativo" -evento: "Reativar"→ "Ativo"
                     ↓
"Inativo" -evento: "Excluir" [apenas para usuários sem registros associados]→ [Estado Final]
```

**Detalhamento dos Estados**:

1. **Cadastrado**: Estado inicial após o cadastro básico do usuário no sistema, mas antes da ativação da conta.

2. **Ativo**: O usuário está ativo e pode acessar o sistema normalmente, de acordo com suas permissões.

3. **Bloqueado**: O acesso do usuário está temporariamente bloqueado, geralmente por múltiplas tentativas de login inválidas.

4. **Inativo**: O usuário não está mais ativo no sistema, possivelmente devido a mudança de função, desligamento ou licença prolongada.

## Transições e Regras de Negócio

Os diagramas de estados apresentados estão diretamente relacionados às regras de negócio definidas para o sistema. A seguir, destacamos as principais relações:

### Atendimentos

1. **Transição para Concluído**:
   - Relacionada à regra RFT-04: "Para encerrar um atendimento como Concluído, é obrigatório o preenchimento de uma descrição da solução implementada."
   - A transição só pode ocorrer se a documentação da solução estiver completa.

2. **Reabertura de Atendimentos**:
   - Relacionada à regra RFT-02: "Atendimentos concluídos podem ser reabertos em até 30 dias após seu fechamento."
   - A transição de "Concluído" para "Em Atendimento" só é permitida dentro deste prazo.

3. **Atribuição de Responsável**:
   - Relacionada à regra RA-02: "Todo atendimento deve ser atribuído a um responsável da Assessoria de Inclusão."
   - A transição de "Em Análise" para "Atribuído" exige a designação de um responsável.

### Tarefas

1. **Aprovação de Conclusão**:
   - Relacionada à supervisão de tarefas pelos Gestores.
   - A transição de "Aguardando Aprovação" para "Concluída" requer revisão e aprovação do Gestor.

2. **Suspensão de Tarefas**:
   - A transição para "Suspensa" requer justificativa formal.

### Materiais

1. **Processo de Aprovação**:
   - Relacionada à regra RGI-02: "Materiais incluídos no repositório devem ser categorizados e podem ter restrições de acesso."
   - A transição de "Aguardando Aprovação" para "Publicado" requer revisão e aprovação de um Gestor.

2. **Controle de Versões**:
   - Relacionada à regra RGI-03: "O sistema deve manter o histórico de versões de materiais e documentos armazenados."
   - As transições entre estados mantêm o controle de versões.

### Usuários

1. **Inativação vs. Exclusão**:
   - Relacionada à regra RUP-04: "Usuários não podem ser excluídos do sistema, apenas inativados."
   - A transição para o estado final (exclusão) só é permitida em casos muito específicos e controlados.

## Considerações de Implementação

1. **Auditoria de Transições**: Todas as transições de estado devem ser registradas com data, hora e usuário responsável para fins de auditoria e rastreabilidade.

2. **Permissões para Transições**: As permissões para realizar transições de estado devem ser rigorosamente controladas de acordo com os perfis de usuário:
   - Transições de aprovação: restritas a Gestores e Administradores
   - Transições operacionais: disponíveis para Técnicos responsáveis
   - Transições específicas: configuradas conforme necessidades do processo

3. **Notificações Automáticas**: O sistema deve enviar notificações automáticas para os interessados quando ocorrerem transições significativas, como:
   - Atribuição de atendimentos/tarefas
   - Conclusão/cancelamento de atendimentos
   - Rejeição/aprovação de materiais
   - Bloqueio de usuários

4. **Visualização de Estados**: A interface do usuário deve apresentar claramente o estado atual de cada entidade, usando indicadores visuais (cores, ícones) para facilitar a identificação rápida.

5. **Histórico de Estados**: O sistema deve manter o histórico completo das transições de estado para cada entidade, permitindo consulta e análise posterior.

6. **Regras de Transição**: A implementação deve validar as condições (guardas) associadas a cada transição antes de permitir a mudança de estado, garantindo a integridade do processo.

7. **Estados Compostos**: Para entidades com comportamentos mais complexos, considerar a implementação de estados compostos que permitam maior granularidade na representação do ciclo de vida.