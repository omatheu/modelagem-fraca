# Diagrama de Classes
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação UML Utilizada](#notação-uml-utilizada)
- [Diagrama de Classes](#diagrama-de-classes-1)
- [Descrição das Classes](#descrição-das-classes)
  - [Classes de Controle de Acesso](#classes-de-controle-de-acesso)
  - [Classes de Gestão de Atendimentos](#classes-de-gestão-de-atendimentos)
  - [Classes de Gestão de Tarefas](#classes-de-gestão-de-tarefas)
  - [Classes de Recursos e Informações](#classes-de-recursos-e-informações)
  - [Classes de Relatórios e Analytics](#classes-de-relatórios-e-analytics)
- [Relacionamentos](#relacionamentos)
- [Mapeamento com Requisitos](#mapeamento-com-requisitos)
- [Padrões de Design Aplicados](#padrões-de-design-aplicados)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Este documento apresenta o diagrama de classes para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza. O diagrama de classes é um elemento central da modelagem estrutural em UML, representando a estrutura estática do sistema, incluindo as classes, seus atributos, operações e as relações entre elas.

Enquanto os diagramas de atividades, BPMN e de estados focam no comportamento dinâmico e nos processos do sistema, o diagrama de classes fornece uma visão da arquitetura estrutural que servirá como base para a implementação do sistema. Esta estrutura reflete o modelo de domínio e as regras de negócio documentadas anteriormente, mas organizada de forma mais próxima à implementação técnica.

## Notação UML Utilizada

O diagrama de classes segue a notação UML (Unified Modeling Language) padrão, incluindo os seguintes elementos:

- **Classes**: Representadas por retângulos divididos em três seções (nome, atributos e métodos)
- **Atributos**: Listados na segunda seção da classe, com indicação de visibilidade e tipo
- **Métodos**: Listados na terceira seção, com indicação de visibilidade, parâmetros e tipo de retorno
- **Visibilidade**: + (público), - (privado), # (protegido)
- **Relacionamentos**:
  - Associação: Linha sólida entre classes
  - Agregação: Linha sólida com diamante vazado
  - Composição: Linha sólida com diamante preenchido
  - Herança: Linha sólida com triângulo vazado apontando para a superclasse
  - Dependência: Linha tracejada com seta
- **Multiplicidade**: Indicações numéricas nas extremidades das linhas de associação (1, *, 0..1, 1..*, etc.)

## Diagrama de Classes

```
+--------------------------------+      +----------------------------------+      +------------------------------------+
|           <<abstract>>         |      |                                  |      |                                    |
|            Usuario             |      |              Perfil              |      |               Unidade              |
+--------------------------------+      +----------------------------------+      +------------------------------------+
| - id: Long                     |      | - id: Long                       |      | - id: Long                         |
| - nome: String                 |      | - nome: String                   |      | - nome: String                     |
| - email: String                |      | - descricao: String              |      | - tipo: TipoUnidade                |
| - senha: String                |      | - permissoes: Set<Permissao>     |      | - endereco: String                 |
| - cargo: String                |<>----| # perfil: Perfil                 |      | - contatoPrincipal: String         |
| - dataCadastro: LocalDate      |1    1| + adicionarPermissao(): void     |      | - recursosAcessibilidade: String[] |
| - status: StatusUsuario        |      | + removerPermissao(): void       |      | + adicionarRecurso(): void         |
| + autenticar(): boolean        |      | + verificarPermissao(): boolean  |      | + removerRecurso(): void           |
| + alterarSenha(): void         |      +----------------------------------+      +------------------------------------+
| + recuperarSenha(): void       |                                                             ^
+------------^-------------------+                                                             |
             |                                                                                 |
 +-----------+-----------+-----------------+----------------+                                  |
 |                       |                 |                |                                  |
 v                       v                 v                v                                  |
+------------+   +---------------+  +-------------+  +----------------+                       |
|            |   |               |  |             |  |                |                       |
| AdminSis   |   | Gestor       |  | Tecnico     |  | Coordenador    |---------------------+
+------------+   +---------------+  +-------------+  +----------------+                    1|
|+ gerenciar |   |+ distribuir   |  |+ executar   |  |+ registrar     |                     |
| Sistema(): |   | Tarefas()     |  | Atend()     |  | Solicitacao()  |                     |
|  void      |   |+ gerarRelat() |  |+ registrar  |  |+ acompanhar    |                     |
+------------+   +---------------+  | Acoes()     |  | Status()       |                     |
                                    +-------------+  +----------------+                     |
                                                                                           |
+------------------------------------+      +----------------------------------+           |
|          Atendimento               |      |               Tarefa             |           |
+------------------------------------+      +----------------------------------+           |
| - protocolo: String                |      | - id: Long                       |           |
| - dataAbertura: LocalDateTime      |      | - titulo: String                 |           |
| - ultimaAtualizacao: LocalDateTime |      | - descricao: String              |           |
| - status: StatusAtendimento        |<>----| - atendimento: Atendimento       |           |
| - descricao: String                |1    *| - responsavel: Tecnico           |           |
| - unidadeSolicitante: Unidade      |<>----| - dataCriacao: LocalDateTime     |           |
| - responsavel: Tecnico             |      | - prazo: LocalDateTime           |           |
| - tipoNecessidade: TipoNecessidade |<>----| - status: StatusTarefa           |           |
| - prioridade: NivelPrioridade      |      | - prioridade: NivelPrioridade    |           |
| - historicoAlteracoes: List<Log>   |      | + atualizarProgresso(): void     |           |
| + atribuirResponsavel(): void      |      | + concluir(): void               |           |
| + atualizarStatus(): void          |      | + registrarComentario(): void    |           |
| + adicionarTarefa(): void          |      +----------------------------------+           |
| + finalizarAtendimento(): void     |                                                     |
| + reabrir(): boolean               |                                                     |
+------------------------------------+                                                     |
                ^                                                                          |
                |                                                                          |
+---------------v------------------+     +-----------------------------------+             |
|       HistoricoAtendimento       |     |       TipoNecessidade            |<------------+
+----------------------------------|     +-----------------------------------+
| - id: Long                       |     | - id: Long                       |
| - atendimento: Atendimento       |     | - nome: String                   |
| - dataHora: LocalDateTime        |     | - descricao: String              |
| - usuario: Usuario               |     | - protocolosSugeridos: String    |
| - acaoRealizada: String          |     | + obterEstatisticas(): Map       |
| - estadoAnterior: String         |     +-----------------------------------+
| - estadoNovo: String             |
| + registrarAlteracao(): void     |
+----------------------------------+

+----------------------------------+     +-----------------------------------+     +----------------------------------+
|         Material                 |     |      ProfissionalEspecializado    |     |        AdaptacaoCurricular      |
+----------------------------------+     +-----------------------------------+     +----------------------------------+
| - id: Long                       |     | - id: Long                        |     | - id: Long                       |
| - titulo: String                 |     | - nome: String                    |     | - curso: String                  |
| - descricao: String              |     | - areasEspecializacao: String[]   |     | - disciplina: String             |
| - categoria: String              |     | - unidade: Unidade                |     | - tipoAdaptacao: String          |
| - tags: String[]                 |     | - contato: String                 |     | - descricao: String              |
| - dataInclusao: LocalDateTime    |     | - disponibilidade: String         |     | - resultadosObservados: String   |
| - autor: Usuario                 |     | - experiencia: String             |     | - responsavel: Usuario           |
| - formato: String                |     | + verificarDisponibilidade():bool |     | - dataImplementacao: LocalDate   |
| - localArmazenamento: String     |     +-----------------------------------+     | + registrarResultados(): void    |
| - permissoesAcesso: Set<Perfil>  |                                               +----------------------------------+
| - versao: int                    |
| - estado: EstadoMaterial         |
| + atualizarVersao(): void        |
| + alterarPermissoes(): void      |
| + arquivar(): void               |
+----------------------------------+

+----------------------------------+     +-----------------------------------+     +----------------------------------+
|         Relatorio               |     |          ConfigRelatorio           |     |         Dashboard               |
+----------------------------------+     +-----------------------------------+     +----------------------------------+
| - id: Long                       |     | - id: Long                        |     | - id: Long                      |
| - titulo: String                 |<>---| - relatorio: Relatorio            |     | - nome: String                  |
| - descricao: String              |1   *| - parametros: Map<String,Object>  |     | - usuario: Usuario              |
| - tipo: TipoRelatorio            |     | - dataUltimaExecucao: LocalDate   |     | - componentes: List<Componente> |
| - criador: Usuario               |     | - agendamento: String             |     | + adicionarComponente(): void    |
| - camposDisponiveis: List<Campo> |     | + executar(): Relatorio           |     | + removerComponente(): void      |
| - predefinido: boolean           |     | + agendar(): void                 |     | + configurarLayout(): void       |
| + gerarRelatorio(): byte[]       |     | + compartilhar(): void            |     +----------------------------------+
| + exportar(formato): File        |     +-----------------------------------+
+----------------------------------+

+----------------------------------+     +-----------------------------------+
|         Notificacao              |     |            Log                    |
+----------------------------------+     +-----------------------------------+
| - id: Long                       |     | - id: Long                        |
| - destinatario: Usuario          |     | - dataHora: LocalDateTime         |
| - tipo: TipoNotificacao          |     | - usuario: Usuario                |
| - titulo: String                 |     | - acao: String                    |
| - conteudo: String               |     | - entidade: String                |
| - dataEnvio: LocalDateTime       |     | - entidadeId: Long                |
| - lida: boolean                  |     | - detalhes: String                |
| + marcarComoLida(): void         |     | + registrar(): static void        |
| + enviar(): void                 |     +-----------------------------------+
+----------------------------------+
```

## Descrição das Classes

### Classes de Controle de Acesso

#### Usuario (Abstrata)
Representa os usuários do sistema, com diferentes perfis e permissões.

**Atributos principais**:
- `id`: identificador único
- `nome`: nome completo do usuário
- `email`: email institucional para login
- `senha`: senha criptografada
- `cargo`: função/cargo na instituição
- `dataCadastro`: data de criação da conta
- `status`: estado atual (ativo, inativo, bloqueado)
- `perfil`: perfil de acesso associado

**Métodos principais**:
- `autenticar()`: valida credenciais
- `alterarSenha()`: permite mudar a senha
- `recuperarSenha()`: inicia processo de redefinição de senha

#### Perfil
Define os diferentes perfis de acesso no sistema.

**Atributos principais**:
- `nome`: identificador do perfil (Admin, Gestor, Técnico, Coordenador)
- `descricao`: descrição das responsabilidades
- `permissoes`: conjunto de permissões associadas

**Métodos principais**:
- `adicionarPermissao()`: adiciona nova permissão ao perfil
- `removerPermissao()`: remove permissão existente
- `verificarPermissao()`: verifica se perfil possui determinada permissão

#### Classes derivadas de Usuario

**AdminSistema**:
- Representa os administradores com acesso total
- Método adicional: `gerenciarSistema()`

**Gestor**:
- Representa os gestores da Assessoria de Inclusão
- Métodos adicionais: `distribuirTarefas()`, `gerarRelatorios()`

**Tecnico**:
- Representa os técnicos que executam os atendimentos
- Métodos adicionais: `executarAtendimento()`, `registrarAcoes()`

**Coordenador**:
- Representa coordenadores das unidades do CPS
- Métodos adicionais: `registrarSolicitacao()`, `acompanharStatus()`

### Classes de Gestão de Atendimentos

#### Atendimento
Representa os casos atendidos pela Assessoria de Inclusão.

**Atributos principais**:
- `protocolo`: número único de identificação
- `dataAbertura`: data/hora da abertura
- `ultimaAtualizacao`: data/hora da última modificação
- `status`: estado atual (registrado, em análise, atribuído, etc.)
- `descricao`: detalhamento do caso
- `unidadeSolicitante`: unidade que solicitou o atendimento
- `responsavel`: técnico designado
- `tipoNecessidade`: categorização da necessidade
- `prioridade`: nível de prioridade
- `historicoAlteracoes`: registro de mudanças

**Métodos principais**:
- `atribuirResponsavel()`: designa um técnico responsável
- `atualizarStatus()`: altera o estado do atendimento
- `adicionarTarefa()`: cria uma nova tarefa vinculada
- `finalizarAtendimento()`: conclui o atendimento
- `reabrir()`: reabre um atendimento concluído

#### HistoricoAtendimento
Registra as alterações ocorridas em um atendimento.

**Atributos principais**:
- `atendimento`: referência ao atendimento
- `dataHora`: momento da alteração
- `usuario`: quem realizou a alteração
- `acaoRealizada`: descrição da ação
- `estadoAnterior`: estado antes da alteração
- `estadoNovo`: estado após a alteração

**Métodos principais**:
- `registrarAlteracao()`: cria novo registro de histórico

#### TipoNecessidade
Representa as categorias de necessidades educacionais especiais.

**Atributos principais**:
- `nome`: identificador do tipo (visual, auditiva, etc.)
- `descricao`: detalhamento do tipo de necessidade
- `protocolosSugeridos`: orientações para atendimento

**Métodos principais**:
- `obterEstatisticas()`: retorna dados estatísticos sobre atendimentos desse tipo

### Classes de Gestão de Tarefas

#### Tarefa
Representa atividades específicas relacionadas a um atendimento.

**Atributos principais**:
- `titulo`: nome da tarefa
- `descricao`: detalhamento da atividade
- `atendimento`: atendimento ao qual está vinculada
- `responsavel`: técnico designado
- `dataCriacao`: data/hora de criação
- `prazo`: data/hora limite para conclusão
- `status`: estado atual (criada, em andamento, concluída, etc.)
- `prioridade`: nível de prioridade

**Métodos principais**:
- `atualizarProgresso()`: registra avanço na execução
- `concluir()`: finaliza a tarefa
- `registrarComentario()`: adiciona observações

### Classes de Recursos e Informações

#### Unidade
Representa as instituições de ensino do CPS.

**Atributos principais**:
- `nome`: nome da unidade
- `tipo`: classificação (Etec, Fatec, Classe Descentralizada)
- `endereco`: localização
- `contatoPrincipal`: responsável para contato
- `recursosAcessibilidade`: recursos de inclusão disponíveis

**Métodos principais**:
- `adicionarRecurso()`: registra novo recurso de acessibilidade
- `removerRecurso()`: remove um recurso existente

#### Material
Representa documentos e recursos disponíveis no repositório.

**Atributos principais**:
- `titulo`: nome do material
- `descricao`: detalhamento do conteúdo
- `categoria`: classificação
- `tags`: palavras-chave para busca
- `dataInclusao`: data de inclusão no repositório
- `autor`: usuário que incluiu
- `formato`: tipo de arquivo
- `localArmazenamento`: caminho ou URL
- `permissoesAcesso`: quem pode acessar
- `versao`: número da versão atual
- `estado`: situação atual (rascunho, publicado, arquivado)

**Métodos principais**:
- `atualizarVersao()`: cria nova versão
- `alterarPermissoes()`: modifica quem pode acessar
- `arquivar()`: marca como arquivado

#### ProfissionalEspecializado
Representa especialistas em áreas de inclusão.

**Atributos principais**:
- `nome`: nome do profissional
- `areasEspecializacao`: áreas de expertise
- `unidade`: local de atuação
- `contato`: informações para contato
- `disponibilidade`: horários disponíveis
- `experiencia`: descrição da experiência

**Métodos principais**:
- `verificarDisponibilidade()`: checa disponibilidade para consulta

#### AdaptacaoCurricular
Registra adaptações implementadas em cursos.

**Atributos principais**:
- `curso`: curso adaptado
- `disciplina`: disciplina específica
- `tipoAdaptacao`: natureza da adaptação
- `descricao`: detalhamento
- `resultadosObservados`: feedback da implementação
- `responsavel`: quem implementou
- `dataImplementacao`: quando foi realizada

**Métodos principais**:
- `registrarResultados()`: documenta resultados observados

### Classes de Relatórios e Analytics

#### Relatorio
Representa os diferentes relatórios disponíveis no sistema.

**Atributos principais**:
- `titulo`: nome do relatório
- `descricao`: finalidade
- `tipo`: categoria do relatório
- `criador`: usuário que criou
- `camposDisponiveis`: dados que podem ser incluídos
- `predefinido`: se é um relatório padrão ou personalizado

**Métodos principais**:
- `gerarRelatorio()`: produz o relatório com base em parâmetros
- `exportar()`: converte para formato externo

#### ConfigRelatorio
Armazena configurações específicas de um relatório.

**Atributos principais**:
- `relatorio`: relatório base
- `parametros`: configurações específicas
- `dataUltimaExecucao`: última vez que foi executado
- `agendamento`: programação de execução automática

**Métodos principais**:
- `executar()`: gera relatório com estas configurações
- `agendar()`: programa execução automática
- `compartilhar()`: distribui com outros usuários

#### Dashboard
Representa painéis de controle personalizados.

**Atributos principais**:
- `nome`: identificação do dashboard
- `usuario`: dono do dashboard
- `componentes`: elementos visuais incluídos

**Métodos principais**:
- `adicionarComponente()`: inclui novo elemento visual
- `removerComponente()`: remove elemento existente
- `configurarLayout()`: ajusta a disposição visual

#### Notificacao
Gerencia alertas e mensagens do sistema.

**Atributos principais**:
- `destinatario`: usuário que receberá
- `tipo`: categoria da notificação
- `titulo`: assunto
- `conteudo`: corpo da mensagem
- `dataEnvio`: quando foi enviada
- `lida`: se foi visualizada

**Métodos principais**:
- `marcarComoLida()`: marca notificação como lida
- `enviar()`: envia para o destinatário

#### Log
Registra eventos do sistema para auditoria.

**Atributos principais**:
- `dataHora`: momento do evento
- `usuario`: quem realizou a ação
- `acao`: o que foi feito
- `entidade`: qual objeto foi afetado
- `entidadeId`: identificador do objeto
- `detalhes`: informações adicionais

**Métodos principais**:
- `registrar()`: cria novo registro de log (método estático)

## Relacionamentos

### Associações Principais

1. **Usuário - Perfil**: Composição (1:1)
   - Cada usuário tem exatamente um perfil
   - O perfil define as permissões do usuário no sistema

2. **Coordenador - Unidade**: Associação (1:1)
   - Um coordenador está vinculado a uma única unidade
   - Uma unidade pode ter vários coordenadores

3. **Atendimento - Unidade**: Associação (N:1)
   - Cada atendimento está associado a uma única unidade solicitante
   - Uma unidade pode solicitar vários atendimentos

4. **Atendimento - Técnico**: Associação (N:1)
   - Um atendimento tem um técnico responsável
   - Um técnico pode ser responsável por múltiplos atendimentos

5. **Atendimento - Tarefa**: Composição (1:N)
   - Um atendimento pode ter várias tarefas associadas
   - Uma tarefa pertence a exatamente um atendimento

6. **Atendimento - TipoNecessidade**: Associação (N:1)
   - Cada atendimento tem um tipo de necessidade principal
   - Um tipo de necessidade pode ser aplicado a vários atendimentos

7. **Atendimento - HistoricoAtendimento**: Composição (1:N)
   - Um atendimento possui múltiplos registros de histórico
   - Cada registro de histórico pertence a um único atendimento

8. **Relatorio - ConfigRelatorio**: Composição (1:N)
   - Um relatório pode ter múltiplas configurações salvas
   - Uma configuração está associada a um único relatório base

### Hierarquias

1. **Usuário (superclasse)**
   - AdminSistema (subclasse)
   - Gestor (subclasse)
   - Técnico (subclasse)
   - Coordenador (subclasse)

## Mapeamento com Requisitos

O diagrama de classes proposto atende aos requisitos funcionais identificados:

1. **RF-01: Controle de Acesso**
   - Classes: Usuario, Perfil e subclasses específicas

2. **RF-02: Gestão de Atendimentos**
   - Classes: Atendimento, HistoricoAtendimento, TipoNecessidade

3. **RF-03: Organização do Fluxo de Trabalho**
   - Classes: Tarefa, Notificacao, Dashboard

4. **RF-04: Geração de Relatórios**
   - Classes: Relatorio, ConfigRelatorio, Dashboard

5. **RF-05: Gestão de Informações de Inclusão**
   - Classes: Unidade, Material, ProfissionalEspecializado, AdaptacaoCurricular

## Padrões de Design Aplicados

1. **Strategy Pattern**: 
   - Implementado na classe Relatorio para permitir diferentes estratégias de geração de relatórios

2. **Observer Pattern**: 
   - Utilizado no sistema de notificações para atualizar usuários sobre mudanças em atendimentos e tarefas

3. **Composite Pattern**: 
   - Aplicado na estrutura do Dashboard para organizar componentes visuais

4. **State Pattern**: 
   - Usado para gerenciar os diferentes estados de Atendimento e Material

5. **Template Method**: 
   - Implementado nas operações comuns da classe abstrata Usuario

6. **Repository Pattern**: 
   - Sugerido para implementação das operações de persistência de cada entidade

7. **Factory Method**: 
   - Recomendado para criação de diferentes tipos de relatórios e notificações

## Considerações de Implementação

1. **Segurança e Controle de Acesso**:
   - Implementar autenticação robusta e controle de acesso baseado em perfis
   - Garantir que todas as operações sensíveis verifiquem permissões

2. **Persistência**:
   - Utilizar ORM para mapear as classes para o banco de dados
   - Implementar estratégias de cache para entidades frequentemente acessadas
   - Considerar abordagens de auditoria automática para alterações em entidades importantes

3. **Tratamento de Estados**:
   - Implementar máquinas de estado para gerenciar transições conforme documentado nos diagramas de estado
   - Validar regras de negócio antes de permitir transições de estado

4. **Escalabilidade**:
   - Projetar para suportar crescimento futuro no número de atendimentos e unidades
   - Implementar paginação e busca eficiente para grandes conjuntos de dados

5. **Integração**:
   - Considerar interfaces para integração com outros sistemas do CPS
   - Implementar API RESTful para permitir futuras integrações

6. **Gestão de Versões**:
   - Implementar versionamento eficiente para materiais no repositório
   - Considerar estratégias de merge para atualizações concorrentes

7. **Localização e Internacionalização**:
   - Preparar o sistema para suportar múltiplos idiomas, especialmente para conteúdo do repositório

8. **Performance**:
   - Otimizar consultas para relatórios complexos
   - Implementar geração assíncrona para relatórios de grande volume de dados
   - Considerar uso de caching para dashboards e relatórios frequentemente acessados