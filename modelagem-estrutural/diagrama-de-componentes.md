# Diagrama de Componentes
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação UML Utilizada](#notação-uml-utilizada)
- [Visão Geral da Arquitetura](#visão-geral-da-arquitetura)
- [Diagrama de Componentes](#diagrama-de-componentes-1)
- [Descrição dos Componentes](#descrição-dos-componentes)
  - [Componentes de Interface do Usuário](#componentes-de-interface-do-usuário)
  - [Componentes de Lógica de Negócio](#componentes-de-lógica-de-negócio)
  - [Componentes de Acesso a Dados](#componentes-de-acesso-a-dados)
  - [Componentes de Serviços Transversais](#componentes-de-serviços-transversais)
- [Relacionamentos e Dependências](#relacionamentos-e-dependências)
- [Interfaces Principais](#interfaces-principais)
- [Mapeamento com Requisitos](#mapeamento-com-requisitos)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Este documento apresenta o diagrama de componentes para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. O diagrama de componentes é uma representação de alto nível da arquitetura do software, mostrando como o sistema é dividido em componentes individuais e as interfaces através das quais eles interagem.

Enquanto o diagrama de classes detalha a estrutura estática em termos de classes e seus relacionamentos, e os diagramas de atividades e BPMN representam o comportamento dinâmico dos processos, o diagrama de componentes proporciona uma visão da organização lógica e física do sistema. Este diagrama é particularmente útil para arquitetos de software, desenvolvedores e integradores, pois facilita o entendimento da estrutura modular do sistema e suas dependências.

## Notação UML Utilizada

O diagrama de componentes segue a notação UML (Unified Modeling Language) padrão, incluindo os seguintes elementos:

- **Componentes**: Representados por retângulos com o símbolo de componente no canto superior direito (retângulo com dois retângulos menores protuberantes)
- **Interfaces**: Representadas pelo símbolo "lollipop" (círculo no final de uma linha conectada a um componente)
- **Dependências**: Representadas por linhas tracejadas com setas
- **Portas**: Pequenos quadrados nas bordas de componentes que representam pontos de interação específicos
- **Conectores**: Linhas sólidas que conectam componentes e interfaces
- **Pacotes**: Retângulos com abas que agrupam componentes relacionados
- **Estereótipos**: Texto entre «» que especifica o tipo de componente (ex: «service», «repository»)

## Visão Geral da Arquitetura

O Sistema de Gestão para a Assessoria de Inclusão segue uma arquitetura em camadas com separação clara de responsabilidades. A arquitetura é composta por quatro camadas principais:

1. **Camada de Apresentação**: Interfaces de usuário e componentes de visualização
2. **Camada de Lógica de Negócio**: Serviços que implementam as regras de negócio e fluxos de trabalho
3. **Camada de Acesso a Dados**: Componentes para persistência e recuperação de dados
4. **Camada de Serviços Transversais**: Componentes que fornecem funcionalidades utilizadas por todas as outras camadas

A arquitetura adota o padrão de design MVC (Model-View-Controller) para a camada de apresentação e o padrão Repository para acesso a dados, garantindo separação de responsabilidades e facilitando a manutenção e evolução do sistema.

## Diagrama de Componentes

```
+---------------------------------------------------------------------+
|                          <<system>>                                  |
|           Sistema de Gestão da Assessoria de Inclusão                |
+---------------------------------------------------------------------+

+-------------------------+  +-------------------------+  +-------------------------+
|       <<subsystem>>     |  |       <<subsystem>>     |  |       <<subsystem>>     |
|    Interface do Usuário |  |   Lógica de Negócio     |  |     Acesso a Dados      |
+-------------------------+  +-------------------------+  +-------------------------+

+-----------------------------------------------------------------------------+
|                        <<subsystem>>                                         |
|                      Serviços Transversais                                   |
+-----------------------------------------------------------------------------+

//Detalhamento da Interface do Usuário
+-------------------------+
|  Interface do Usuário   |
+-------------------------+
|                         |
|  +-----------------+    |
|  |   <<component>> |    |
|  |   Dashboard UI  |    |
|  +-----------------+    |
|                         |
|  +-----------------+    |
|  |   <<component>> |    |
|  | Atendimentos UI |    |
|  +-----------------+    |
|                         |
|  +-----------------+    |
|  |   <<component>> |    |
|  |   Tarefas UI    |    |
|  +-----------------+    |
|                         |
|  +-----------------+    |
|  |   <<component>> |    |
|  | Repositório UI  |    |
|  +-----------------+    |
|                         |
|  +-----------------+    |
|  |   <<component>> |    |
|  | Relatórios UI   |    |
|  +-----------------+    |
|                         |
|  +-----------------+    |
|  |   <<component>> |    |
|  | Admin UI        |    |
|  +-----------------+    |
+-------------------------+

//Detalhamento da Lógica de Negócio
+------------------------------------------------------+
|                  Lógica de Negócio                   |
+------------------------------------------------------+
|                                                      |
|  +-------------------+      +-------------------+    |
|  |    <<service>>    |      |    <<service>>    |    |
|  | AtendimentoService|<---->| NotificacaoService|    |
|  +-------------------+      +-------------------+    |
|            ^                                         |
|            |                                         |
|            v                                         |
|  +-------------------+      +-------------------+    |
|  |    <<service>>    |      |    <<service>>    |    |
|  |   TarefaService   |<---->|  RelatorioService |    |
|  +-------------------+      +-------------------+    |
|                                      ^               |
|  +-------------------+               |               |
|  |    <<service>>    |               |               |
|  | RepositorioService|<--------------+               |
|  +-------------------+                               |
|                                                      |
|  +-------------------+      +-------------------+    |
|  |   <<validator>>   |      |    <<factory>>    |    |
|  |BusinessRuleValidator|    |DocumentoGenerator |    |
|  +-------------------+      +-------------------+    |
+------------------------------------------------------+

//Detalhamento do Acesso a Dados
+------------------------------------------------------+
|                    Acesso a Dados                    |
+------------------------------------------------------+
|                                                      |
|  +-------------------+      +-------------------+    |
|  |   <<repository>>  |      |   <<repository>>  |    |
|  | AtendimentoRepo   |      |    TarefaRepo     |    |
|  +-------------------+      +-------------------+    |
|                                                      |
|  +-------------------+      +-------------------+    |
|  |   <<repository>>  |      |   <<repository>>  |    |
|  |    UsuarioRepo    |      |   MaterialRepo    |    |
|  +-------------------+      +-------------------+    |
|                                                      |
|  +-------------------+      +-------------------+    |
|  |   <<repository>>  |      |     <<dao>>       |    |
|  |   RelatorioRepo   |      |   QueryExecutor   |    |
|  +-------------------+      +-------------------+    |
|                                                      |
|  +--------------------------------+                  |
|  |         <<adapter>>            |                  |
|  |    DatabaseConnectionManager   |                  |
|  +--------------------------------+                  |
+------------------------------------------------------+

//Detalhamento dos Serviços Transversais
+-----------------------------------------------------------------------------+
|                           Serviços Transversais                             |
+-----------------------------------------------------------------------------+
|                                                                             |
|  +-------------------+      +-------------------+     +------------------+  |
|  |   <<component>>   |      |   <<component>>   |     |  <<component>>   |  |
|  | SegurancaService  |      |   LoggingService  |     | ConfigService    |  |
|  +-------------------+      +-------------------+     +------------------+  |
|                                                                             |
|  +-------------------+      +-------------------+     +------------------+  |
|  |   <<component>>   |      |   <<component>>   |     |  <<component>>   |  |
|  | NotificacaoManager|      | AgendadorTarefas  |     | CacheManager     |  |
|  +-------------------+      +-------------------+     +------------------+  |
|                                                                             |
+-----------------------------------------------------------------------------+
```

## Descrição dos Componentes

### Componentes de Interface do Usuário

1. **Dashboard UI**
   - **Responsabilidade**: Apresenta visões consolidadas e painéis de controle personalizados para cada perfil de usuário
   - **Interfaces fornecidas**: IDashboardView
   - **Interfaces requeridas**: IDashboardDataProvider, INotificacaoListener

2. **Atendimentos UI**
   - **Responsabilidade**: Interface para registro, consulta e acompanhamento de atendimentos
   - **Interfaces fornecidas**: IAtendimentoView
   - **Interfaces requeridas**: IAtendimentoService, INotificacaoListener

3. **Tarefas UI**
   - **Responsabilidade**: Interface para gerenciamento de tarefas relacionadas a atendimentos
   - **Interfaces fornecidas**: ITarefaView
   - **Interfaces requeridas**: ITarefaService, INotificacaoListener

4. **Repositório UI**
   - **Responsabilidade**: Interface para acesso, busca e gerenciamento de materiais no repositório
   - **Interfaces fornecidas**: IRepositorioView
   - **Interfaces requeridas**: IRepositorioService, IUploadManager

5. **Relatórios UI**
   - **Responsabilidade**: Interface para configuração, geração e visualização de relatórios
   - **Interfaces fornecidas**: IRelatorioView
   - **Interfaces requeridas**: IRelatorioService, IExportManager

6. **Admin UI**
   - **Responsabilidade**: Interface para administração do sistema, incluindo gestão de usuários e configurações
   - **Interfaces fornecidas**: IAdminView
   - **Interfaces requeridas**: IUsuarioService, IConfigService

### Componentes de Lógica de Negócio

1. **AtendimentoService**
   - **Responsabilidade**: Implementa a lógica de negócio relacionada ao fluxo de atendimentos
   - **Interfaces fornecidas**: IAtendimentoService
   - **Interfaces requeridas**: IAtendimentoRepository, INotificacaoService, ITarefaService

2. **TarefaService**
   - **Responsabilidade**: Gerencia o ciclo de vida das tarefas e sua execução
   - **Interfaces fornecidas**: ITarefaService
   - **Interfaces requeridas**: ITarefaRepository, INotificacaoService

3. **RepositorioService**
   - **Responsabilidade**: Gerencia o armazenamento, categorização e acesso a materiais no repositório
   - **Interfaces fornecidas**: IRepositorioService
   - **Interfaces requeridas**: IMaterialRepository, IFileStorageService

4. **RelatorioService**
   - **Responsabilidade**: Coordena a geração de relatórios e análises estatísticas
   - **Interfaces fornecidas**: IRelatorioService
   - **Interfaces requeridas**: IRelatorioRepository, IDocumentoGenerator

5. **NotificacaoService**
   - **Responsabilidade**: Gerencia o envio de notificações para usuários do sistema
   - **Interfaces fornecidas**: INotificacaoService
   - **Interfaces requeridas**: INotificacaoRepository, INotificacaoManager

6. **BusinessRuleValidator**
   - **Responsabilidade**: Centraliza a validação de regras de negócio complexas
   - **Interfaces fornecidas**: IBusinessRuleValidator
   - **Interfaces requeridas**: -

7. **DocumentoGenerator**
   - **Responsabilidade**: Cria documentos em diferentes formatos a partir de templates
   - **Interfaces fornecidas**: IDocumentoGenerator
   - **Interfaces requeridas**: ITemplateRepository

### Componentes de Acesso a Dados

1. **AtendimentoRepo**
   - **Responsabilidade**: Persiste e recupera dados de atendimentos
   - **Interfaces fornecidas**: IAtendimentoRepository
   - **Interfaces requeridas**: IDatabaseConnectionManager

2. **TarefaRepo**
   - **Responsabilidade**: Persiste e recupera dados de tarefas
   - **Interfaces fornecidas**: ITarefaRepository
   - **Interfaces requeridas**: IDatabaseConnectionManager

3. **UsuarioRepo**
   - **Responsabilidade**: Gerencia dados de usuários e perfis
   - **Interfaces fornecidas**: IUsuarioRepository
   - **Interfaces requeridas**: IDatabaseConnectionManager

4. **MaterialRepo**
   - **Responsabilidade**: Persiste e recupera metadados dos materiais do repositório
   - **Interfaces fornecidas**: IMaterialRepository
   - **Interfaces requeridas**: IDatabaseConnectionManager, IFileStorageService

5. **RelatorioRepo**
   - **Responsabilidade**: Armazena configurações e templates de relatórios
   - **Interfaces fornecidas**: IRelatorioRepository
   - **Interfaces requeridas**: IDatabaseConnectionManager

6. **QueryExecutor**
   - **Responsabilidade**: Executa consultas complexas para relatórios e análises
   - **Interfaces fornecidas**: IQueryExecutor
   - **Interfaces requeridas**: IDatabaseConnectionManager

7. **DatabaseConnectionManager**
   - **Responsabilidade**: Centraliza o acesso ao banco de dados
   - **Interfaces fornecidas**: IDatabaseConnectionManager
   - **Interfaces requeridas**: -

### Componentes de Serviços Transversais

1. **SegurancaService**
   - **Responsabilidade**: Gerencia autenticação, autorização e controle de acesso
   - **Interfaces fornecidas**: IAutenticacaoService, IAutorizacaoService
   - **Interfaces requeridas**: IUsuarioRepository, IConfigService

2. **LoggingService**
   - **Responsabilidade**: Fornece serviços de log para todas as camadas da aplicação
   - **Interfaces fornecidas**: ILogService
   - **Interfaces requeridas**: -

3. **ConfigService**
   - **Responsabilidade**: Gerencia configurações da aplicação
   - **Interfaces fornecidas**: IConfigService
   - **Interfaces requeridas**: -

4. **NotificacaoManager**
   - **Responsabilidade**: Implementa mecanismos de notificação (email, in-app, etc.)
   - **Interfaces fornecidas**: INotificacaoManager
   - **Interfaces requeridas**: IConfigService

5. **AgendadorTarefas**
   - **Responsabilidade**: Executa tarefas programadas e recorrentes
   - **Interfaces fornecidas**: IAgendadorTarefas
   - **Interfaces requeridas**: IConfigService

6. **CacheManager**
   - **Responsabilidade**: Gerencia o cache de dados para melhorar a performance
   - **Interfaces fornecidas**: ICacheManager
   - **Interfaces requeridas**: -

## Relacionamentos e Dependências

Os relacionamentos entre os componentes refletem o fluxo de dados e dependências do sistema:

1. **Fluxo de Atendimentos**:
   - `AtendimentosUI → AtendimentoService → AtendimentoRepo → DatabaseConnectionManager`
   - `AtendimentoService → NotificacaoService → NotificacaoManager`
   - `AtendimentoService → TarefaService`

2. **Fluxo de Tarefas**:
   - `TarefasUI → TarefaService → TarefaRepo → DatabaseConnectionManager`
   - `TarefaService → NotificacaoService`

3. **Fluxo de Relatórios**:
   - `RelatoriosUI → RelatorioService → RelatorioRepo → DatabaseConnectionManager`
   - `RelatorioService → QueryExecutor`
   - `RelatorioService → DocumentoGenerator`

4. **Fluxo de Repositório**:
   - `RepositorioUI → RepositorioService → MaterialRepo → DatabaseConnectionManager`

5. **Serviços Transversais**:
   - Todos os componentes dependem de `LoggingService`
   - Componentes com interfaces de usuário dependem de `SegurancaService`
   - Serviços que enviam notificações dependem de `NotificacaoManager`

## Interfaces Principais

1. **IAtendimentoService**
   ```
   + registrarAtendimento(atendimento: Atendimento): String
   + buscarAtendimento(protocolo: String): Atendimento
   + atualizarStatus(protocolo: String, status: StatusAtendimento): void
   + atribuirResponsavel(protocolo: String, tecnico: Usuario): void
   + listarAtendimentos(filtro: FiltroAtendimento): List<Atendimento>
   + finalizarAtendimento(protocolo: String, solucao: String): void
   ```

2. **ITarefaService**
   ```
   + criarTarefa(tarefa: Tarefa): Long
   + atribuirResponsavel(idTarefa: Long, tecnico: Usuario): void
   + atualizarProgresso(idTarefa: Long, percentual: int): void
   + adicionarComentario(idTarefa: Long, comentario: String): void
   + concluirTarefa(idTarefa: Long): void
   + listarTarefas(filtro: FiltroTarefa): List<Tarefa>
   ```

3. **IRepositorioService**
   ```
   + adicionarMaterial(material: Material, arquivo: File): Long
   + buscarMaterial(filtro: FiltroMaterial): List<Material>
   + atualizarMaterial(material: Material): void
   + alterarPermissoes(idMaterial: Long, permissoes: Set<Perfil>): void
   + obterVersaoAtual(idMaterial: Long): Material
   + listarVersoes(idMaterial: Long): List<Material>
   ```

4. **IRelatorioService**
   ```
   + listarRelatoriosPredefinidos(): List<Relatorio>
   + gerarRelatorio(idRelatorio: Long, parametros: Map<String, Object>): RelatorioGerado
   + agendarRelatorio(idRelatorio: Long, agendamento: String, parametros: Map<String, Object>): void
   + exportarRelatorio(relatorioGerado: RelatorioGerado, formato: FormatoExportacao): File
   ```

5. **INotificacaoService**
   ```
   + enviarNotificacao(notificacao: Notificacao): void
   + marcarComoLida(idNotificacao: Long): void
   + listarNotificacoesNaoLidas(usuario: Usuario): List<Notificacao>
   + configurarPreferencias(usuario: Usuario, preferencias: PreferenciasNotificacao): void
   ```

## Mapeamento com Requisitos

Este diagrama de componentes apoia a implementação dos requisitos funcionais identificados no documento de especificação de requisitos:

1. **RF-01: Controle de Acesso**
   - Componentes: SegurancaService, UsuarioRepo, Admin UI

2. **RF-02: Gestão de Atendimentos**
   - Componentes: AtendimentoService, AtendimentoRepo, Atendimentos UI, NotificacaoService

3. **RF-03: Organização do Fluxo de Trabalho**
   - Componentes: TarefaService, TarefaRepo, Tarefas UI, AgendadorTarefas

4. **RF-04: Geração de Relatórios**
   - Componentes: RelatorioService, RelatorioRepo, Relatórios UI, DocumentoGenerator

5. **RF-05: Gestão de Informações de Inclusão**
   - Componentes: RepositorioService, MaterialRepo, Repositório UI

## Considerações de Implementação

1. **Tecnologias Recomendadas**:
   - **Backend**: Spring Boot para implementar a arquitetura em camadas
   - **Frontend**: React ou Angular para interfaces de usuário responsivas
   - **Persistência**: JPA/Hibernate como ORM com PostgreSQL
   - **Segurança**: Spring Security para autenticação e controle de acesso
   - **Relatórios**: JasperReports ou similar para geração de documentos

2. **Padrões de Design**:
   - Implementar Repository Pattern para acesso a dados
   - Utilizar Dependency Injection para acoplamento fraco entre componentes
   - Aplicar Observer Pattern para notificações e eventos
   - Considerar Command Pattern para operações e transações
   - Implementar Strategy Pattern para relatórios e exportação em diferentes formatos

3. **Considerações de Escalabilidade**:
   - Desenhar componentes para funcionarem independentemente para facilitar escalabilidade horizontal
   - Implementar cache para melhorar desempenho de operações frequentes
   - Considerar processamento assíncrono para relatórios complexos e notificações

4. **Considerações de Manutenção**:
   - Utilizar interfaces bem definidas para facilitar substituição de implementações
   - Implementar testes automatizados para cada componente
   - Documentar as interfaces entre componentes detalhadamente
   - Considerar uso de contêineres para facilitar implantação e atualizações

5. **Monitoramento e Operação**:
   - Implementar logging extensivo em todos os componentes
   - Considerar ferramentas de APM (Application Performance Monitoring)
   - Definir estratégia de backup para dados do repositório de materiais
   - Implementar health checks para cada componente principal