# Diagrama de Objetos
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação UML Utilizada](#notação-uml-utilizada)
- [Cenários de Exemplo](#cenários-de-exemplo)
  - [Cenário 1: Atendimento em Andamento](#cenário-1-atendimento-em-andamento)
  - [Cenário 2: Gestão de Materiais no Repositório](#cenário-2-gestão-de-materiais-no-repositório)
  - [Cenário 3: Geração de Relatório](#cenário-3-geração-de-relatório)
  - [Cenário 4: Equipe e Permissões](#cenário-4-equipe-e-permissões)
- [Descrição dos Objetos](#descrição-dos-objetos)
  - [Objetos de Usuários](#objetos-de-usuários)
  - [Objetos de Atendimentos](#objetos-de-atendimentos)
  - [Objetos de Tarefas](#objetos-de-tarefas)
  - [Objetos de Materiais](#objetos-de-materiais)
  - [Objetos de Relatórios](#objetos-de-relatórios)
- [Relação com o Diagrama de Classes](#relação-com-o-diagrama-de-classes)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

Enquanto o diagrama de classes apresenta a estrutura estática do sistema em termos de classes, atributos e relacionamentos, o diagrama de objetos oferece uma visão instantânea do sistema em um determinado momento, mostrando objetos específicos, seus valores de atributos e as ligações entre eles.

Este documento apresenta diagramas de objetos que ilustram cenários representativos do Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza, complementando o diagrama de classes e tornando mais tangível o funcionamento do sistema através de exemplos concretos. Estes diagramas servem como uma ponte entre o modelo conceitual e a implementação, ajudando desenvolvedores e stakeholders a visualizarem o sistema em operação.

## Notação UML Utilizada

Os diagramas de objetos neste documento seguem a notação UML (Unified Modeling Language) padrão, incluindo os seguintes elementos:

- **Objetos**: Representados por retângulos com o nome do objeto sublinhado, no formato `nomeObjeto : NomeClasse`
- **Valores de Atributos**: Listados dentro do retângulo do objeto no formato `nomeAtributo = valor`
- **Links**: Representados por linhas que conectam objetos, indicando instâncias de associações
- **Multiplicidade**: Indicada nas extremidades dos links quando necessário
- **Estereótipos**: Utilizados para adicionar informações específicas aos elementos do diagrama

## Cenários de Exemplo

### Cenário 1: Atendimento em Andamento

Este cenário ilustra um atendimento específico em andamento no sistema, mostrando o relacionamento entre objetos de usuário, atendimento, tarefas e histórico.

```
                                                     +------------------------------------------------+
                                                     |        coordenadorEtec123 : Coordenador        |
                                                     +------------------------------------------------+
                                                     | nome = "Carlos Silva"                          |
                                                     | email = "carlos.silva@etec.sp.gov.br"          |
                                                     | cargo = "Coordenador Pedagógico"               |
                                                     | dataCadastro = 2024-02-15                      |
                                                     | status = ATIVO                                 |
                                                     +------------------------------------------------+
                                                                          ▲
                                                                          | solicitante
                                                                          |
+---------------------------------------------+                           |
|            etec123 : Unidade                |                           |
+---------------------------------------------+                           |
| nome = "Etec Armando Pannunzio"            |◄---------------------------|
| tipo = ETEC                                 |                           |
| endereco = "Rua Dr. João Machado, 1.434"   |                           |
| contatoPrincipal = "direção@etecap.com.br" |                           |
| recursosAcessibilidade = ["Rampas",         |                           |
|                          "Piso tátil"]      |                           |
+---------------------------------------------+                           |
                      ▲                                                   |
                      | unidade                                           |
                      |                                                   |
                      |                                                   |
                      |                                                   |
+---------------------------------------------+                           |
|       tipoDefVisual : TipoNecessidade       |                           |
+---------------------------------------------+                           |
| nome = "Deficiência Visual"                 |                           |
| descricao = "Baixa visão ou cegueira"       |                           |
| protocolosSugeridos = "Protocolo DV-2023"   |                           |
+---------------------------------------------+                           |
                      ▲                                                   |
                      | tipoNecessidade                                   |
                      |                                                   |
+---------------------------------------------+                           |
|       atendimento2024042 : Atendimento      |◄--------------------------|
+---------------------------------------------+
| protocolo = "A2024-042"                     |
| dataAbertura = 2024-04-10 10:15:00          |
| ultimaAtualizacao = 2024-04-15 09:30:00     |
| status = EM_ATENDIMENTO                     |
| descricao = "Aluno com baixa visão..."      |
| prioridade = ALTA                           |
+---------------------------------------------+
          ▲                    ▲
          |                    |
          | responsável        | registra
          |                    |
+---------------------+    +----------------------------------------+
| tecnico1 : Tecnico  |    | historico1 : HistoricoAtendimento     |
+---------------------+    +----------------------------------------+
| nome = "Rafael M."  |    | dataHora = 2024-04-10 14:22:00        |
| email = "rafael@..." |    | acaoRealizada = "Atribuição de resp." |
| status = ATIVO      |    | estadoAnterior = "REGISTRADO"         |
+---------------------+    | estadoNovo = "ATRIBUIDO"              |
          ▲                +----------------------------------------+
          |
          | responsável
          |
+-------------------------------------------------+      +-----------------------------------------------+
|            tarefa20240501 : Tarefa              |      |           tarefa20240502 : Tarefa             |
+-------------------------------------------------+      +-----------------------------------------------+
| titulo = "Preparar materiais ampliados"         |      | titulo = "Contatar NAPNE"                     |
| descricao = "Preparar apostilas com fonte 16pt" |      | descricao = "Agendar reunião com NAPNE"      |
| dataCriacao = 2024-04-12 15:45:00               |      | dataCriacao = 2024-04-12 15:50:00            |
| prazo = 2024-04-18 17:00:00                     |      | prazo = 2024-04-20 12:00:00                  |
| status = EM_ANDAMENTO                           |      | status = PENDENTE                             |
| prioridade = ALTA                               |      | prioridade = MEDIA                            |
+-------------------------------------------------+      +-----------------------------------------------+
```

### Cenário 2: Gestão de Materiais no Repositório

Este cenário ilustra o repositório de materiais, mostrando exemplos de materiais disponíveis e seus metadados.

```
+-----------------------------------------------+
|        gestora1 : Gestor                     |
+-----------------------------------------------+
| nome = "Marina Gomes"                        |
| email = "marina.gomes@cps.sp.gov.br"         |
| cargo = "Coordenadora de Assessoria"         |
| status = ATIVO                               |
+-----------------------------------------------+
           |
           | autor
           ▼
+-----------------------------------------------+
|    materialCartilhaDV : Material             |
+-----------------------------------------------+
| titulo = "Cartilha de Inclusão - Def Visual" |
| descricao = "Guia para docentes..."          |
| categoria = "Material de Apoio"              |
| tags = ["deficiência visual", "docentes"]    |
| dataInclusao = 2024-01-15 09:20:00           |
| formato = "PDF"                              |
| localArmazenamento = "/repositorio/dv/..."   |
| versao = 2                                   |
| estado = PUBLICADO                           |
+-----------------------------------------------+
           |
           | temVersãoAnterior
           ▼
+-----------------------------------------------+
| materialCartilhaDV_v1 : Material             |
+-----------------------------------------------+
| titulo = "Cartilha de Inclusão - Def Visual" |
| descricao = "Guia para docentes..."          |
| versao = 1                                   |
| estado = ARQUIVADO                           |
+-----------------------------------------------+

+-----------------------------------------------+
|       tecnico2 : Tecnico                     |
+-----------------------------------------------+
| nome = "Amanda Silva"                        |
| email = "amanda.silva@cps.sp.gov.br"         |
| cargo = "Especialista em Inclusão"           |
| status = ATIVO                               |
+-----------------------------------------------+
           |
           | autor
           ▼
+-----------------------------------------------+
|     materialAudioDescricao : Material        |
+-----------------------------------------------+
| titulo = "Tutorial de Audiodescrição"        |
| descricao = "Técnicas para criar..."         |
| categoria = "Tutorial"                       |
| tags = ["audiodescrição", "acessibilidade"]  |
| dataInclusao = 2024-03-22 14:30:00           |
| formato = "MP4"                              |
| localArmazenamento = "/repositorio/ad/..."   |
| versao = 1                                   |
| estado = AGUARDANDO_APROVACAO                |
+-----------------------------------------------+
```

### Cenário 3: Geração de Relatório

Este cenário ilustra a geração de um relatório personalizado, mostrando a relação entre o objeto de relatório, sua configuração e o usuário criador.

```
+-----------------------------------------------+
|        gestora1 : Gestor                     |
+-----------------------------------------------+
| nome = "Marina Gomes"                        |
| email = "marina.gomes@cps.sp.gov.br"         |
| cargo = "Coordenadora de Assessoria"         |
| status = ATIVO                               |
+-----------------------------------------------+
           |
           | criador
           ▼
+-----------------------------------------------+
|     relatorioAtendimentosMensais : Relatorio  |
+-----------------------------------------------+
| titulo = "Atendimentos por Tipo de Necessidade"|
| descricao = "Relatório mensal de atendimentos"|
| tipo = ESTATISTICO                           |
| camposDisponiveis = ["tipoNecessidade",      |
|                      "status",               |
|                      "unidade"]              |
| predefinido = true                           |
+-----------------------------------------------+
           |
           | configuração
           ▼
+-----------------------------------------------+
|   configAbril2024 : ConfigRelatorio         |
+-----------------------------------------------+
| parametros = {                               |
|   "dataInicio": "2024-04-01",               |
|   "dataFim": "2024-04-30",                  |
|   "agruparPor": "tipoNecessidade",          |
|   "incluirGrafico": true                    |
| }                                           |
| dataUltimaExecucao = 2024-05-01 08:15:00    |
| agendamento = "MENSAL:1:8:00"               |
+-----------------------------------------------+
```

### Cenário 4: Equipe e Permissões

Este cenário ilustra a estrutura da equipe da Assessoria de Inclusão, mostrando os diferentes usuários, seus perfis e permissões.

```
+-----------------------------------------------+
|       perfilAdmin : Perfil                   |
+-----------------------------------------------+
| nome = "Administrador"                       |
| descricao = "Acesso total ao sistema"        |
| permissoes = ["GERENCIAR_USUARIOS",          |
|               "GERENCIAR_SISTEMA",           |
|               "CONFIGURAR_PARAMETROS",       |
|               "TODOS_OS_RELATORIOS"]         |
+-----------------------------------------------+
           |
           | perfil
           ▼
+-----------------------------------------------+
|       admin1 : AdminSistema                  |
+-----------------------------------------------+
| nome = "Juliana Costa"                       |
| email = "juliana.costa@cps.sp.gov.br"        |
| cargo = "Analista de Sistemas"               |
| dataCadastro = 2023-10-01                    |
| status = ATIVO                               |
+-----------------------------------------------+

+-----------------------------------------------+
|       perfilGestor : Perfil                  |
+-----------------------------------------------+
| nome = "Gestor"                              |
| descricao = "Gestão da Assessoria"           |
| permissoes = ["GERENCIAR_ATENDIMENTOS",      |
|               "ATRIBUIR_RESPONSAVEIS",       |
|               "GERAR_RELATORIOS",            |
|               "APROVAR_MATERIAIS"]           |
+-----------------------------------------------+
           |
           | perfil
           ▼
+-----------------------------------------------+
|        gestora1 : Gestor                     |
+-----------------------------------------------+
| nome = "Marina Gomes"                        |
| email = "marina.gomes@cps.sp.gov.br"         |
| cargo = "Coordenadora de Assessoria"         |
| status = ATIVO                               |
+-----------------------------------------------+

+-----------------------------------------------+
|       perfilTecnico : Perfil                 |
+-----------------------------------------------+
| nome = "Técnico"                             |
| descricao = "Execução de atendimentos"       |
| permissoes = ["EXECUTAR_ATENDIMENTOS",       |
|               "REGISTRAR_ACOES",             |
|               "ADICIONAR_MATERIAIS",         |
|               "RELATORIOS_BASICOS"]          |
+-----------------------------------------------+
           |
           | perfil
           ▼
+-----------------------------------------------+
|       tecnico1 : Tecnico                     |
+-----------------------------------------------+
| nome = "Rafael Menezes"                      |
| email = "rafael.menezes@cps.sp.gov.br"       |
| cargo = "Especialista em Inclusão"           |
| status = ATIVO                               |
+-----------------------------------------------+

+-----------------------------------------------+
|       perfilCoordenador : Perfil             |
+-----------------------------------------------+
| nome = "Coordenador"                         |
| descricao = "Coordenação de unidade"         |
| permissoes = ["REGISTRAR_SOLICITACOES",      |
|               "VISUALIZAR_ATENDIMENTOS",     |
|               "ACESSAR_REPOSITORIO"]         |
+-----------------------------------------------+
           |
           | perfil
           ▼
+-----------------------------------------------+
|       coordenadorEtec123 : Coordenador       |
+-----------------------------------------------+
| nome = "Carlos Silva"                        |
| email = "carlos.silva@etec.sp.gov.br"        |
| cargo = "Coordenador Pedagógico"             |
| status = ATIVO                               |
+-----------------------------------------------+
           |
           | unidade
           ▼
+-----------------------------------------------+
|            etec123 : Unidade                 |
+-----------------------------------------------+
| nome = "Etec Armando Pannunzio"              |
| tipo = ETEC                                  |
| endereco = "Rua Dr. João Machado, 1.434"     |
+-----------------------------------------------+
```

## Descrição dos Objetos

### Objetos de Usuários

1. **admin1 : AdminSistema**
   - Representa a administradora do sistema Juliana Costa, responsável pela manutenção técnica e configuração do sistema
   - Possui o perfil de Administrador com acesso total ao sistema
   - Mantém os dados cadastrais dos usuários e configura parâmetros gerais

2. **gestora1 : Gestor**
   - Representa a coordenadora Marina Gomes, responsável pela gestão operacional da Assessoria de Inclusão
   - Possui o perfil de Gestor, permitindo atribuir responsáveis, aprovar materiais e gerar relatórios gerenciais
   - Supervisiona a equipe técnica e responde por resultados gerais da Assessoria

3. **tecnico1 : Tecnico**
   - Representa o especialista Rafael Menezes, que executa diretamente os atendimentos
   - Possui o perfil de Técnico, permitindo trabalhar nos casos, registrar ações e adicionar materiais ao repositório
   - É responsável pelos atendimentos que lhe são atribuídos

4. **coordenadorEtec123 : Coordenador**
   - Representa o coordenador pedagógico Carlos Silva da Etec Armando Pannunzio
   - Possui o perfil de Coordenador com permissões limitadas à sua unidade
   - Registra solicitações de atendimento para estudantes de sua unidade

### Objetos de Atendimentos

1. **atendimento2024042 : Atendimento**
   - Representa um caso em andamento para um aluno com deficiência visual
   - Aberto pelo coordenadorEtec123 em 10/04/2024
   - Atribuído ao tecnico1 como responsável pela resolução
   - Possui prioridade alta e está atualmente em atendimento

2. **historico1 : HistoricoAtendimento**
   - Representa um registro de alteração no atendimento2024042
   - Documenta a mudança de estado de "Registrado" para "Atribuído"
   - Inclui data, hora e usuário que realizou a alteração

3. **tipoDefVisual : TipoNecessidade**
   - Representa a categoria "Deficiência Visual" para classificação de atendimentos
   - Inclui descrição e protocolos sugeridos para este tipo de necessidade

### Objetos de Tarefas

1. **tarefa20240501 : Tarefa**
   - Representa a tarefa de preparação de materiais ampliados para o aluno
   - Vinculada ao atendimento2024042
   - Atribuída ao tecnico1 com prazo de 18/04/2024
   - Está em andamento e possui prioridade alta

2. **tarefa20240502 : Tarefa**
   - Representa a tarefa de contato com o NAPNE (Núcleo de Apoio às Pessoas com Necessidades Especiais)
   - Vinculada ao mesmo atendimento2024042
   - Também atribuída ao tecnico1 com prazo posterior
   - Está pendente e possui prioridade média

### Objetos de Materiais

1. **materialCartilhaDV : Material**
   - Representa a versão atual da cartilha de orientação sobre deficiência visual
   - Criado pela gestora1 e disponível no repositório
   - Está na versão 2 e com estado "Publicado"
   - Possui tags para facilitar a busca e identificação

2. **materialCartilhaDV_v1 : Material**
   - Representa a versão anterior da mesma cartilha
   - Mantida no sistema para histórico
   - Está com estado "Arquivado" para indicar que não é mais a versão atual

3. **materialAudioDescricao : Material**
   - Representa um tutorial em vídeo sobre audiodescrição
   - Criado pela tecnico2 e aguardando aprovação para publicação
   - Está na versão 1 e com estado "Aguardando Aprovação"

### Objetos de Relatórios

1. **relatorioAtendimentosMensais : Relatorio**
   - Representa um modelo de relatório predefinido para análise de atendimentos por tipo de necessidade
   - Criado pela gestora1 para acompanhamento gerencial
   - Define os campos disponíveis e o tipo de relatório

2. **configAbril2024 : ConfigRelatorio**
   - Representa uma configuração específica do relatório para o mês de abril/2024
   - Inclui parâmetros como período e agrupamento
   - Configurado para execução automática mensal

## Relação com o Diagrama de Classes

Os objetos apresentados nestes diagramas são instâncias das classes definidas no diagrama de classes do sistema. A relação entre eles é a seguinte:

1. **Classes de Controle de Acesso**
   - Os objetos admin1, gestora1, tecnico1 e coordenadorEtec123 são instâncias das respectivas subclasses de Usuario
   - Os objetos perfilAdmin, perfilGestor, perfilTecnico e perfilCoordenador são instâncias da classe Perfil
   - O objeto etec123 é uma instância da classe Unidade

2. **Classes de Gestão de Atendimentos**
   - O objeto atendimento2024042 é uma instância da classe Atendimento
   - O objeto historico1 é uma instância da classe HistoricoAtendimento
   - O objeto tipoDefVisual é uma instância da classe TipoNecessidade

3. **Classes de Gestão de Tarefas**
   - Os objetos tarefa20240501 e tarefa20240502 são instâncias da classe Tarefa

4. **Classes de Recursos e Informações**
   - Os objetos materialCartilhaDV, materialCartilhaDV_v1 e materialAudioDescricao são instâncias da classe Material

5. **Classes de Relatórios e Analytics**
   - O objeto relatorioAtendimentosMensais é uma instância da classe Relatorio
   - O objeto configAbril2024 é uma instância da classe ConfigRelatorio

Os relacionamentos entre os objetos refletem as associações definidas entre as classes, como a relação entre Atendimento e Tarefa, Usuario e Perfil, etc. Os valores dos atributos apresentados são exemplos concretos do que seria armazenado no sistema em um cenário real de operação.

## Considerações de Implementação

1. **Persistência e Identidade de Objetos**:
   - Na implementação real, cada objeto terá um identificador único para persistência em banco de dados
   - Os relacionamentos serão implementados através de chaves estrangeiras ou mecanismos equivalentes de ORM

2. **Valores de Atributos**:
   - Os valores apresentados nos diagramas são exemplos representativos
   - Na implementação, tipos como datas, enumerações e listas precisarão ser adequadamente convertidos para os tipos de dados do sistema

3. **Estado e Comportamento**:
   - Os diagramas de objetos mostram apenas o estado dos objetos, não seu comportamento
   - A implementação deverá garantir que os métodos de cada classe manipulem corretamente os atributos conforme as regras de negócio

4. **Ciclo de Vida dos Objetos**:
   - O diagrama representa um "snapshot" do sistema em um momento específico
   - A implementação deverá gerenciar todo o ciclo de vida dos objetos, desde sua criação até sua eventual arquivação/exclusão

5. **Consistência dos Dados**:
   - A implementação deverá garantir a consistência dos relacionamentos bidirecionais
   - Por exemplo, se uma Tarefa está associada a um Atendimento, o Atendimento também deve incluir essa Tarefa em sua coleção

6. **Validação de Estados**:
   - As transições entre estados dos objetos devem ser validadas conforme as regras de negócio
   - Por exemplo, um Atendimento só pode ser finalizado quando todas suas Tarefas estiverem concluídas

7. **Carregamento Lazy vs. Eager**:
   - Considerar estratégias de carregamento adequadas para relacionamentos
   - Por exemplo, ao carregar um Atendimento, pode ser necessário carregar imediatamente seu TipoNecessidade, mas deixar o carregamento do histórico completo para quando for explicitamente solicitado