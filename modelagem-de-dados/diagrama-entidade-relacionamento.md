# Diagrama Entidade-Relacionamento
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação Utilizada](#notação-utilizada)
- [Diagrama Entidade-Relacionamento Conceitual](#diagrama-entidade-relacionamento-conceitual)
- [Diagrama Entidade-Relacionamento Lógico](#diagrama-entidade-relacionamento-lógico)
- [Descrição das Entidades](#descrição-das-entidades)
  - [Entidades Principais](#entidades-principais)
  - [Entidades Associativas](#entidades-associativas)
  - [Entidades de Catálogo](#entidades-de-catálogo)
- [Descrição dos Relacionamentos](#descrição-dos-relacionamentos)
- [Dicionário de Dados](#dicionário-de-dados)
- [Considerações de Implementação](#considerações-de-implementação)
  - [Estratégias de Persistência](#estratégias-de-persistência)
  - [Normalização](#normalização)
  - [Índices Recomendados](#índices-recomendados)
  - [Considerações de Integridade](#considerações-de-integridade)

## Introdução

Este documento apresenta o Diagrama Entidade-Relacionamento (DER) para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. O DER é uma ferramenta essencial no projeto de banco de dados, representando entidades no domínio do problema e os relacionamentos entre elas.

Enquanto os diagramas de classes e objetos descrevem a estrutura orientada a objetos da aplicação, o DER foca especificamente na estrutura do banco de dados relacional que suportará o sistema. Este documento complementa os diagramas já apresentados, especificando como os conceitos de domínio serão persistidos em um modelo de dados relacional.

O DER é apresentado em duas versões:
1. **Conceitual**: Visão de alto nível do modelo de dados, focando nas entidades, seus relacionamentos e cardinaiidades
2. **Lógico**: Detalhamento do modelo conceitual, incluindo atributos, chaves e tipos de dados

## Notação Utilizada

O diagrama segue a notação Chen ampliada, utilizando os seguintes elementos:

- **Retângulos**: Representam entidades (tabelas)
- **Losangos**: Representam relacionamentos entre entidades
- **Elipses**: Representam atributos
- **Linhas**: Conectam entidades aos seus relacionamentos e atributos
- **Cardinálidade**: Indicada próximo às linhas de relacionamento
  - **1:1**: Um para um
  - **1:N**: Um para muitos
  - **N:M**: Muitos para muitos
- **Sublinhas**: Indicam chaves primárias
- **Linhas pontilhadas**: Indicam atributos derivados ou calculados
- **Retângulos com borda dupla**: Indicam entidades fracas
- **Losangos com borda dupla**: Indicam relacionamentos identificadores

## Diagrama Entidade-Relacionamento Conceitual

```
+-------------+       registra       +--------------+      possui      +------------------+
|             |---------------------<|              |>-----------------|                  |
|   USUARIO   |       1     N        | ATENDIMENTO  |     1     N      |      TAREFA      |
|             |                      |              |                  |                  |
+-------------+                      +--------------+                  +------------------+
       |                                    |                                   |
       | possui                             | classifica                        | atribuída a
       | 1                                  | N                                 | N
       |                                    |                                   |
       V                                    V                                   |
+-------------+                      +--------------+                           |
|             |                      |              |                           |
|   PERFIL    |                      |    TIPO      |                           |
|             |                      | NECESSIDADE  |                           |
+-------------+                      |              |                           |
                                     +--------------+                           |
                                                                                |
+-------------+      pertence       +--------------+                            |
|             |---------------------<|              |                           |
|   UNIDADE   |       1     N        | COORDENADOR  |>-------------------------+
|             |                      |              |            1
+------^------+                      +--------------+
       |
       | vinculada a
       | N
       |
+-------------+       registra       +--------------+
|             |---------------------<|              |
|  MATERIAL   |       1     N        |  HISTORIO    |
|             |                      |  MATERIAL    |
+-------------+                      +--------------+
       |
       | possui
       | N
       |
+-------------+
|             |
|  CATEGORIA  |
|  MATERIAL   |
|             |
+-------------+

+-------------+       possui        +--------------+
|             |---------------------<|              |
|  RELATORIO  |       1     N        |   CONFIG     |
|             |                      |  RELATORIO   |
+-------------+                      +--------------+

+-------------+       registra       +--------------+
|             |---------------------<|              |
| ATENDIMENTO |       1     N        |   HISTORICO  |
|             |                      | ATENDIMENTO  |
+------^------+                      +--------------+
       |
       | solicita
       | 1
       |
+-------------+
|             |
| PROFISSIONAL|
|ESPECIALIZADO|
|             |
+-------------+
```

## Diagrama Entidade-Relacionamento Lógico

```
+------------------------+                  +------------------------+
| USUARIO                |                  | PERFIL                 |
+------------------------+                  +------------------------+
| PK usuario_id          |<-------------+   | PK perfil_id           |
| nome                   |              |   | nome                   |
| email                  |              |   | descricao              |
| senha_hash             |              |   | permissoes             |
| FK perfil_id           |--------------+   +------------------------+
| data_cadastro          |
| ultimo_acesso          |
| status                 |
+------------------------+
         |
         |
         +-----------------+
                          |
+------------------------+|                 +------------------------+
| COORDENADOR            ||                 | UNIDADE                |
+------------------------+|                 +------------------------+
| PK usuario_id          |<------------+    | PK unidade_id          |
| FK unidade_id          |-------------+---->tipo (Etec/Fatec)       |
+------------------------+                  | nome                   |
                                           | cidade                  |
                                           | endereco                |
                                           | telefone                |
                                           | email                   |
                                           +------------------------+
+------------------------+                  +------------------------+
| TIPO_NECESSIDADE       |                  | ATENDIMENTO            |
+------------------------+                  +------------------------+
| PK tipo_necessidade_id |<------------+    | PK atendimento_id      |
| nome                   |             |    | protocolo              |
| descricao              |             |    | FK tipo_necessidade_id |-+
| recomendacoes          |             +----| FK unidade_id          | |
+------------------------+                  | FK responsavel_id      | |
                                           | data_abertura          | |
                                           | ultima_atualizacao     | |
                                           | status                 | |
                                           | descricao              | |
                                           | prioridade             | |
                                           +------------------------+ |
                                                    |                 |
                                                    |                 |
                                                    v                 |
+------------------------+                  +------------------------+|
| HISTORICO_ATENDIMENTO  |                  | TAREFA                 ||
+------------------------+                  +------------------------+|
| PK historico_id        |                  | PK tarefa_id           ||
| FK atendimento_id      |<-----------------| FK atendimento_id      |+
| FK usuario_id          |                  | FK responsavel_id      |--+
| data_hora              |                  | titulo                 |  |
| acao_realizada         |                  | descricao              |  |
| estado_anterior        |                  | data_criacao           |  |
| estado_novo            |                  | prazo                  |  |
| observacoes            |                  | status                 |  |
+------------------------+                  | prioridade             |  |
                                           +------------------------+  |
                                                    ^                  |
                                                    |                  |
                                                    |                  |
+------------------------+                          |                  |
| TECNICO                |                          |                  |
+------------------------+                          |                  |
| PK usuario_id          |<-------------------------+------------------+
| especialidade          |
| disponibilidade        |
+------------------------+

+------------------------+                  +------------------------+
| MATERIAL               |                  | CATEGORIA_MATERIAL     |
+------------------------+                  +------------------------+
| PK material_id         |                  | PK categoria_id        |
| titulo                 |                  | nome                   |
| descricao              |<--------+        | descricao              |
| FK categoria_id        |---------|------->+------------------------+
| FK autor_id            |         |
| formato                |         |        +------------------------+
| data_inclusao          |         |        | HISTORICO_MATERIAL     |
| versao                 |         |        +------------------------+
| arquivo_path           |         |        | PK historico_id        |
| tags                   |         |        | FK material_id         |<---+
| estado                 |         |        | FK usuario_id          |    |
+------------------------+         |        | data_hora              |    |
         |                         |        | acao                   |    |
         |                         +--------| versao_anterior        |    |
         |                                  | versao_nova            |    |
         +----------------------------------->observacoes            |    |
                                           +------------------------+    |
                                                                         |
+------------------------+                  +------------------------+   |
| RELATORIO              |                  | CONFIG_RELATORIO       |   |
+------------------------+                  +------------------------+   |
| PK relatorio_id        |<------------+    | PK config_id           |   |
| titulo                 |             |    | FK relatorio_id        |   |
| descricao              |             +----| parametros             |   |
| tipo                   |                  | data_ultima_execucao   |   |
| FK criador_id          |----------------->| agendamento            |   |
| campos_disponiveis     |                  +------------------------+   |
| predefinido            |                                              |
+------------------------+                                              |
                                                                        |
+------------------------+                                              |
| NOTIFICACAO            |                                              |
+------------------------+                                              |
| PK notificacao_id      |                                              |
| FK destinatario_id     |                                              |
| tipo                   |                                              |
| titulo                 |                                              |
| conteudo               |                                              |
| data_envio             |                                              |
| lida                   |                                              |
+------------------------+                                              |
                                                                        |
+------------------------+                                              |
| LOG                    |                                              |
+------------------------+                                              |
| PK log_id              |                                              |
| data_hora              |                                              |
| FK usuario_id          |----------------------------------------------+
| acao                   |
| entidade               |
| entidade_id            |
| detalhes               |
+------------------------+
```

## Descrição das Entidades

### Entidades Principais

1. **USUARIO**
   - Representa todos os usuários do sistema
   - Contém informações de autenticação e perfil básico
   - Tipos de usuário (como Administrador, Gestor, Técnico e Coordenador) são implementados através da associação com PERFIL e/ou tabelas de especialização

2. **ATENDIMENTO**
   - Entidade central que representa casos atendidos pela Assessoria
   - Armazena informações sobre o progresso, responsáveis e prazos
   - Mantém relação com a unidade solicitante e o tipo de necessidade

3. **TAREFA**
   - Representa atividades específicas relacionadas a um atendimento
   - Registra responsável, prazos e status
   - Permite acompanhamento detalhado das ações necessárias

4. **MATERIAL**
   - Representa documentos e recursos do repositório de materiais
   - Contém metadados sobre conteúdo, autoria e versão
   - O conteúdo físico é armazenado no sistema de arquivos e referenciado pelo caminho

5. **UNIDADE**
   - Representa as unidades educacionais do CPS (Etecs e Fatecs)
   - Armazena informações de contato e localização
   - Base para relatórios geográficos e estatísticos

### Entidades Associativas

1. **HISTORICO_ATENDIMENTO**
   - Registra todas as alterações ocorridas em um atendimento
   - Implementa o padrão de auditoria para rastreabilidade
   - Permite reconstruir a linha do tempo de cada atendimento

2. **HISTORICO_MATERIAL**
   - Registra alterações em materiais do repositório
   - Permite controle de versão e histórico de modificações
   - Base para recuperação de versões anteriores

3. **CONFIG_RELATORIO**
   - Armazena configurações específicas de relatórios
   - Permite personalização e agendamento de relatórios recorrentes
   - Associa parâmetros de execução a modelos de relatório

4. **LOG**
   - Registra operações realizadas no sistema para fins de auditoria
   - Armazena informações sobre o usuário, ação e entidade afetada
   - Utilizada para monitoramento de segurança e conformidade

### Entidades de Catálogo

1. **PERFIL**
   - Define tipos de usuário e suas permissões
   - Implementa o modelo RBAC (Role-Based Access Control)
   - Permite configuração flexível de acessos

2. **TIPO_NECESSIDADE**
   - Categoriza os tipos de necessidades educacionais especiais
   - Fornece recomendações padronizadas para cada tipo
   - Base para relatórios estatísticos por categoria de atendimento

3. **CATEGORIA_MATERIAL**
   - Organiza materiais do repositório em categorias
   - Facilita a busca e organização do conhecimento
   - Permite relatórios sobre tipos de materiais disponíveis

4. **RELATORIO**
   - Define modelos de relatório disponíveis no sistema
   - Especifica campos e filtros disponíveis
   - Base para geração de relatórios personalizados

## Descrição dos Relacionamentos

1. **USUARIO - ATENDIMENTO** (1:N)
   - Um usuário pode registrar/ser responsável por múltiplos atendimentos
   - Cada atendimento tem exatamente um usuário responsável atual
   - Registra quem está encarregado de dar andamento ao caso

2. **USUARIO - PERFIL** (N:1)
   - Cada usuário pertence a um perfil específico
   - Um perfil pode ser atribuído a múltiplos usuários
   - Define as permissões e o papel do usuário no sistema

3. **ATENDIMENTO - TIPO_NECESSIDADE** (N:1)
   - Cada atendimento é classificado em um tipo de necessidade
   - Um tipo de necessidade pode ser aplicado a múltiplos atendimentos
   - Base para categorização e aplicação de protocolos específicos

4. **ATENDIMENTO - TAREFA** (1:N)
   - Um atendimento pode ter múltiplas tarefas associadas
   - Cada tarefa pertence a exatamente um atendimento
   - Permite decompor um caso em ações específicas e atribuíveis

5. **COORDENADOR - UNIDADE** (N:1)
   - Cada coordenador está associado a uma unidade específica
   - Uma unidade pode ter múltiplos coordenadores
   - Estabelece o vínculo organizacional do coordenador

6. **ATENDIMENTO - UNIDADE** (N:1)
   - Cada atendimento está associado a uma unidade específica
   - Uma unidade pode ter múltiplos atendimentos
   - Identifica a origem do caso e permite filtragem geográfica

7. **MATERIAL - CATEGORIA_MATERIAL** (N:1)
   - Cada material pertence a uma categoria específica
   - Uma categoria pode conter múltiplos materiais
   - Permite organização hierárquica do repositório de conhecimento

8. **MATERIAL - HISTORICO_MATERIAL** (1:N)
   - Um material tem múltiplos registros históricos
   - Cada registro histórico pertence a um único material
   - Implementa o controle de versão e auditoria de alterações

9. **ATENDIMENTO - HISTORICO_ATENDIMENTO** (1:N)
   - Um atendimento tem múltiplos registros históricos
   - Cada registro histórico pertence a um único atendimento
   - Rastreia alterações e evolução do atendimento

10. **RELATORIO - CONFIG_RELATORIO** (1:N)
    - Um modelo de relatório pode ter múltiplas configurações
    - Cada configuração está associada a um modelo específico
    - Permite personalizar e salvar variações de relatórios

## Dicionário de Dados

### USUARIO

| Atributo      | Tipo       | Descrição                            | Restrições             |
|---------------|------------|--------------------------------------|------------------------|
| usuario_id    | SERIAL     | Identificador único do usuário       | PK, NOT NULL           |
| nome          | VARCHAR    | Nome completo do usuário             | NOT NULL               |
| email         | VARCHAR    | Email institucional para login       | NOT NULL, UNIQUE       |
| senha_hash    | VARCHAR    | Hash da senha para autenticação      | NOT NULL               |
| perfil_id     | INTEGER    | Referência ao perfil do usuário      | FK, NOT NULL           |
| data_cadastro | TIMESTAMP  | Data/hora do cadastro no sistema     | NOT NULL, DEFAULT NOW  |
| ultimo_acesso | TIMESTAMP  | Data/hora do último acesso           | NULL                   |
| status        | VARCHAR    | Status do usuário (Ativo/Inativo)    | NOT NULL, DEFAULT 'A'  |

### PERFIL

| Atributo    | Tipo       | Descrição                             | Restrições        |
|-------------|------------|-----------------------------------------|-------------------|
| perfil_id   | SERIAL     | Identificador único do perfil           | PK, NOT NULL      |
| nome        | VARCHAR    | Nome do perfil (Admin, Gestor, etc.)    | NOT NULL, UNIQUE  |
| descricao   | TEXT       | Descrição das responsabilidades         | NOT NULL          |
| permissoes  | JSONB      | Matriz de permissões em formato JSON    | NOT NULL          |

### ATENDIMENTO

| Atributo             | Tipo       | Descrição                             | Restrições        |
|----------------------|------------|-----------------------------------------|-------------------|
| atendimento_id       | SERIAL     | Identificador único do atendimento      | PK, NOT NULL      |
| protocolo            | VARCHAR    | Código de protocolo para referência     | NOT NULL, UNIQUE  |
| tipo_necessidade_id  | INTEGER    | Tipo de necessidade educacional         | FK, NOT NULL      |
| unidade_id           | INTEGER    | Unidade solicitante                     | FK, NOT NULL      |
| responsavel_id       | INTEGER    | Técnico responsável pelo atendimento    | FK, NOT NULL      |
| data_abertura        | TIMESTAMP  | Data/hora de registro do atendimento    | NOT NULL          |
| ultima_atualizacao   | TIMESTAMP  | Data/hora da última atualização         | NOT NULL          |
| status               | VARCHAR    | Estado atual do atendimento             | NOT NULL          |
| descricao            | TEXT       | Detalhamento da necessidade             | NOT NULL          |
| prioridade           | VARCHAR    | Nível de prioridade                     | NOT NULL          |

### TAREFA

| Atributo        | Tipo       | Descrição                              | Restrições           |
|-----------------|------------|----------------------------------------|----------------------|
| tarefa_id       | SERIAL     | Identificador único da tarefa          | PK, NOT NULL         |
| atendimento_id  | INTEGER    | Atendimento relacionado                | FK, NOT NULL         |
| responsavel_id  | INTEGER    | Técnico responsável pela tarefa        | FK, NOT NULL         |
| titulo          | VARCHAR    | Título descritivo da tarefa            | NOT NULL             |
| descricao       | TEXT       | Detalhamento da atividade              | NOT NULL             |
| data_criacao    | TIMESTAMP  | Data/hora de criação                   | NOT NULL, DEFAULT NOW |
| prazo           | TIMESTAMP  | Data/hora limite para conclusão        | NULL                 |
| status          | VARCHAR    | Estado atual da tarefa                 | NOT NULL             |
| prioridade      | VARCHAR    | Nível de prioridade                    | NOT NULL             |

### MATERIAL

| Atributo      | Tipo       | Descrição                              | Restrições        |
|---------------|------------|----------------------------------------|-------------------|
| material_id   | SERIAL     | Identificador único do material        | PK, NOT NULL      |
| titulo        | VARCHAR    | Título do material                     | NOT NULL          |
| descricao     | TEXT       | Descrição do conteúdo                  | NOT NULL          |
| categoria_id  | INTEGER    | Categoria do material                  | FK, NOT NULL      |
| autor_id      | INTEGER    | Usuário que incluiu o material         | FK, NOT NULL      |
| formato       | VARCHAR    | Formato do arquivo (PDF, DOC, etc.)    | NOT NULL          |
| data_inclusao | TIMESTAMP  | Data/hora da inclusão                  | NOT NULL          |
| versao        | INTEGER    | Versão atual do documento              | NOT NULL, DEFAULT 1 |
| arquivo_path  | VARCHAR    | Caminho para o arquivo no storage      | NOT NULL          |
| tags          | VARCHAR[]  | Array de palavras-chave                | NULL              |
| estado        | VARCHAR    | Estado (Rascunho, Publicado, etc.)     | NOT NULL          |

## Considerações de Implementação

### Estratégias de Persistência

1. **Sistema Gerenciador de Banco de Dados**:
   - Recomenda-se a utilização do PostgreSQL como SGBD pela sua robustez, capacidade de lidar com tipos complexos (arrays, JSON), e extensibilidade.
   - Versão recomendada: PostgreSQL 14 ou superior.

2. **Extensões Recomendadas**:
   - **pg_trgm**: Para busca por similaridade em campos textuais (útil para busca de materiais).
   - **pgcrypto**: Para funções criptográficas adicionais.
   - **temporal_tables**: Para facilitar o gerenciamento de histórico (opcional).

3. **Armazenamento de Arquivos**:
   - Arquivos físicos do repositório de materiais devem ser armazenados fora do banco de dados, em um sistema de arquivos estruturado ou solução de armazenamento específica.
   - Apenas os metadados e caminhos são armazenados no banco de dados relacional.

4. **Estratégia para Objetos Grandes**:
   - Para arquivos pequenos ou binários que precisam ser armazenados diretamente no banco, utilizar tipo BYTEA.
   - Considerar particionamento para tabelas que crescem rapidamente, como LOG e HISTORICO_ATENDIMENTO.

### Normalização

1. **Nível de Normalização**:
   - O modelo está predominantemente na 3ª Forma Normal (3FN).
   - Algumas desnormalizações controladas podem ser implementadas para otimização de desempenho em consultas frequentes.

2. **Abordagem para Valores Enumerados**:
   - Valores enumerados frequentemente consultados (como status, prioridade) podem ser implementados como tabelas de domínio separadas ao invés de colunas VARCHAR com restrições.
   - Exemplo: Criar tabela STATUS_ATENDIMENTO ao invés de coluna status VARCHAR em ATENDIMENTO.

3. **Tratamento de Dados Dinâmicos**:
   - Utilizar JSONB para estruturas de dados flexíveis ou de alta variabilidade (como parâmetros de relatórios).
   - Considerar tabelas EAV (Entity-Attribute-Value) para atributos altamente variáveis que precisam ser consultáveis.

### Índices Recomendados

1. **Índices Primários**:
   - Chaves primárias de todas as tabelas (criados automaticamente pelo SGBD).

2. **Índices Estrangeiros**:
   - Todas as chaves estrangeiras, para otimizar JOINs.

3. **Índices de Busca**:
   - ATENDIMENTO(protocolo): para busca rápida por protocolo.
   - ATENDIMENTO(status): para filtragem por status.
   - ATENDIMENTO(unidade_id, status): para consultas filtradas por unidade e status.
   - MATERIAL(titulo, tags): para busca textual no repositório.
   - TAREFA(responsavel_id, status): para visualização de tarefas por responsável.

4. **Índices Especializados**:
   - Índice GIN para o campo tags em MATERIAL (para busca eficiente em arrays).
   - Índice GIN para o campo permissoes em PERFIL (para consultas em JSON).
   - Índice de texto completo (ts_vector) para campos de busca textual em materiais.

### Considerações de Integridade

1. **Integridade Referencial**:
   - Implementar ON DELETE e ON UPDATE apropriados para chaves estrangeiras.
   - Para registros críticos, usar RESTRICT ou NO ACTION para evitar exclusão acidental.
   - Para relações hierárquicas, considerar CASCADE onde apropriado (ex: exclusão de um atendimento deve excluir suas tarefas).

2. **Soft Delete**:
   - Implementar exclusão lógica para entidades principais como ATENDIMENTO, MATERIAL e USUARIO.
   - Adicionar coluna "deleted_at" às tabelas que suportam exclusão lógica.
   - Criar views que filtram automaticamente registros excluídos logicamente.

3. **Constraints Avançadas**:
   - Utilizar CHECK constraints para validação de dados (ex: datas de prazo devem ser futuras).
   - Implementar regras de negócio complexas via triggers quando não for possível com constraints simples.
   - Considerar o uso de EXCLUSION constraints para regras como "um técnico não pode ter tarefas simultâneas com mesma prioridade alta".

4. **Auditoria e Rastreabilidade**:
   - Além das tabelas de histórico, considerar triggers para registro automático de alterações em tabelas críticas.
   - Implementar mecanismo para registro de sessões de usuário e operações entre transações.