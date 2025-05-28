# Diagrama de Fluxo de Dados (DFD)
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Notação Utilizada](#notação-utilizada)
- [DFD Nível 0 (Diagrama de Contexto)](#dfd-nível-0-diagrama-de-contexto)
- [DFD Nível 1 (Processos Principais)](#dfd-nível-1-processos-principais)
- [DFD Nível 2](#dfd-nível-2)
  - [Processo 1: Gerenciar Acesso](#processo-1-gerenciar-acesso)
  - [Processo 2: Gerenciar Atendimentos](#processo-2-gerenciar-atendimentos)
  - [Processo 3: Gerenciar Tarefas](#processo-3-gerenciar-tarefas)
  - [Processo 4: Gerenciar Repositório](#processo-4-gerenciar-repositório)
  - [Processo 5: Gerar Relatórios](#processo-5-gerar-relatórios)
- [Descrição dos Elementos](#descrição-dos-elementos)
  - [Entidades Externas](#entidades-externas)
  - [Processos](#processos)
  - [Fluxos de Dados](#fluxos-de-dados)
  - [Armazenamentos de Dados](#armazenamentos-de-dados)
- [Mapeamento com Requisitos](#mapeamento-com-requisitos)
- [Considerações de Implementação](#considerações-de-implementação)

## Introdução

O Diagrama de Fluxo de Dados (DFD) é uma representação gráfica que ilustra como os dados fluem através de um sistema de informação. Diferentemente dos diagramas de classes ou entidade-relacionamento que mostram estruturas estáticas, os DFDs focam no movimento, transformação e armazenamento dos dados, ajudando a visualizar como o sistema processa as informações.

Para o Sistema de Gestão da Assessoria de Inclusão do Centro Paula Souza, os DFDs são especialmente relevantes pois permitem:

- Visualizar como os dados fluem entre diferentes atores (coordenadores de unidades, técnicos, gestores)
- Identificar processos-chave que transformam dados em informações úteis
- Entender os requisitos de armazenamento de dados
- Mapear integrações com sistemas externos
- Fornecer uma visão processual complementar aos modelos estruturais e comportamentais

Este documento apresenta uma hierarquia de DFDs, começando com uma visão de alto nível (nível 0) e refinando progressivamente para mostrar detalhes de processos específicos (níveis 1 e 2).

## Notação Utilizada

Os diagramas utilizam a notação Yourdon/DeMarco, com os seguintes elementos:

- **Entidades Externas** (Retângulos): Representam origens ou destinos de dados fora do sistema. São fontes ou receptores de informações, como pessoas, organizações ou outros sistemas.

- **Processos** (Círculos ou Elipses): Representam atividades ou funções que manipulam dados, transformando entradas em saídas. São identificados por um número e um nome descritivo.

- **Fluxos de Dados** (Setas): Representam o movimento de dados no sistema, conectando processos, entidades externas e armazenamentos. São rotulados para indicar o tipo de informação transportada.

- **Armazenamentos de Dados** (Retângulos abertos ou linhas paralelas): Representam repositórios de dados que o sistema utiliza para armazenar ou recuperar informações.

## DFD Nível 0 (Diagrama de Contexto)

```
                            +--------------------------------------+
                            |                                      |
+----------------------+    |    Sistema de Gestão                 |    +----------------------+
|                      |    |    para Assessoria                   |    |                      |
|  Coordenadores de    +---->    de Inclusão                       +---->  Sistema LDAP/SSO    |
|  Unidades            |    |                                      |    |  do CPS              |
|                      <----+                                      <----+                      |
+----------------------+    |                                      |    +----------------------+
                            |                                      |
+----------------------+    |                                      |    +----------------------+
|                      |    |                                      |    |                      |
|  Técnicos da         +---->                                      +---->  Sistema de Email    |
|  Assessoria          |    |                                      |    |  do CPS              |
|                      <----+                                      |    |                      |
+----------------------+    |                                      |    +----------------------+
                            |                                      |
+----------------------+    |                                      |    +----------------------+
|                      |    |                                      |    |                      |
|  Gestores da         +---->                                      +---->  Sistema Acadêmico   |
|  Assessoria          |    |                                      |    |  do CPS              |
|                      <----+                                      <----+                      |
+----------------------+    |                                      |    +----------------------+
                            |                                      |
+----------------------+    |                                      |
|                      |    |                                      |
|  Administradores     +---->                                      |
|  do Sistema          |    |                                      |
|                      <----+                                      |
+----------------------+    |                                      |
                            +--------------------------------------+
```

## DFD Nível 1 (Processos Principais)

```
                                        Credenciais                                    Dados de
                                        de Usuário                                    Autenticação
+----------------------+              +------------+           +----------------------+
|                      |              |            |           |                      |
|  Coordenadores de    +--------------+            +---------->+  Sistema LDAP/SSO    |
|  Unidades            |              |     1.0    |           |  do CPS              |
|                      |  Solicitação |            |   Dados   |                      |
+----------------------+  de          |  Gerenciar |   de      +----------------------+
          |               Atendimento |  Acesso    |   Perfil            |
          |                           |            <--------------------------+
          |                           |            |                      |   |
          |                           +-----+------+                      |   |
          |                                 |                             |   |
          |                                 | Perfil                      |   |
          |                                 | do Usuário                  |   |
          |                                 |                             |   |
          |              +------------------v--+    Informações de        |   |
          |              |                    |    Unidades               |   |
          +------------->+       2.0          +---------------------->----+   |
          |   Registro   |                    |                              |
          |              |    Gerenciar       |                              |
          |              |   Atendimentos     |                              |
          |              |                    |                              |
          |              +-----+---------+----+                              |
          |                    |         |                                   |
          |                    |         |                                   |
          |    +---------------+         |                                   |
          |    |                         |                                   |
          |    | Informações             | Dados de                          |
          |    | de Atendimento          | Atendimentos                      |
+----------------------+  |    |         |      +----------------------+     |
|                      |  |    |         |      |                      |     |
|  Técnicos da         <--+    |         |      |  Sistema de Email    |     |
|  Assessoria          |       |         |      |  do CPS              |     |
|                      +-------+         |      |                      |     |
+----------------------+  Atualização    |      +----------------------+     |
          |               de Tarefas     |              ^                    |
          |                              |              |                    |
          |                              |              | Notificações       |
          |                              |              |                    |
          |              +--------------+v+             |                    |
          +------------->+               +-------------+                     |
                         |      3.0      |                                   |
                         |               |                                   |
                         |   Gerenciar   |                                   |
+----------------------+ |    Tarefas    | +----------------------+          |
|                      | |               | |                      |          |
|  Gestores da         +-+               +-+  Armazenamento      |          |
|  Assessoria          | |               | |  de Arquivos        |          |
|                      <-+               | |                     |          |
+----------------------+ |               | +----------------------+          |
          |              +-------+-------+           ^                      |
          |                      |                   |                      |
          |                      |                   | Arquivos             |
          |                      |                   |                      |
          |              +-------v-------+           |                      |
          +------------->+               +-----------+                      |
          |   Adição     |     4.0       |                                  |
          |   de         |               |                                  |
          |   Material   |  Gerenciar    |                                  |
          |              |  Repositório  |                                  |
          |              |               |                                  |
          |              +-------+-------+                                  |
          |                      |                                          |
          |                      |                                          |
          |                      | Metadados                                |
+----------------------+         | de Material                              |
|                      |         |                                          |
|  Administradores     |  +------v------+        Dados do                   |
|  do Sistema          |  |             |        Sistema          Dados     |
|                      +->+    5.0      +--------------------->  de Usuários|
|                      |  |             |                                   |
|                      <--+   Gerar     <-----------------------------------+
+----------------------+  |  Relatórios  |
                          |             |
                          +-------------+
```

## DFD Nível 2

### Processo 1: Gerenciar Acesso

```
                  Credenciais                         Solicitação de
                  de Usuário                          Validação
+-------------+  +--------------+  +-------------+  +--------------+  +-------------+
|             |  |              |  |             |  |              |  |             |
| Usuário     +->+ 1.1 Validar  +->+ 1.2 Verificar+->+ Sistema     |  |             |
| do Sistema  |  | Credenciais  |  | Permissões  |  | LDAP/SSO     |  |             |
|             |  |              |  |             |  |              |  |             |
+-------------+  +------+-------+  +------+------+  +------+-------+  +-------------+
                        |                 |                |
                        |                 |                | Resultado da
                        |                 |                | Validação
                        |                 |                |
                        |                 |                v
                        |                 |          +-----+-------+
                        |                 |          |             |
                        |                 +--------->+ 1.3 Gerenciar|
                        |    Decisão de              | Sessão      |
                        |    Autorização             |             |
                        |                            +------+------+
                        |                                   |
                        | Credenciais                       | Token de
                        | Inválidas                         | Sessão
                        v                                   v
                  +-----+-------+                     +-----+-------+
                  |             |                     |             |
                  | 1.4 Registrar|                    | 1.5 Carregar|
                  | Falha de    |                     | Perfil do   |
                  | Autenticação|                     | Usuário     |
                  |             |                     |             |
                  +-------------+                     +-------------+
```

### Processo 2: Gerenciar Atendimentos

```
                         Solicitação de                        Informações de
                         Atendimento                           Unidade
+-------------+         +-------------+         +-------------+         +-------------+
|             |         |             |         |             |         | Sistema     |
| Coordenador +-------->+ 2.1 Registrar+-------->+ 2.2 Validar+-------->+ Acadêmico   |
| de Unidade  |         | Atendimento |         | Dados       |         |             |
|             |         |             |         |             |         |             |
+-------------+         +------+------+         +------+------+         +------+------+
                               |                       |                       |
                               |                       |                       |
                               |                       |                       |
                               |                       |                       |
                               v                       |                       |
                         +-----+-----+                 |                       |
                         |           |                 |                       |
      +----------------->+ D1 Atendi-|<----------------+                       |
      |                  | mentos    |  Dados          |                       |
      |                  |           |  Validados      |                       |
      |                  +-----+-----+                 |                       |
      |                        |                       |  Dados                |
      |                        |                       |  da Unidade           |
      |                        |                       |                       |
      |       Atendimentos     |                       |                       |
      |       Pendentes        |                       v                       |
      |                        |                 +-----+------+                |
      |                        |                 |            |                |
      |                  +-----v-----+           | 2.3 Classi-+<---------------+
      |                  |           |           | ficar Aten-|
      |                  | 2.4 Atribuir|          | dimento    |
+-----+----+             | Responsável|           |            |
|          |             |           |           +------------+
| Gestor   +------------>+           |
|          |             +-----+-----+
+----------+                   |
                              |
                              | Atribuição
                              |
                              v
                        +-----+-----+         +-------------+
                        |           |         |             |
                        | 2.5 Notifi-+-------->+ Técnico     |
                        | car        |         | da Assessoria|
                        | Responsável|         |             |
                        |           |         |             |
                        +-----------+         +-------------+
```

### Processo 3: Gerenciar Tarefas

```
                     Criação de                      Atribuição
                     Tarefa                          de Tarefa
+-------------+     +------------+     +-------------+     +-------------+
|             |     |            |     |             |     |             |
| Gestor      +---->+ 3.1 Criar  +---->+ 3.2 Atribuir+---->+ Técnico     |
| da Assessoria|     | Tarefa    |     | Responsável |     | da Assessoria|
|             |     |            |     |             |     |             |
+-------------+     +-----+------+     +------+------+     +------+------+
                          |                   |                   |
                          |                   |                   |
                          v                   |                   |
                    +-----+-----+             |                   |
                    |           |             |                   |
                    | D2 Tarefas|<------------+                   |
                    |           |  Dados da                       |
                    |           |  Tarefa                         |
                    +-----+-----+                                 |
                          |                                       |
                          |                                       |
                          | Tarefas                               |
                          | Pendentes                             |
                          |                                       |
                          v                                       |
                    +-----+------+           Atualização          |
                    |            |           de Status            |
                    | 3.3 Monito-|<------------------------------+
                    | rar Progres|
                    | so         |
                    |            |
                    +-----+------+
                          |
                          |
                          | Tarefas
                          | Atrasadas
                          |
                          v
                    +-----+------+     +-------------+
                    |            |     |             |
                    | 3.4 Gerar  +---->+ Sistema de  |
                    | Alertas    |     | Email       |
                    |            |     |             |
                    +------------+     +-------------+
```

### Processo 4: Gerenciar Repositório

```
                       Upload de                      Validação
                       Material                       de Conteúdo
+-------------+       +------------+       +------------+       +------------+
|             |       |            |       |            |       |            |
| Técnico/    +------>+ 4.1 Receber+------>+ 4.2 Validar+------>+ 4.3 Armaze-|
| Gestor      |       | Material   |       | Conteúdo   |       | nar Arquivo|
|             |       |            |       |            |       |            |
+-------------+       +-----+------+       +-----+------+       +-----+------+
                            |                    |                    |
                            |                    |                    |
                            |                    |                    v
                            |                    |              +-----+------+
                            |                    |              |            |
                            |                    |              | D4 Armazena|
                            |                    |              | mento de   |
                            |                    |              | Arquivos   |
                            |                    |              |            |
                            |                    |              +-----+------+
                            |                    |                    |
                            |                    |                    |
                            |                    |                    | Referência
                            |                    |                    | ao Arquivo
                            v                    |                    |
                      +-----+------+             |                    |
                      |            |             |                    |
     +-------------->+ D3 Materiais|<------------+                    |
     |                |            |  Metadados                       |
     |                |            |  do Material                     |
     |                +-----+------+                                  |
     |                      |                                         |
     |                      | Informações                             |
     |                      | de Material                             |
     |                      |                                         |
     |                      v                                         |
     |                +-----+------+                                  |
     |                |            |                                  |
     |                | 4.4 Indexar+<---------------------------------+
     |                | Material   |
     |                |            |
     |                +-----+------+
     |                      |
     |                      |
     |                      | Metadados
     |                      | Indexados
     |                      |
     |                      v
+----+-------+        +-----+------+        +------------+
|            |        |            |        |            |
| Usuário    |        | 4.5 Buscar |        | 4.6 Contro-|
| do Sistema +------->+ Material   +------->+ lar Acesso |
|            |        |            |        |            |
|            |        |            |        |            |
+------------+        +------------+        +-----+------+
                                                  |
                                                  | Resultado
                                                  | da Busca
                                                  |
                                                  v
                                            +-----+------+
                                            |            |
                                            | 4.7 Visua- |
                                            | lizar      |
                                            | Material   |
                                            |            |
                                            +------------+
```

### Processo 5: Gerar Relatórios

```
                     Solicitação                    Definição
                     de Relatório                   de Parâmetros
+-------------+     +------------+     +-------------+
|             |     |            |     |             |
| Gestor      +---->+ 5.1 Seleci-+---->+ 5.2 Definir |
| da Assessoria|     | onar Tipo |     | Parâmetros  |
|             |     |            |     |             |
+-------------+     +-----+------+     +------+------+
                          |                   |
                          |                   |
                          v                   |
                    +-----+-----+             |
                    |           |             |
                    | D5 Configu|<------------+
                    | rações de |  Parâmetros
                    | Relatórios|
                    |           |
                    +-----+-----+
                          |
                          |
                          | Definição de
                          | Relatório
                          |
                          v
                    +-----+------+
                    |            |
                    | 5.3 Execu- |
                    | tar Consulta|
                    |            |
                    +-----+------+
                          |
                          |
                          | Consulta
                          |
                          v                                  Dados de
               +----------+----------+                       Atendimentos
               |                     |                            +
        +------+------+      +------+------+      +------+------+|
        |             |      |             |      |             ||
        | D1 Atendi-  |      | D2 Tarefas  |      | D3 Materiais||
        | mentos      |      |             |      |             ||
        |             |      |             |      |             ||
        +------+------+      +------+------+      +------+------+|
               |                    |                    |       |
               +--------------------+--------------------+       |
                                    |                           |
                                    v                           |
                             +------+------+                    |
                             |             |                    |
                             | 5.4 Processar|<-------------------+
                             | Dados       |
                             |             |
                             +------+------+
                                    |
                                    |
                                    | Dados
                                    | Processados
                                    |
                                    v
                             +------+------+
                             |             |
                             | 5.5 Formatar|
                             | Saída       |
                             |             |
                             +------+------+
                                    |
                                    |
                                    | Relatório
                                    | Formatado
                                    |
                                    v
                             +------+------+      +-------------+
                             |             |      |             |
                             | 5.6 Entregar+----->+ Destinatário|
                             | Relatório   |      | do Relatório|
                             |             |      |             |
                             +-------------+      +-------------+
```

## Descrição dos Elementos

### Entidades Externas

1. **Coordenadores de Unidades**
   - Profissionais responsáveis pela coordenação pedagógica nas unidades educacionais (Etecs/Fatecs)
   - Interações principais: Registrar solicitações de atendimento, consultar status, acessar materiais

2. **Técnicos da Assessoria**
   - Especialistas que executam os atendimentos e desenvolvem soluções
   - Interações principais: Atualizar status de tarefas, registrar ações, produzir materiais

3. **Gestores da Assessoria**
   - Coordenadores responsáveis pela organização do trabalho na Assessoria de Inclusão
   - Interações principais: Atribuir responsáveis, monitorar indicadores, gerar relatórios

4. **Administradores do Sistema**
   - Profissionais responsáveis pela manutenção técnica do sistema
   - Interações principais: Configurar parâmetros, gerenciar usuários, monitorar operação

5. **Sistema LDAP/SSO do CPS**
   - Sistema externo para autenticação centralizada
   - Interações principais: Validar credenciais, fornecer informações básicas de usuários

6. **Sistema de Email do CPS**
   - Sistema externo para envio de notificações
   - Interações principais: Receber requisições para envio de mensagens

7. **Sistema Acadêmico do CPS**
   - Sistema externo que gerencia informações sobre alunos e unidades
   - Interações principais: Fornecer dados sobre unidades e estudantes quando necessário

### Processos

#### Processos de Nível 1

1. **Gerenciar Acesso (1.0)**
   - Finalidade: Controlar autenticação e autorização de usuários
   - Entradas: Credenciais de usuário, dados de perfil
   - Saídas: Sessão autenticada, permissões do usuário

2. **Gerenciar Atendimentos (2.0)**
   - Finalidade: Registrar e acompanhar casos de atendimento
   - Entradas: Solicitações de atendimento, dados de unidades
   - Saídas: Atendimentos registrados, notificações

3. **Gerenciar Tarefas (3.0)**
   - Finalidade: Organizar atividades relacionadas aos atendimentos
   - Entradas: Atendimentos, atribuições de responsabilidade
   - Saídas: Tarefas organizadas, alertas de prazos

4. **Gerenciar Repositório (4.0)**
   - Finalidade: Administrar materiais e documentos técnicos
   - Entradas: Arquivos, metadados, categorias
   - Saídas: Materiais indexados, resultados de busca

5. **Gerar Relatórios (5.0)**
   - Finalidade: Produzir informações consolidadas para análise
   - Entradas: Parâmetros de relatório, dados do sistema
   - Saídas: Relatórios formatados, visualizações gráficas

#### Processos de Nível 2 (detalhamento de exemplos)

**Processo 2: Gerenciar Atendimentos**

- **2.1 Registrar Atendimento**: Captura informações iniciais do caso
- **2.2 Validar Dados**: Verifica consistência e completude das informações
- **2.3 Classificar Atendimento**: Categoriza o tipo de necessidade
- **2.4 Atribuir Responsável**: Define o técnico que atuará no caso
- **2.5 Notificar Responsável**: Comunica o técnico sobre a atribuição

**Processo 5: Gerar Relatórios**

- **5.1 Selecionar Tipo**: Escolha do modelo de relatório
- **5.2 Definir Parâmetros**: Configuração de filtros e critérios
- **5.3 Executar Consulta**: Recuperação dos dados necessários
- **5.4 Processar Dados**: Aplicação de cálculos e transformações
- **5.5 Formatar Saída**: Organização visual da informação
- **5.6 Entregar Relatório**: Disponibilização ao destinatário

### Fluxos de Dados

1. **Credenciais de Usuário**
   - Conteúdo: Nome de usuário e senha para autenticação
   - Origem: Usuários do sistema (todas as categorias)
   - Destino: Processo Gerenciar Acesso (1.0)

2. **Solicitação de Atendimento**
   - Conteúdo: Descrição do caso, unidade, tipo de necessidade
   - Origem: Coordenador de Unidade
   - Destino: Processo Gerenciar Atendimentos (2.0)

3. **Dados de Atendimentos**
   - Conteúdo: Informações sobre atendimentos em andamento ou concluídos
   - Origem: Armazenamento Atendimentos (D1)
   - Destino: Diversos processos e entidades externas

4. **Atribuição de Tarefa**
   - Conteúdo: Descrição da atividade, responsável, prazo
   - Origem: Processo Gerenciar Tarefas (3.0)
   - Destino: Técnicos da Assessoria

5. **Metadados de Material**
   - Conteúdo: Informações descritivas sobre documentos no repositório
   - Origem: Processo Gerenciar Repositório (4.0)
   - Destino: Armazenamento Materiais (D3)

6. **Parâmetros de Relatório**
   - Conteúdo: Filtros, critérios e formato do relatório
   - Origem: Gestores da Assessoria
   - Destino: Processo Gerar Relatórios (5.0)

7. **Notificações**
   - Conteúdo: Alertas sobre novos atendimentos, tarefas ou prazos
   - Origem: Diversos processos
   - Destino: Sistema de Email do CPS

### Armazenamentos de Dados

1. **Atendimentos (D1)**
   - Conteúdo: Registros completos dos atendimentos
   - Principais atributos: ID, protocolo, tipo, unidade, responsável, status, histórico

2. **Tarefas (D2)**
   - Conteúdo: Atividades relacionadas aos atendimentos
   - Principais atributos: ID, atendimento, título, responsável, prazo, status

3. **Materiais (D3)**
   - Conteúdo: Metadados dos documentos do repositório
   - Principais atributos: ID, título, descrição, categoria, autor, versão, tags

4. **Armazenamento de Arquivos (D4)**
   - Conteúdo: Arquivos físicos dos documentos
   - Principais atributos: Caminho, formato, tamanho, checksum

5. **Configurações de Relatórios (D5)**
   - Conteúdo: Definições de relatórios predefinidos e personalizados
   - Principais atributos: ID, nome, parâmetros, agendamento, formato

## Mapeamento com Requisitos

Os fluxos de dados apresentados atendem aos principais requisitos funcionais do sistema:

1. **RF-01: Controle de Acesso**
   - Processo Gerenciar Acesso (1.0) e seus subprocessos
   - Fluxos: Credenciais de Usuário, Dados de Perfil

2. **RF-02: Gestão de Atendimentos**
   - Processo Gerenciar Atendimentos (2.0) e seus subprocessos
   - Fluxos: Solicitação de Atendimento, Dados de Atendimentos, Notificações

3. **RF-03: Organização do Fluxo de Trabalho**
   - Processo Gerenciar Tarefas (3.0) e seus subprocessos
   - Fluxos: Atribuição de Tarefa, Atualização de Status, Alertas

4. **RF-04: Geração de Relatórios**
   - Processo Gerar Relatórios (5.0) e seus subprocessos
   - Fluxos: Parâmetros de Relatório, Dados Processados, Relatório Formatado

5. **RF-05: Gestão de Informações de Inclusão**
   - Processo Gerenciar Repositório (4.0) e seus subprocessos
   - Fluxos: Metadados de Material, Arquivos, Resultados de Busca

## Considerações de Implementação

1. **Transações e Consistência de Dados**
   - Implementar mecanismos para garantir consistência entre os diversos armazenamentos de dados
   - Utilizar transações para operações que envolvem múltiplos armazenamentos
   - Manter histórico de alterações em entidades críticas como Atendimentos

2. **Tratamento de Falhas de Integração**
   - Implementar estratégias de retry para comunicação com sistemas externos
   - Manter filas para operações assíncronas como envio de notificações
   - Registrar falhas de integração para diagnóstico posterior

3. **Controle de Concorrência**
   - Considerar situações onde múltiplos usuários tentam atualizar os mesmos dados
   - Implementar mecanismos de bloqueio ou versionamento otimista
   - Garantir feedback adequado para conflitos de atualização

4. **Otimização de Consultas**
   - Especial atenção às consultas para geração de relatórios, que podem envolver grandes volumes de dados
   - Considerar materialização de visões para relatórios frequentes
   - Implementar paginação para resultados de busca no repositório

5. **Gestão de Arquivos**
   - Implementar estratégia adequada para armazenamento e versionamento de arquivos
   - Considerar políticas de backup específicas para o repositório de materiais
   - Implementar validação de formatos e verificação de segurança para uploads

6. **Extensibilidade**
   - Projetar o sistema para permitir a adição de novos tipos de relatório
   - Permitir configuração flexível de fluxos de trabalho
   - Considerar mecanismos de plugin para funcionalidades específicas de tipos de atendimento