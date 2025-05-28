# Dicionário de Dados
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Convenções Utilizadas](#convenções-utilizadas)
- [Tabelas e Entidades](#tabelas-e-entidades)
  - [PERFIL](#perfil)
  - [USUARIO](#usuario)
  - [UNIDADE](#unidade)
  - [COORDENADOR](#coordenador)
  - [TECNICO](#tecnico)
  - [TIPO_NECESSIDADE](#tipo_necessidade)
  - [STATUS_ATENDIMENTO](#status_atendimento)
  - [PRIORIDADE](#prioridade)
  - [ATENDIMENTO](#atendimento)
  - [HISTORICO_ATENDIMENTO](#historico_atendimento)
  - [TAREFA](#tarefa)
  - [CATEGORIA_MATERIAL](#categoria_material)
  - [MATERIAL](#material)
  - [HISTORICO_MATERIAL](#historico_material)
  - [RELATORIO](#relatorio)
  - [CONFIG_RELATORIO](#config_relatorio)
  - [NOTIFICACAO](#notificacao)
  - [LOG](#log)
- [Domínios e Tipos Enumerados](#domínios-e-tipos-enumerados)
- [Relacionamentos](#relacionamentos)
- [Índices](#índices)
- [Referências](#referências)

## Introdução

Este dicionário de dados fornece uma descrição detalhada de todas as entidades, atributos e relacionamentos no banco de dados do Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza. O documento serve como referência para desenvolvedores, analistas de banco de dados e outros stakeholders técnicos que necessitam compreender a estrutura dos dados do sistema.

O dicionário de dados complementa os modelos lógico e físico, fornecendo informações adicionais sobre o propósito de cada elemento do banco de dados, suas restrições, tipos de dados, domínios e exemplos de valores.

## Convenções Utilizadas

### Tipos de Dados
- **INTEGER/SERIAL**: Números inteiros
- **VARCHAR(n)**: Texto de comprimento variável com máximo de n caracteres
- **TEXT**: Texto de comprimento variável sem limite predefinido
- **TIMESTAMP WITH TIME ZONE**: Data e hora com informação de fuso horário
- **BOOLEAN**: Valor booleano (verdadeiro/falso)
- **JSONB**: Dados JSON armazenados em formato binário
- **ARRAY**: Coleção de elementos do mesmo tipo

### Restrições
- **PK**: Primary Key (Chave Primária)
- **FK**: Foreign Key (Chave Estrangeira)
- **NN**: Not Null (Valor não pode ser nulo)
- **UQ**: Unique (Valor deve ser único)
- **CK**: Check (Restrição de verificação)
- **DF**: Default (Valor padrão)

## Tabelas e Entidades

### PERFIL

Armazena informações sobre os perfis de acesso no sistema, definindo conjuntos de permissões para diferentes tipos de usuários.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| perfil_id | SERIAL | PK, NN | Identificador único do perfil | Número inteiro positivo | 1 |
| nome | VARCHAR(50) | NN, UQ | Nome do perfil | Texto | "Administrador" |
| descricao | TEXT | NN | Descrição das responsabilidades e características do perfil | Texto | "Acesso total ao sistema para configuração e manutenção" |
| permissoes | JSONB | NN, DF='{}' | Matriz de permissões em formato JSON | Objeto JSON | {"atendimentos": {"criar": true, "editar": true, "excluir": true}, "relatorios": {"visualizar": true, "criar": true}} |

**Descrição**: Define os diferentes perfis de usuário e suas respectivas permissões no sistema. Implementa o modelo RBAC (Role-Based Access Control).

### USUARIO

Representa todos os usuários do sistema, independente de sua função específica.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| usuario_id | SERIAL | PK, NN | Identificador único do usuário | Número inteiro positivo | 42 |
| nome | VARCHAR(100) | NN | Nome completo do usuário | Texto | "Maria Silva" |
| email | VARCHAR(100) | NN, UQ, CK | Email institucional para login | Email válido com domínio institucional | "maria.silva@etec.sp.gov.br" |
| senha_hash | VARCHAR(255) | NN | Hash da senha para autenticação | Texto criptografado | "5e884898da2847151d0e56f8dc629..." |
| perfil_id | INTEGER | FK, NN | Referência ao perfil do usuário | Chave estrangeira para PERFIL | 2 |
| data_cadastro | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora do cadastro no sistema | Data/hora válida | "2025-05-15 14:30:00-03" |
| ultimo_acesso | TIMESTAMP WITH TIME ZONE | - | Data/hora do último acesso | Data/hora válida | "2025-05-28 09:45:12-03" |
| status | VARCHAR(10) | NN, DF='Ativo', CK | Status do usuário | 'Ativo', 'Inativo' | "Ativo" |

**Descrição**: Armazena informações básicas sobre os usuários do sistema, incluindo credenciais de acesso e status. O email deve ser institucional (domínios @cps.sp.gov.br, @etec.sp.gov.br ou @fatec.sp.gov.br).

### UNIDADE

Representa as unidades educacionais do CPS (Etecs e Fatecs).

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| unidade_id | SERIAL | PK, NN | Identificador único da unidade | Número inteiro positivo | 135 |
| tipo | VARCHAR(10) | NN, CK | Tipo da unidade | 'Etec', 'Fatec' | "Etec" |
| nome | VARCHAR(100) | NN | Nome da unidade | Texto | "Etec de São Paulo" |
| cidade | VARCHAR(100) | NN | Cidade da unidade | Texto | "São Paulo" |
| endereco | TEXT | NN | Endereço completo | Texto | "Av. Tiradentes, 615 - Luz" |
| telefone | VARCHAR(20) | - | Telefone principal | Número de telefone | "(11) 3322-2200" |
| email | VARCHAR(100) | NN | Email institucional da unidade | Email válido | "e135dir@cps.sp.gov.br" |

**Descrição**: Contém informações sobre as unidades de ensino do CPS. A combinação de tipo, nome e cidade deve ser única para evitar duplicidade.

### COORDENADOR

Associa usuários a unidades como coordenadores, representando uma especialização do usuário.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| usuario_id | INTEGER | PK, FK, NN | Identificador do usuário coordenador | Referência a USUARIO | 42 |
| unidade_id | INTEGER | FK, NN | Identificador da unidade | Referência a UNIDADE | 135 |

**Descrição**: Estabelece a relação entre coordenadores e suas respectivas unidades educacionais. Implementada como uma especialização da entidade USUARIO.

### TECNICO

Representa os técnicos especializados da Assessoria de Inclusão, como uma especialização do usuário.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| usuario_id | INTEGER | PK, FK, NN | Identificador do usuário técnico | Referência a USUARIO | 43 |
| especialidade | VARCHAR(100) | NN | Área de especialização | Texto | "Deficiência Visual" |
| disponibilidade | TEXT | - | Informações sobre disponibilidade | Texto | "Segunda a sexta, 8h às 17h" |

**Descrição**: Armazena informações específicas sobre os técnicos da Assessoria de Inclusão, incluindo suas áreas de especialização e disponibilidade.

### TIPO_NECESSIDADE

Categoriza os diferentes tipos de necessidades educacionais especiais.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| tipo_necessidade_id | SERIAL | PK, NN | Identificador único do tipo | Número inteiro positivo | 3 |
| nome | VARCHAR(100) | NN, UQ | Nome do tipo de necessidade | Texto | "Deficiência Auditiva" |
| descricao | TEXT | NN | Descrição detalhada | Texto | "Perda bilateral, parcial ou total da audição" |
| recomendacoes | TEXT | - | Recomendações padronizadas | Texto | "Disponibilizar intérprete de Libras, utilizar recursos visuais..." |

**Descrição**: Define os tipos de necessidades educacionais especiais atendidas pela Assessoria, fornecendo descrições e recomendações padronizadas para cada tipo.

### STATUS_ATENDIMENTO

Define os possíveis estados de um atendimento e suas transições permitidas.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| status_id | SERIAL | PK, NN | Identificador único do status | Número inteiro positivo | 1 |
| nome | VARCHAR(30) | NN, UQ | Nome do status | Texto | "Aberto" |
| descricao | TEXT | NN | Descrição do status | Texto | "Caso registrado, aguardando triagem" |
| permite_transicao_para | INTEGER[] | - | IDs dos status permitidos após este | Array de inteiros | {2, 5, 6} |

**Descrição**: Controla os possíveis estados de um atendimento e define as transições válidas entre eles, implementando um fluxo de trabalho estruturado.

### PRIORIDADE

Define os níveis de prioridade para atendimentos e tarefas.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| prioridade_id | SERIAL | PK, NN | Identificador único da prioridade | Número inteiro positivo | 3 |
| nome | VARCHAR(20) | NN, UQ | Nome da prioridade | Texto | "Alta" |
| nivel | INTEGER | NN | Nível numérico para ordenação | Número inteiro | 3 |
| descricao | TEXT | NN | Descrição da prioridade | Texto | "Requer atenção prioritária" |

**Descrição**: Estabelece os diferentes níveis de prioridade que podem ser atribuídos a atendimentos e tarefas, permitindo a ordenação e filtragem por criticidade.

### ATENDIMENTO

Registra os casos de atendimento realizados pela Assessoria de Inclusão.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| atendimento_id | SERIAL | PK, NN | Identificador único do atendimento | Número inteiro positivo | 2025 |
| protocolo | VARCHAR(20) | NN, UQ, CK | Código de protocolo | Formato "ATD-YYMMDD" + sequencial | "ATD-250515001" |
| tipo_necessidade_id | INTEGER | FK, NN | Tipo de necessidade educacional | Referência a TIPO_NECESSIDADE | 3 |
| unidade_id | INTEGER | FK, NN | Unidade solicitante | Referência a UNIDADE | 135 |
| responsavel_id | INTEGER | FK | Técnico responsável pelo atendimento | Referência a TECNICO | 43 |
| data_abertura | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora de registro do atendimento | Data/hora válida | "2025-05-15 14:30:00-03" |
| ultima_atualizacao | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora da última atualização | Data/hora válida | "2025-05-28 09:45:12-03" |
| status_id | INTEGER | FK, NN | Estado atual do atendimento | Referência a STATUS_ATENDIMENTO | 2 |
| descricao | TEXT | NN | Detalhamento da necessidade | Texto | "Aluno com deficiência auditiva necessita de intérprete para aulas de Matemática" |
| prioridade_id | INTEGER | FK, NN | Nível de prioridade | Referência a PRIORIDADE | 3 |

**Descrição**: Entidade central que registra todos os casos de atendimento da Assessoria, incluindo informações sobre a unidade solicitante, tipo de necessidade, responsável e status atual.

### HISTORICO_ATENDIMENTO

Rastreia todas as alterações ocorridas em um atendimento.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| historico_id | SERIAL | PK, NN | Identificador único do registro histórico | Número inteiro positivo | 5042 |
| atendimento_id | INTEGER | FK, NN | Atendimento relacionado | Referência a ATENDIMENTO | 2025 |
| usuario_id | INTEGER | FK, NN | Usuário que realizou a ação | Referência a USUARIO | 43 |
| data_hora | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora da alteração | Data/hora válida | "2025-05-20 14:30:00-03" |
| acao_realizada | VARCHAR(100) | NN | Descrição da ação realizada | Texto | "Alteração de Status" |
| status_anterior_id | INTEGER | FK | Status anterior do atendimento | Referência a STATUS_ATENDIMENTO | 1 |
| status_novo_id | INTEGER | FK | Novo status do atendimento | Referência a STATUS_ATENDIMENTO | 2 |
| observacoes | TEXT | - | Comentários adicionais | Texto | "Iniciado processo de análise após contato com coordenador" |

**Descrição**: Implementa o padrão de auditoria para rastreabilidade de todas as ações realizadas em um atendimento, permitindo reconstruir a linha do tempo completa do caso.

### TAREFA

Representa atividades específicas relacionadas a um atendimento.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| tarefa_id | SERIAL | PK, NN | Identificador único da tarefa | Número inteiro positivo | 345 |
| atendimento_id | INTEGER | FK, NN | Atendimento relacionado | Referência a ATENDIMENTO | 2025 |
| responsavel_id | INTEGER | FK | Usuário responsável pela tarefa | Referência a USUARIO | 43 |
| titulo | VARCHAR(100) | NN | Título descritivo da tarefa | Texto | "Contato com intérprete de Libras" |
| descricao | TEXT | NN | Detalhamento da atividade | Texto | "Entrar em contato com intérprete João para verificar disponibilidade" |
| data_criacao | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora de criação | Data/hora válida | "2025-05-20 14:30:00-03" |
| prazo | TIMESTAMP WITH TIME ZONE | CK | Data/hora limite para conclusão | Data futura | "2025-05-25 17:00:00-03" |
| status | VARCHAR(20) | NN, DF='Pendente', CK | Estado atual da tarefa | 'Pendente', 'Em Andamento', 'Concluída', 'Bloqueada', 'Cancelada' | "Em Andamento" |
| prioridade_id | INTEGER | FK, NN | Nível de prioridade | Referência a PRIORIDADE | 3 |

**Descrição**: Permite decompor um atendimento em atividades específicas e atribuíveis, facilitando o gerenciamento do fluxo de trabalho e acompanhamento de responsabilidades.

### CATEGORIA_MATERIAL

Classifica materiais do repositório em categorias temáticas.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| categoria_id | SERIAL | PK, NN | Identificador único da categoria | Número inteiro positivo | 4 |
| nome | VARCHAR(100) | NN, UQ | Nome da categoria | Texto | "Legislação" |
| descricao | TEXT | NN | Descrição do escopo da categoria | Texto | "Leis, decretos e normativas relacionadas à inclusão" |

**Descrição**: Organiza o repositório de materiais em categorias temáticas, facilitando a navegação, busca e organização do conhecimento.

### MATERIAL

Representa documentos e recursos do repositório de conhecimento.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| material_id | SERIAL | PK, NN | Identificador único do material | Número inteiro positivo | 128 |
| titulo | VARCHAR(200) | NN | Título do material | Texto | "Guia de Adaptações para Deficiência Visual" |
| descricao | TEXT | NN | Descrição do conteúdo | Texto | "Manual com orientações práticas para adaptação de materiais didáticos" |
| categoria_id | INTEGER | FK, NN | Categoria do material | Referência a CATEGORIA_MATERIAL | 2 |
| autor_id | INTEGER | FK, NN | Usuário que incluiu o material | Referência a USUARIO | 43 |
| formato | VARCHAR(20) | NN | Formato do arquivo | Texto | "PDF" |
| data_inclusao | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora da inclusão | Data/hora válida | "2025-04-10 11:25:00-03" |
| versao | INTEGER | NN, DF=1 | Versão atual do documento | Número inteiro positivo | 2 |
| arquivo_path | VARCHAR(255) | NN | Caminho para o arquivo no storage | Caminho válido | "/materiais/2025/04/guia-adapt-def-visual-v2.pdf" |
| tags | TEXT[] | DF='{}' | Array de palavras-chave | Array de texto | {"deficiência visual", "adaptação", "material didático"} |
| estado | VARCHAR(20) | NN, DF='Rascunho', CK | Estado do material | 'Rascunho', 'Revisão', 'Publicado', 'Arquivado' | "Publicado" |
| texto_busca | TSVECTOR | - | Índice para pesquisa textual | Vetor de tokens | 'adapt':3,8 'deficiênc':1 'guia':2 'visual':5 |

**Descrição**: Armazena metadados dos documentos e recursos do repositório de conhecimento, facilitando a organização, busca e versionamento de materiais relacionados à inclusão.

### HISTORICO_MATERIAL

Registra alterações em materiais do repositório.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| historico_id | SERIAL | PK, NN | Identificador único do registro histórico | Número inteiro positivo | 456 |
| material_id | INTEGER | FK, NN | Material relacionado | Referência a MATERIAL | 128 |
| usuario_id | INTEGER | FK, NN | Usuário que realizou a ação | Referência a USUARIO | 43 |
| data_hora | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora da alteração | Data/hora válida | "2025-05-20 14:30:00-03" |
| acao | VARCHAR(100) | NN | Tipo de ação realizada | Texto | "Atualização" |
| versao_anterior | INTEGER | - | Versão antes da alteração | Número inteiro | 1 |
| versao_nova | INTEGER | - | Versão após alteração | Número inteiro | 2 |
| observacoes | TEXT | - | Comentários sobre a alteração | Texto | "Adicionadas informações sobre impressão em Braille" |

**Descrição**: Permite controle de versão e histórico de modificações em materiais do repositório, facilitando a recuperação de versões anteriores e auditoria de alterações.

### RELATORIO

Define modelos de relatórios disponíveis no sistema.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| relatorio_id | SERIAL | PK, NN | Identificador único do relatório | Número inteiro positivo | 7 |
| titulo | VARCHAR(100) | NN | Título do relatório | Texto | "Atendimentos por Tipo de Necessidade" |
| descricao | TEXT | NN | Descrição do conteúdo e propósito | Texto | "Quantitativo de atendimentos agrupados por tipo de necessidade" |
| tipo | VARCHAR(50) | NN | Tipo de relatório | Texto | "Estatístico" |
| criador_id | INTEGER | FK, NN | Usuário que criou o modelo | Referência a USUARIO | 42 |
| campos_disponiveis | JSONB | NN | Definição dos campos disponíveis | Objeto JSON | {"campos": ["tipo_necessidade", "quantidade", "percentual"], "filtros": ["periodo", "unidade"]} |
| predefinido | BOOLEAN | NN, DF=false | Indica se é um relatório padrão do sistema | Boolean | true |

**Descrição**: Define os modelos de relatório disponíveis no sistema, especificando seus campos, filtros e características, permitindo tanto relatórios predefinidos quanto customizados.

### CONFIG_RELATORIO

Armazena configurações específicas para execução de relatórios.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| config_id | SERIAL | PK, NN | Identificador único da configuração | Número inteiro positivo | 23 |
| relatorio_id | INTEGER | FK, NN | Relatório associado | Referência a RELATORIO | 7 |
| parametros | JSONB | NN | Parâmetros configurados | Objeto JSON | {"periodo": {"inicio": "2025-01-01", "fim": "2025-05-31"}, "unidade_id": null, "agrupar_por": "mes"} |
| data_ultima_execucao | TIMESTAMP WITH TIME ZONE | - | Data/hora da última execução | Data/hora válida | "2025-05-28 09:45:12-03" |
| agendamento | TEXT | - | Expressão cron para agendamento | Expressão cron válida | "0 7 1 * *" |

**Descrição**: Permite salvar configurações específicas para relatórios, incluindo parâmetros personalizados e agendamento para execução periódica automatizada.

### NOTIFICACAO

Armazena notificações enviadas aos usuários do sistema.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| notificacao_id | SERIAL | PK, NN | Identificador único da notificação | Número inteiro positivo | 1024 |
| destinatario_id | INTEGER | FK, NN | Usuário destinatário | Referência a USUARIO | 43 |
| tipo | VARCHAR(50) | NN | Tipo de notificação | Texto | "Nova Tarefa" |
| titulo | VARCHAR(100) | NN | Título da notificação | Texto | "Tarefa atribuída: Contato com intérprete" |
| conteudo | TEXT | NN | Conteúdo completo | Texto | "Você foi designado responsável pela tarefa 'Contato com intérprete de Libras' no atendimento ATD-250515001" |
| data_envio | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora do envio | Data/hora válida | "2025-05-20 14:30:00-03" |
| lida | BOOLEAN | NN, DF=false | Indica se foi lida pelo destinatário | Boolean | false |

**Descrição**: Sistema de notificações internas para informar usuários sobre eventos relevantes como novas tarefas, prazos próximos ou atualizações em atendimentos.

### LOG

Registra operações realizadas no sistema para fins de auditoria.

| Atributo | Tipo | Restrições | Descrição | Valores Permitidos | Exemplo |
|----------|------|------------|-----------|-------------------|---------|
| log_id | BIGSERIAL | PK, NN | Identificador único do log | Número inteiro positivo | 98765 |
| data_hora | TIMESTAMP WITH TIME ZONE | NN, DF=CURRENT_TIMESTAMP | Data/hora do evento | Data/hora válida | "2025-05-20 14:30:00-03" |
| usuario_id | INTEGER | FK | Usuário que realizou a ação | Referência a USUARIO | 43 |
| acao | VARCHAR(100) | NN | Descrição da ação | Texto | "Atualização" |
| entidade | VARCHAR(50) | NN | Tipo de entidade afetada | Nome de tabela | "ATENDIMENTO" |
| entidade_id | INTEGER | - | ID da entidade afetada | Número inteiro | 2025 |
| detalhes | JSONB | - | Informações adicionais | Objeto JSON | {"antes": {"status_id": 1}, "depois": {"status_id": 2}} |

**Descrição**: Mecanismo de auditoria que registra todas as operações significativas realizadas no sistema, permitindo rastreabilidade para fins de segurança e conformidade.

## Domínios e Tipos Enumerados

### Status de Usuário
- **Ativo**: Conta de usuário ativa no sistema
- **Inativo**: Conta de usuário desativada temporária ou permanentemente

### Tipo de Unidade
- **Etec**: Escola Técnica Estadual
- **Fatec**: Faculdade de Tecnologia Estadual

### Status de Atendimento
1. **Aberto**: Caso registrado, aguardando triagem
   - *Transições permitidas*: Em Análise, Cancelado
2. **Em Análise**: Em avaliação pela equipe
   - *Transições permitidas*: Em Atendimento, Aguardando Informações, Cancelado
3. **Em Atendimento**: Atendimento em andamento
   - *Transições permitidas*: Aguardando Informações, Concluído, Cancelado
4. **Aguardando Informações**: Aguardando dados adicionais
   - *Transições permitidas*: Em Análise, Em Atendimento, Cancelado
5. **Concluído**: Atendimento finalizado com sucesso
   - *Transições permitidas*: Reaberto
6. **Cancelado**: Atendimento cancelado
   - *Transições permitidas*: Reaberto
7. **Reaberto**: Atendimento anteriormente fechado, reaberto para novas ações
   - *Transições permitidas*: Em Análise, Em Atendimento

### Status de Tarefa
- **Pendente**: Ainda não iniciada
- **Em Andamento**: Trabalho iniciado
- **Concluída**: Finalizada com sucesso
- **Bloqueada**: Impedida por algum motivo
- **Cancelada**: Não será executada

### Níveis de Prioridade
1. **Baixa**: Pode ser tratado quando houver disponibilidade
2. **Média**: Requer atenção em tempo hábil
3. **Alta**: Requer atenção prioritária
4. **Crítica**: Requer atenção imediata

### Estado de Material
- **Rascunho**: Em preparação
- **Revisão**: Aguardando aprovação
- **Publicado**: Disponível para consulta
- **Arquivado**: Não mais em uso ativo

## Relacionamentos

### Principais Relacionamentos

1. **USUARIO - PERFIL** (N:1)
   - Um usuário pertence a exatamente um perfil
   - Um perfil pode ser associado a múltiplos usuários
   - Implementado pela chave estrangeira `perfil_id` em USUARIO

2. **COORDENADOR - USUARIO** (1:1)
   - Cada registro em COORDENADOR estende um usuário específico
   - Implementado pela chave primária e estrangeira `usuario_id` em COORDENADOR

3. **COORDENADOR - UNIDADE** (N:1)
   - Um coordenador está associado a uma unidade específica
   - Uma unidade pode ter múltiplos coordenadores
   - Implementado pela chave estrangeira `unidade_id` em COORDENADOR

4. **TECNICO - USUARIO** (1:1)
   - Cada registro em TECNICO estende um usuário específico
   - Implementado pela chave primária e estrangeira `usuario_id` em TECNICO

5. **ATENDIMENTO - UNIDADE** (N:1)
   - Cada atendimento está associado a uma unidade específica
   - Uma unidade pode ter múltiplos atendimentos
   - Implementado pela chave estrangeira `unidade_id` em ATENDIMENTO

6. **ATENDIMENTO - TIPO_NECESSIDADE** (N:1)
   - Cada atendimento é classificado em um tipo de necessidade
   - Um tipo de necessidade pode ser aplicado a múltiplos atendimentos
   - Implementado pela chave estrangeira `tipo_necessidade_id` em ATENDIMENTO

7. **ATENDIMENTO - TECNICO** (N:1)
   - Cada atendimento tem um técnico responsável
   - Um técnico pode ser responsável por múltiplos atendimentos
   - Implementado pela chave estrangeira `responsavel_id` em ATENDIMENTO

8. **ATENDIMENTO - STATUS_ATENDIMENTO** (N:1)
   - Cada atendimento tem um status atual
   - Um status pode ser aplicado a múltiplos atendimentos
   - Implementado pela chave estrangeira `status_id` em ATENDIMENTO

9. **ATENDIMENTO - HISTORICO_ATENDIMENTO** (1:N)
   - Um atendimento tem múltiplos registros históricos
   - Cada registro histórico pertence a um único atendimento
   - Implementado pela chave estrangeira `atendimento_id` em HISTORICO_ATENDIMENTO

10. **ATENDIMENTO - TAREFA** (1:N)
    - Um atendimento pode ter múltiplas tarefas associadas
    - Cada tarefa pertence a exatamente um atendimento
    - Implementado pela chave estrangeira `atendimento_id` em TAREFA

11. **MATERIAL - CATEGORIA_MATERIAL** (N:1)
    - Cada material pertence a uma categoria específica
    - Uma categoria pode conter múltiplos materiais
    - Implementado pela chave estrangeira `categoria_id` em MATERIAL

12. **MATERIAL - HISTORICO_MATERIAL** (1:N)
    - Um material tem múltiplos registros históricos
    - Cada registro histórico pertence a um único material
    - Implementado pela chave estrangeira `material_id` em HISTORICO_MATERIAL

13. **RELATORIO - CONFIG_RELATORIO** (1:N)
    - Um modelo de relatório pode ter múltiplas configurações
    - Cada configuração está associada a um modelo específico
    - Implementado pela chave estrangeira `relatorio_id` em CONFIG_RELATORIO

## Índices

### Índices Principais

1. **idx_usuario_email**: Índice para busca rápida de usuários por email
   - Tabela: USUARIO
   - Coluna(s): email

2. **idx_usuario_perfil**: Índice para agrupamento de usuários por perfil
   - Tabela: USUARIO
   - Coluna(s): perfil_id

3. **idx_atendimento_protocolo**: Índice para busca rápida por protocolo
   - Tabela: ATENDIMENTO
   - Coluna(s): protocolo

4. **idx_atendimento_unidade**: Índice para filtragem por unidade
   - Tabela: ATENDIMENTO
   - Coluna(s): unidade_id

5. **idx_atendimento_responsavel**: Índice para consulta de atendimentos por técnico responsável
   - Tabela: ATENDIMENTO
   - Coluna(s): responsavel_id

6. **idx_atendimento_status**: Índice para filtragem por status
   - Tabela: ATENDIMENTO
   - Coluna(s): status_id

7. **idx_atendimento_composto**: Índice composto para relatórios frequentes
   - Tabela: ATENDIMENTO
   - Coluna(s): unidade_id, status_id, prioridade_id

8. **idx_historico_atendimento**: Índice para buscar histórico por atendimento
   - Tabela: HISTORICO_ATENDIMENTO
   - Coluna(s): atendimento_id

9. **idx_tarefa_responsavel_status**: Índice para filtragem de tarefas por responsável e status
   - Tabela: TAREFA
   - Coluna(s): responsavel_id, status

10. **idx_material_titulo_trgm**: Índice para busca por similaridade em títulos
    - Tabela: MATERIAL
    - Coluna(s): titulo
    - Tipo: GIN com operador trgm

11. **idx_material_tags**: Índice para busca em array de tags
    - Tabela: MATERIAL
    - Coluna(s): tags
    - Tipo: GIN

12. **idx_material_texto_busca**: Índice para busca textual completa
    - Tabela: MATERIAL
    - Coluna(s): texto_busca
    - Tipo: GIN

13. **idx_notificacao_lida**: Índice para filtragem de notificações não lidas por usuário
    - Tabela: NOTIFICACAO
    - Coluna(s): destinatario_id, lida

14. **idx_log_data**: Índice para consulta de logs por período
    - Tabela: LOG
    - Coluna(s): data_hora

## Referências

Este dicionário de dados foi elaborado com base nos seguintes documentos do projeto:

1. Especificação de Requisitos de Software (SRS)
2. Modelo de Domínio
3. Diagrama Entidade-Relacionamento
4. Modelo Lógico e Físico de Banco de Dados
5. Diagramas de Fluxo de Dados