# Casos de Teste
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Módulo de Autenticação e Controle de Acesso](#módulo-de-autenticação-e-controle-de-acesso)
- [Módulo de Gestão de Atendimentos](#módulo-de-gestão-de-atendimentos)
- [Módulo de Organização do Fluxo de Trabalho](#módulo-de-organização-do-fluxo-de-trabalho)
- [Módulo de Geração de Relatórios](#módulo-de-geração-de-relatórios)
- [Módulo de Gestão de Informações de Inclusão](#módulo-de-gestão-de-informações-de-inclusão)
- [Testes de Requisitos Não-Funcionais](#testes-de-requisitos-não-funcionais)
- [Matriz de Rastreabilidade](#matriz-de-rastreabilidade)

## Introdução

Este documento apresenta os casos de teste detalhados para o Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza. Os casos de teste foram elaborados com base nos requisitos funcionais, não-funcionais, histórias de usuário e casos de uso do sistema, buscando garantir a qualidade e conformidade do produto final.

Cada caso de teste contém:
- Identificador único
- Descrição
- Pré-condições necessárias para execução
- Dados de teste a serem utilizados
- Passos para execução
- Resultado esperado
- Prioridade (Alta, Média, Baixa)
- Status de automação (Total, Parcial, Não automatizado)
- Referência ao requisito relacionado

Os casos estão organizados por módulos do sistema para facilitar a execução e o acompanhamento.

## Módulo de Autenticação e Controle de Acesso

### CT-AAC-001: Login com credenciais válidas

**Descrição**: Verificar se o usuário consegue fazer login com credenciais válidas  
**Pré-condição**: Usuário cadastrado no sistema  
**Dados de teste**: 
- Email: usuario@etec.sp.gov.br
- Senha: SenhaValida123

**Passos**:
1. Acessar a página de login
2. Preencher o campo de email
3. Preencher o campo de senha
4. Clicar no botão "Entrar"

**Resultado esperado**: 
- Usuário autenticado e redirecionado para dashboard
- O sistema deve exibir uma mensagem de boas-vindas com o nome do usuário
- Menu de navegação deve corresponder às permissões do perfil do usuário

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-01.1

### CT-AAC-002: Login com credenciais inválidas

**Descrição**: Verificar se o sistema impede acesso com credenciais inválidas  
**Pré-condição**: N/A  
**Dados de teste**: 
- Email: usuario@etec.sp.gov.br
- Senha: SenhaInvalida

**Passos**:
1. Acessar a página de login
2. Preencher o campo de email
3. Preencher o campo de senha com senha incorreta
4. Clicar no botão "Entrar"

**Resultado esperado**: 
- Mensagem de erro informando que as credenciais são inválidas
- Usuário permanece na página de login
- Nenhuma informação sensível deve ser revelada na mensagem de erro

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-01.1

### CT-AAC-003: Tentativas consecutivas de login sem sucesso

**Descrição**: Verificar se o sistema bloqueia temporariamente o acesso após múltiplas tentativas sem sucesso  
**Pré-condição**: Usuário cadastrado no sistema  
**Dados de teste**: 
- Email: usuario@etec.sp.gov.br
- Senha: SenhasIncorretas

**Passos**:
1. Acessar a página de login
2. Realizar 5 tentativas de login com senhas incorretas para o mesmo usuário
3. Tentar realizar login novamente

**Resultado esperado**: 
- Após a quinta tentativa, o sistema deve bloquear temporariamente o acesso para o usuário
- Mensagem informando sobre o bloqueio temporário e sugerindo recuperação de senha
- Registro da atividade suspeita no log do sistema

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-01.1

### CT-AAC-004: Recuperação de senha

**Descrição**: Verificar se o processo de recuperação de senha funciona corretamente  
**Pré-condição**: Usuário cadastrado no sistema  
**Dados de teste**: 
- Email: usuario@etec.sp.gov.br

**Passos**:
1. Acessar a página de login
2. Clicar em "Esqueci minha senha"
3. Informar o email cadastrado
4. Clicar em "Enviar link"
5. Acessar o email recebido
6. Clicar no link de redefinição
7. Definir nova senha
8. Submeter o formulário

**Resultado esperado**: 
- Sistema confirma envio de email com instruções
- Email contém link seguro para redefinição
- Link permite definir nova senha
- Após redefinição, usuário consegue fazer login com a nova senha
- Link de redefinição deve ser de uso único e com prazo de validade

**Prioridade**: Alta  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-01.1

### CT-AAC-005: Verificação de permissões por perfil - Administrador

**Descrição**: Verificar se o sistema aplica corretamente as permissões para usuários com perfil Administrador  
**Pré-condição**: Usuário logado com perfil Administrador  
**Dados de teste**:
- Usuário: admin@cps.sp.gov.br
- Senha: SenhaAdmin123

**Passos**:
1. Fazer login com o usuário de perfil Administrador
2. Verificar as opções disponíveis no menu
3. Acessar configurações de usuários
4. Tentar criar um novo usuário
5. Tentar acessar relatórios gerenciais
6. Tentar acessar configurações do sistema

**Resultado esperado**: 
- Menu completo disponível, incluindo opções administrativas
- Acesso permitido a todas as funcionalidades testadas
- Capacidade de criar novos usuários
- Acesso a todos os relatórios e configurações

**Prioridade**: Alta  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-01.2, RF-01.3

### CT-AAC-006: Verificação de permissões por perfil - Técnico

**Descrição**: Verificar se o sistema aplica corretamente as permissões para usuários com perfil Técnico  
**Pré-condição**: Usuário logado com perfil Técnico  
**Dados de teste**:
- Usuário: tecnico@cps.sp.gov.br
- Senha: SenhaTecnico123

**Passos**:
1. Fazer login com o usuário de perfil Técnico
2. Verificar as opções disponíveis no menu
3. Tentar acessar configurações de usuários (via URL direta)
4. Tentar acessar atendimentos atribuídos
5. Tentar acessar repositório de materiais

**Resultado esperado**: 
- Menu apresenta apenas opções pertinentes ao perfil Técnico
- Acesso negado às configurações de usuários
- Acesso permitido aos atendimentos atribuídos ao técnico
- Acesso permitido ao repositório de materiais

**Prioridade**: Alta  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-01.2, RF-01.3

### CT-AAC-007: Criação de usuário

**Descrição**: Verificar se é possível criar um novo usuário no sistema  
**Pré-condição**: Usuário logado com perfil Administrador  
**Dados de teste**:
- Nome: Novo Usuário
- Email: novo.usuario@fatec.sp.gov.br
- Perfil: Coordenador
- Unidade: Fatec São Paulo

**Passos**:
1. Acessar o módulo de administração
2. Selecionar "Usuários"
3. Clicar em "Novo Usuário"
4. Preencher os dados do formulário
5. Clicar em "Salvar"

**Resultado esperado**: 
- Usuário criado com sucesso
- Sistema gera senha provisória e envia ao email
- Novo usuário aparece na listagem de usuários
- Log de criação registrado no sistema

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-01.4

### CT-AAC-008: Desativação de usuário

**Descrição**: Verificar se é possível desativar um usuário existente  
**Pré-condição**: Usuário alvo cadastrado e ativo no sistema; Usuário administrador logado  
**Dados de teste**:
- ID do usuário a ser desativado: 42

**Passos**:
1. Acessar o módulo de administração
2. Selecionar "Usuários"
3. Localizar o usuário desejado
4. Clicar em "Desativar"
5. Confirmar a desativação

**Resultado esperado**: 
- Usuário marcado como inativo no sistema
- Usuário desativado não consegue mais fazer login
- Status atualizado na listagem de usuários
- Log de desativação registrado no sistema

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-01.4

## Módulo de Gestão de Atendimentos

### CT-GA-001: Registro de novo atendimento

**Descrição**: Verificar se é possível criar um novo atendimento  
**Pré-condição**: Usuário logado com permissão para criar atendimentos  
**Dados de teste**: 
- Tipo de necessidade: Deficiência Visual
- Unidade: Etec São Paulo
- Descrição: "Aluno necessita de materiais adaptados para deficiência visual"
- Prioridade: Alta

**Passos**:
1. Acessar o módulo de atendimentos
2. Clicar em "Novo Atendimento"
3. Preencher os dados obrigatórios
4. Clicar em "Salvar"

**Resultado esperado**: 
- Atendimento criado com número de protocolo gerado automaticamente
- Status inicial definido como "Aberto"
- Histórico de criação registrado
- Confirmação visual de sucesso

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.1

### CT-GA-002: Categorização de atendimento

**Descrição**: Verificar se é possível categorizar corretamente um atendimento  
**Pré-condição**: Atendimento existente no sistema  
**Dados de teste**: 
- ID de atendimento: 2025
- Tipo de necessidade atual: Não especificado
- Novo tipo de necessidade: Deficiência Auditiva

**Passos**:
1. Acessar o atendimento existente
2. Editar o campo "Tipo de Necessidade"
3. Selecionar "Deficiência Auditiva"
4. Salvar alterações

**Resultado esperado**: 
- Tipo de necessidade atualizado para Deficiência Auditiva
- Alteração registrada no histórico
- Confirmação visual de sucesso

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.2

### CT-GA-003: Atualização de status de atendimento

**Descrição**: Verificar se é possível atualizar o status de um atendimento e se as regras de transição são respeitadas  
**Pré-condição**: Atendimento existente no status "Aberto"  
**Dados de teste**: 
- ID de atendimento: 2025
- Status atual: Aberto
- Novo status: Em Análise
- Comentário: "Iniciando análise do caso"

**Passos**:
1. Acessar o atendimento existente
2. Clicar em "Atualizar Status"
3. Selecionar o novo status "Em Análise"
4. Adicionar comentário
5. Clicar em "Salvar"

**Resultado esperado**: 
- Status atualizado para "Em Análise"
- Comentário e alteração registrados no histórico
- Data de última atualização atualizada
- Confirmação visual de sucesso

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.3

### CT-GA-004: Transição de status inválida

**Descrição**: Verificar se o sistema impede transições de status inválidas  
**Pré-condição**: Atendimento existente no status "Aberto"  
**Dados de teste**: 
- ID de atendimento: 2025
- Status atual: Aberto
- Tentativa de novo status: Concluído (transição inválida)

**Passos**:
1. Acessar o atendimento existente
2. Clicar em "Atualizar Status"
3. Tentar selecionar o status "Concluído"

**Resultado esperado**: 
- Status "Concluído" deve estar desabilitado ou
- Se tentativa forçada (via API), sistema deve retornar erro
- Mensagem explicativa sobre transições válidas
- Status do atendimento permanece inalterado

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.3

### CT-GA-005: Visualização de histórico de atendimento

**Descrição**: Verificar se o histórico de alterações de um atendimento é exibido corretamente  
**Pré-condição**: Atendimento existente com múltiplas alterações  
**Dados de teste**: 
- ID de atendimento: 2025

**Passos**:
1. Acessar o atendimento existente
2. Navegar para a aba "Histórico"

**Resultado esperado**: 
- Lista cronológica de todas as alterações
- Cada entrada contém data/hora, usuário responsável, ação realizada e detalhes
- Histórico organizado do mais recente para o mais antigo
- Mudanças de status destacadas visualmente

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.4

### CT-GA-006: Atribuição de responsável por atendimento

**Descrição**: Verificar se é possível atribuir um técnico responsável para um atendimento  
**Pré-condição**: Atendimento existente sem responsável atribuído, usuário com perfil Gestor  
**Dados de teste**: 
- ID de atendimento: 2025
- Técnico a ser atribuído: Maria Silva (ID: 43)

**Passos**:
1. Acessar o atendimento existente
2. Clicar em "Atribuir Responsável"
3. Selecionar o técnico da lista
4. Clicar em "Confirmar"

**Resultado esperado**: 
- Responsável atribuído ao atendimento
- Alteração registrada no histórico
- Notificação enviada ao técnico atribuído
- Confirmação visual de sucesso

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.5

### CT-GA-007: Reatribuição de responsável

**Descrição**: Verificar se é possível alterar o técnico responsável por um atendimento  
**Pré-condição**: Atendimento existente com responsável já atribuído, usuário com perfil Gestor  
**Dados de teste**: 
- ID de atendimento: 2025
- Responsável atual: Maria Silva (ID: 43)
- Novo responsável: João Santos (ID: 44)
- Motivo: "Especialidade mais adequada ao caso"

**Passos**:
1. Acessar o atendimento existente
2. Clicar em "Atribuir Responsável"
3. Selecionar o novo técnico da lista
4. Informar motivo da reatribuição
5. Clicar em "Confirmar"

**Resultado esperado**: 
- Responsável atualizado no atendimento
- Alteração registrada no histórico com o motivo
- Notificação enviada ao novo técnico
- Notificação enviada ao técnico anterior
- Confirmação visual de sucesso

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.5

### CT-GA-008: Filtro de atendimentos por status

**Descrição**: Verificar se o filtro de atendimentos por status funciona corretamente  
**Pré-condição**: Múltiplos atendimentos cadastrados com diferentes status  
**Dados de teste**: 
- Status para filtro: "Em Análise"

**Passos**:
1. Acessar a listagem de atendimentos
2. Aplicar filtro por status "Em Análise"

**Resultado esperado**: 
- Lista exibe apenas atendimentos com status "Em Análise"
- Contador de resultados atualizado
- Indicação visual do filtro aplicado

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-02.3

## Módulo de Organização do Fluxo de Trabalho

### CT-OF-001: Visualização de dashboard personalizado

**Descrição**: Verificar se o dashboard exibe informações adequadas conforme o perfil do usuário  
**Pré-condição**: Usuários cadastrados com diferentes perfis  
**Dados de teste**: 
- Usuário Gestor: gestor@cps.sp.gov.br / SenhaGestor123
- Usuário Técnico: tecnico@cps.sp.gov.br / SenhaTecnico123
- Usuário Coordenador: coordenador@etec.sp.gov.br / SenhaCoordenador123

**Passos**:
1. Fazer login com usuário de perfil Gestor
2. Verificar componentes do dashboard
3. Fazer logout
4. Fazer login com usuário de perfil Técnico
5. Verificar componentes do dashboard
6. Fazer logout
7. Fazer login com usuário de perfil Coordenador
8. Verificar componentes do dashboard

**Resultado esperado**: 
- Dashboard do Gestor: visão geral de todos os atendimentos, métricas gerais, gráficos consolidados
- Dashboard do Técnico: atendimentos atribuídos, tarefas pendentes, próximos prazos
- Dashboard do Coordenador: atendimentos da unidade, status de solicitações

**Prioridade**: Média  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-03.1

### CT-OF-002: Criação de tarefa associada a atendimento

**Descrição**: Verificar se é possível criar uma tarefa vinculada a um atendimento  
**Pré-condição**: Atendimento existente no sistema, usuário com permissão adequada  
**Dados de teste**: 
- ID de atendimento: 2025
- Dados da tarefa:
  - Título: "Contatar intérprete de Libras"
  - Descrição: "Verificar disponibilidade do intérprete João para aulas de Matemática"
  - Responsável: Maria Silva (ID: 43)
  - Prazo: Data atual + 2 dias
  - Prioridade: Alta

**Passos**:
1. Acessar o atendimento existente
2. Clicar em "Nova Tarefa"
3. Preencher os dados da tarefa
4. Clicar em "Salvar"

**Resultado esperado**: 
- Tarefa criada e vinculada ao atendimento
- Tarefa visível na lista de tarefas do atendimento
- Tarefa adicionada à lista de tarefas do responsável
- Notificação enviada ao responsável
- Confirmação visual de sucesso

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-03.2

### CT-OF-003: Atualização de status de tarefa

**Descrição**: Verificar se é possível atualizar o status de uma tarefa  
**Pré-condição**: Tarefa existente no status "Pendente", usuário responsável pela tarefa logado  
**Dados de teste**: 
- ID da tarefa: 345
- Status atual: Pendente
- Novo status: Em Andamento
- Comentário: "Iniciando contato com intérprete"

**Passos**:
1. Acessar a lista de tarefas
2. Localizar a tarefa específica
3. Clicar em "Atualizar Status"
4. Selecionar status "Em Andamento"
5. Adicionar comentário
6. Clicar em "Salvar"

**Resultado esperado**: 
- Status da tarefa atualizado para "Em Andamento"
- Comentário registrado
- Data de atualização registrada
- Confirmação visual de sucesso

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-03.2

### CT-OF-004: Conclusão de tarefa

**Descrição**: Verificar se é possível marcar uma tarefa como concluída  
**Pré-condição**: Tarefa existente no status "Em Andamento", usuário responsável pela tarefa logado  
**Dados de teste**: 
- ID da tarefa: 345
- Status atual: Em Andamento
- Novo status: Concluída
- Comentário: "Intérprete contatado e confirmado para as aulas"

**Passos**:
1. Acessar a lista de tarefas
2. Localizar a tarefa específica
3. Clicar em "Atualizar Status"
4. Selecionar status "Concluída"
5. Adicionar comentário
6. Clicar em "Salvar"

**Resultado esperado**: 
- Status da tarefa atualizado para "Concluída"
- Comentário registrado
- Data de conclusão registrada
- Tarefa removida da lista de pendentes
- Notificação enviada ao criador da tarefa
- Confirmação visual de sucesso

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RF-03.2

### CT-OF-005: Recebimento de notificação

**Descrição**: Verificar se as notificações são enviadas e recebidas corretamente  
**Pré-condição**: Configuração de evento que gera notificação  
**Dados de teste**: 
- Evento: Atribuição de responsável por atendimento
- Usuário destinatário: tecnico@cps.sp.gov.br / SenhaTecnico123

**Passos**:
1. Logar com usuário gestor
2. Criar atendimento ou atribuir responsável (evento gerador)
3. Fazer logout
4. Fazer login com usuário técnico destinatário
5. Verificar recebimento de notificação

**Resultado esperado**: 
- Indicador de notificação não lida visível
- Notificação presente na lista de notificações
- Conteúdo da notificação correto e relacionado ao evento gerador
- Link na notificação leva ao item relacionado

**Prioridade**: Média  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-03.3

### CT-OF-006: Marcação de notificação como lida

**Descrição**: Verificar se é possível marcar notificações como lidas  
**Pré-condição**: Usuário com notificações não lidas  
**Dados de teste**: 
- Usuário: tecnico@cps.sp.gov.br / SenhaTecnico123
- ID da notificação: 1024

**Passos**:
1. Fazer login com o usuário
2. Acessar o centro de notificações
3. Selecionar a notificação
4. Clicar em "Marcar como lida"

**Resultado esperado**: 
- Notificação marcada como lida
- Contador de notificações não lidas atualizado
- Indicador visual da notificação alterado

**Prioridade**: Baixa  
**Automatizado**: Sim  
**Requisito relacionado**: RF-03.3

### CT-OF-007: Configuração de fluxo de trabalho para tipo de atendimento

**Descrição**: Verificar se é possível configurar fluxos de trabalho específicos para diferentes tipos de atendimento  
**Pré-condição**: Usuário logado como Administrador ou Gestor  
**Dados de teste**: 
- Tipo de necessidade: Deficiência Visual
- Estados do fluxo: Aberto → Avaliação → Providenciar Material → Teste → Concluído
- Prazos estimados para cada etapa

**Passos**:
1. Acessar o módulo de configurações
2. Selecionar "Fluxos de Trabalho"
3. Clicar em "Novo Fluxo"
4. Selecionar tipo de necessidade
5. Configurar etapas e transições
6. Definir prazos estimados
7. Salvar configuração

**Resultado esperado**: 
- Fluxo de trabalho criado e associado ao tipo de necessidade
- Visualização gráfica do fluxo disponível
- Configuração aplicada a novos atendimentos deste tipo
- Confirmação visual de sucesso

**Prioridade**: Baixa  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-03.4

## Módulo de Geração de Relatórios

### CT-GR-001: Geração de relatório predefinido

**Descrição**: Verificar se os relatórios predefinidos são gerados corretamente  
**Pré-condição**: Usuário com permissão para acessar relatórios, dados suficientes no sistema  
**Dados de teste**: 
- Tipo de relatório: "Atendimentos por Tipo de Necessidade"
- Período: Último trimestre
- Filtro: Todas as unidades

**Passos**:
1. Acessar o módulo de relatórios
2. Selecionar o relatório predefinido
3. Configurar parâmetros (período, filtros)
4. Clicar em "Gerar Relatório"

**Resultado esperado**: 
- Relatório gerado e exibido na tela
- Dados completos e precisos conforme os filtros
- Totalizadores corretos
- Opções de exportação disponíveis

**Prioridade**: Alta  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-04.1

### CT-GR-002: Geração de relatório sem dados

**Descrição**: Verificar o comportamento do sistema ao tentar gerar um relatório para um período sem dados  
**Pré-condição**: Usuário com permissão para acessar relatórios  
**Dados de teste**: 
- Tipo de relatório: "Atendimentos por Tipo de Necessidade"
- Período: Ano anterior (sem registros)
- Filtro: Unidade específica sem atendimentos

**Passos**:
1. Acessar o módulo de relatórios
2. Selecionar o relatório predefinido
3. Configurar parâmetros deliberadamente para período sem dados
4. Clicar em "Gerar Relatório"

**Resultado esperado**: 
- Mensagem informando que não há dados para os filtros selecionados
- Sugestão para alterar os filtros
- Não deve ocorrer erro ou exceção

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-04.1

### CT-GR-003: Criação de relatório personalizado

**Descrição**: Verificar se é possível criar e salvar um relatório personalizado  
**Pré-condição**: Usuário com permissão para criar relatórios  
**Dados de teste**: 
- Campos para o relatório: Unidade, Tipo de Necessidade, Quantidade de Atendimentos, Tempo Médio de Resolução
- Filtros: Período específico, Status "Concluído"
- Agrupamento: Por unidade e tipo de necessidade
- Ordenação: Tempo médio de resolução (decrescente)

**Passos**:
1. Acessar o gerador de relatórios
2. Selecionar "Novo Relatório Personalizado"
3. Definir título "Tempo de Resolução por Tipo e Unidade"
4. Selecionar campos desejados
5. Configurar filtros, agrupamentos e ordenação
6. Visualizar prévia
7. Salvar configuração

**Resultado esperado**: 
- Relatório personalizado salvo com sucesso
- Relatório disponível na lista de relatórios do usuário
- Prévia exibe dados corretos conforme configuração
- Confirmação visual de sucesso

**Prioridade**: Média  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-04.2

### CT-GR-004: Exportação de relatório para PDF

**Descrição**: Verificar se é possível exportar relatórios em formato PDF  
**Pré-condição**: Relatório gerado no sistema  
**Dados de teste**: 
- Relatório: "Atendimentos por Tipo de Necessidade"
- Formato: PDF

**Passos**:
1. Gerar um relatório
2. Clicar em "Exportar"
3. Selecionar formato "PDF"
4. Iniciar download

**Resultado esperado**: 
- Arquivo PDF gerado corretamente
- Arquivo contém todos os dados do relatório
- Formatação adequada para o formato
- Cabeçalhos e rodapés com informações do sistema
- Download iniciado automaticamente ou link disponibilizado

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-04.3

### CT-GR-005: Exportação de relatório para Excel

**Descrição**: Verificar se é possível exportar relatórios em formato Excel  
**Pré-condição**: Relatório gerado no sistema  
**Dados de teste**: 
- Relatório: "Atendimentos por Tipo de Necessidade"
- Formato: Excel

**Passos**:
1. Gerar um relatório
2. Clicar em "Exportar"
3. Selecionar formato "Excel"
4. Iniciar download

**Resultado esperado**: 
- Arquivo Excel (.xlsx) gerado corretamente
- Arquivo contém todos os dados do relatório
- Formatação adequada incluindo cabeçalhos de coluna
- Fórmulas para totalizadores
- Download iniciado automaticamente ou link disponibilizado

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-04.3

### CT-GR-006: Visualização gráfica de dados

**Descrição**: Verificar se as visualizações gráficas de relatórios funcionam corretamente  
**Pré-condição**: Relatório gerado com dados adequados para visualização gráfica  
**Dados de teste**: 
- Relatório: "Atendimentos por Tipo de Necessidade"
- Visualização: Gráfico de barras

**Passos**:
1. Gerar um relatório
2. Clicar na aba "Visualização Gráfica"
3. Selecionar tipo de gráfico "Barras"
4. Configurar eixos (se aplicável)

**Resultado esperado**: 
- Gráfico gerado corretamente
- Dados representados visualmente de forma precisa
- Legenda e rótulos corretos
- Interatividade (tooltip ao passar o mouse)

**Prioridade**: Média  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-04.4

## Módulo de Gestão de Informações de Inclusão

### CT-GI-001: Cadastro de unidade

**Descrição**: Verificar se é possível cadastrar uma nova unidade educacional  
**Pré-condição**: Usuário com perfil Administrador ou Gestor  
**Dados de teste**: 
- Tipo: Etec
- Nome: Etec Teste
- Cidade: São Paulo
- Endereço: Av. Teste, 123
- Telefone: (11) 1234-5678
- Email: etec.teste@etec.sp.gov.br

**Passos**:
1. Acessar o módulo de cadastros
2. Selecionar "Unidades"
3. Clicar em "Nova Unidade"
4. Preencher os dados
5. Clicar em "Salvar"

**Resultado esperado**: 
- Unidade cadastrada com sucesso
- Dados salvos corretamente
- Unidade aparece na listagem
- Confirmação visual de sucesso

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-05.1

### CT-GI-002: Cadastro de unidade com email duplicado

**Descrição**: Verificar se o sistema impede o cadastro de unidade com email já existente  
**Pré-condição**: Unidade já cadastrada com email etec.existente@etec.sp.gov.br  
**Dados de teste**: 
- Tipo: Etec
- Nome: Etec Duplicada
- Cidade: São Paulo
- Endereço: Av. Teste, 456
- Telefone: (11) 8765-4321
- Email: etec.existente@etec.sp.gov.br (já existe no sistema)

**Passos**:
1. Acessar o módulo de cadastros
2. Selecionar "Unidades"
3. Clicar em "Nova Unidade"
4. Preencher os dados com email já existente
5. Clicar em "Salvar"

**Resultado esperado**: 
- Mensagem de erro indicando que o email já está em uso
- Unidade não é cadastrada
- Formulário mantém dados preenchidos para correção

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-05.1

### CT-GI-003: Upload de material para repositório

**Descrição**: Verificar se é possível fazer upload de material para o repositório  
**Pré-condição**: Usuário logado com permissão para gerenciar materiais  
**Dados de teste**: 
- Título: "Guia de Adaptações para Deficiência Visual"
- Descrição: "Manual com orientações práticas para adaptação de materiais didáticos"
- Categoria: "Manuais e Guias"
- Tags: "deficiência visual", "adaptação", "material didático"
- Arquivo: documento PDF válido (tamanho < 10MB)

**Passos**:
1. Acessar o repositório de materiais
2. Clicar em "Novo Material"
3. Preencher os metadados
4. Fazer upload do arquivo
5. Clicar em "Salvar"

**Resultado esperado**: 
- Material salvo e disponível no repositório
- Arquivo armazenado corretamente
- Metadados registrados
- Material aparece nas buscas utilizando as tags definidas
- Confirmação visual de sucesso

**Prioridade**: Média  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-05.2

### CT-GI-004: Upload de material com formato inválido

**Descrição**: Verificar se o sistema impede o upload de arquivos com formato não permitido  
**Pré-condição**: Usuário logado com permissão para gerenciar materiais  
**Dados de teste**: 
- Título: "Guia de Adaptações para Deficiência Visual"
- Descrição: "Manual com orientações práticas para adaptação de materiais didáticos"
- Categoria: "Manuais e Guias"
- Arquivo: arquivo executável (.exe)

**Passos**:
1. Acessar o repositório de materiais
2. Clicar em "Novo Material"
3. Preencher os metadados
4. Tentar fazer upload do arquivo executável
5. Clicar em "Salvar"

**Resultado esperado**: 
- Mensagem de erro sobre formato de arquivo não permitido
- Upload bloqueado
- Material não é salvo
- Lista de formatos permitidos apresentada

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-05.2

### CT-GI-005: Busca de material por palavras-chave

**Descrição**: Verificar a funcionalidade de busca no repositório de materiais  
**Pré-condição**: Materiais cadastrados no repositório com metadados  
**Dados de teste**: 
- Termo de busca: "deficiência visual"

**Passos**:
1. Acessar o repositório de materiais
2. Inserir o termo de busca no campo de pesquisa
3. Executar a busca

**Resultado esperado**: 
- Lista de materiais que contêm o termo buscado (no título, descrição ou tags)
- Resultados ordenados por relevância
- Destacamento (highlight) dos termos encontrados
- Contador de resultados

**Prioridade**: Média  
**Automatizado**: Sim  
**Requisito relacionado**: RF-05.2

### CT-GI-006: Versionamento de material

**Descrição**: Verificar se o sistema gerencia corretamente novas versões de material existente  
**Pré-condição**: Material existente no repositório  
**Dados de teste**: 
- ID do material: 128
- Versão atual: 1
- Arquivo para nova versão: documento PDF atualizado

**Passos**:
1. Acessar o repositório de materiais
2. Localizar o material existente
3. Clicar em "Editar"
4. Fazer upload de novo arquivo
5. Adicionar comentário sobre a atualização
6. Clicar em "Salvar"

**Resultado esperado**: 
- Nova versão registrada (versão 2)
- Histórico de versões disponível
- Versão antiga preservada e acessível
- Versão atual definida como a mais recente
- Registro de quem fez a atualização e quando

**Prioridade**: Baixa  
**Automatizado**: Parcial  
**Requisito relacionado**: RF-05.2

## Testes de Requisitos Não-Funcionais

### CT-NF-001: Tempo de resposta em operações críticas

**Descrição**: Verificar se o sistema atende ao requisito de tempo de resposta  
**Pré-condição**: Sistema com volume de dados representativo  
**Dados de teste**: N/A

**Passos**:
1. Medir tempo de resposta para login
2. Medir tempo de resposta para carregamento de listagem de atendimentos
3. Medir tempo de resposta para geração de relatório simples
4. Medir tempo de resposta para busca no repositório

**Resultado esperado**: 
- Tempo de login < 2 segundos
- Tempo de carregamento de listagens < 3 segundos
- Tempo de geração de relatório simples < 5 segundos
- Tempo de busca < 3 segundos

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RNF-03.1

### CT-NF-002: Teste de carga com usuários simultâneos

**Descrição**: Verificar se o sistema suporta o número especificado de usuários simultâneos  
**Pré-condição**: Ambiente de teste com capacidade para simular múltiplos usuários  
**Dados de teste**: Scripts de teste simulando ações comuns de usuários

**Passos**:
1. Configurar teste com 50 usuários simultâneos
2. Executar cenários típicos (login, consulta, alteração de status)
3. Monitorar métricas de desempenho
4. Aumentar gradualmente até 100 usuários

**Resultado esperado**: 
- Sistema mantém operação normal com 50 usuários
- Degradação aceitável de performance até 100 usuários
- Sem erros de timeout ou falhas de conexão
- Sem deadlocks ou condições de corrida

**Prioridade**: Alta  
**Automatizado**: Sim  
**Requisito relacionado**: RNF-03.2

### CT-NF-003: Teste de acessibilidade WCAG

**Descrição**: Verificar se o sistema atende às diretrizes de acessibilidade WCAG 2.1 nível AA  
**Pré-condição**: Sistema em ambiente de teste com todas as interfaces implementadas  
**Dados de teste**: N/A

**Passos**:
1. Executar ferramentas automatizadas (WAVE, Axe) em todas as páginas principais
2. Verificar navegação por teclado
3. Testar com leitor de tela
4. Verificar contraste de cores
5. Testar redimensionamento de texto

**Resultado esperado**: 
- Sem erros de acessibilidade de nível A ou AA
- Navegação por teclado funcional em todas as interfaces
- Compatibilidade com leitores de tela
- Contraste adequado para texto e elementos de interface
- Layout responsivo ao redimensionar texto

**Prioridade**: Alta  
**Automatizado**: Parcial  
**Requisito relacionado**: RNF-02

### CT-NF-004: Teste de compatibilidade com navegadores

**Descrição**: Verificar se o sistema funciona corretamente em diferentes navegadores  
**Pré-condição**: Sistema em ambiente de teste com todas as interfaces implementadas  
**Dados de teste**: N/A

**Passos**:
1. Testar funcionalidades principais no Chrome
2. Testar funcionalidades principais no Firefox
3. Testar funcionalidades principais no Edge
4. Testar funcionalidades principais no Safari

**Resultado esperado**: 
- Funcionalidades principais operam corretamente em todos os navegadores
- Sem diferenças significativas de layout
- Sem erros JavaScript específicos de navegador
- Formulários funcionam consistentemente

**Prioridade**: Alta  
**Automatizado**: Parcial  
**Requisito relacionado**: RNF-04

### CT-NF-005: Teste de recuperação de sessão

**Descrição**: Verificar se o sistema recupera adequadamente a sessão do usuário após desconexão temporária  
**Pré-condição**: Usuário autenticado realizando trabalho no sistema  
**Dados de teste**: Formulário de atendimento parcialmente preenchido

**Passos**:
1. Iniciar preenchimento de formulário de atendimento
2. Simular perda de conexão temporária
3. Restabelecer conexão
4. Verificar estado do formulário

**Resultado esperado**: 
- Dados do formulário preservados
- Sessão mantida ativa
- Mensagem adequada sobre reconexão
- Possibilidade de continuar o trabalho sem perda de dados

**Prioridade**: Média  
**Automatizado**: Parcial  
**Requisito relacionado**: RNF-03

## Matriz de Rastreabilidade

Esta matriz relaciona os casos de teste aos requisitos funcionais e não funcionais do sistema:

| Caso de Teste | Requisito Relacionado | Prioridade |
|---------------|----------------------|------------|
| CT-AAC-001    | RF-01.1              | Alta       |
| CT-AAC-002    | RF-01.1              | Alta       |
| CT-AAC-003    | RF-01.1              | Média      |
| CT-AAC-004    | RF-01.1              | Alta       |
| CT-AAC-005    | RF-01.2, RF-01.3     | Alta       |
| CT-AAC-006    | RF-01.2, RF-01.3     | Alta       |
| CT-AAC-007    | RF-01.4              | Alta       |
| CT-AAC-008    | RF-01.4              | Média      |
| CT-GA-001     | RF-02.1              | Alta       |
| CT-GA-002     | RF-02.2              | Média      |
| CT-GA-003     | RF-02.3              | Alta       |
| CT-GA-004     | RF-02.3              | Média      |
| CT-GA-005     | RF-02.4              | Média      |
| CT-GA-006     | RF-02.5              | Alta       |
| CT-GA-007     | RF-02.5              | Média      |
| CT-GA-008     | RF-02.3              | Média      |
| CT-OF-001     | RF-03.1              | Média      |
| CT-OF-002     | RF-03.2              | Alta       |
| CT-OF-003     | RF-03.2              | Alta       |
| CT-OF-004     | RF-03.2              | Alta       |
| CT-OF-005     | RF-03.3              | Média      |
| CT-OF-006     | RF-03.3              | Baixa      |
| CT-OF-007     | RF-03.4              | Baixa      |
| CT-GR-001     | RF-04.1              | Alta       |
| CT-GR-002     | RF-04.1              | Média      |
| CT-GR-003     | RF-04.2              | Média      |
| CT-GR-004     | RF-04.3              | Média      |
| CT-GR-005     | RF-04.3              | Média      |
| CT-GR-006     | RF-04.4              | Média      |
| CT-GI-001     | RF-05.1              | Média      |
| CT-GI-002     | RF-05.1              | Média      |
| CT-GI-003     | RF-05.2              | Média      |
| CT-GI-004     | RF-05.2              | Média      |
| CT-GI-005     | RF-05.2              | Média      |
| CT-GI-006     | RF-05.2              | Baixa      |
| CT-NF-001     | RNF-03.1             | Alta       |
| CT-NF-002     | RNF-03.2             | Alta       |
| CT-NF-003     | RNF-02               | Alta       |
| CT-NF-004     | RNF-04               | Alta       |
| CT-NF-005     | RNF-03               | Média      |