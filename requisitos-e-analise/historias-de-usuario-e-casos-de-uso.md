# Histórias de Usuário e Casos de Uso
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Histórias de Usuário](#histórias-de-usuário)
- [Casos de Uso](#casos-de-uso)
- [Diagrama de Casos de Uso](#diagrama-de-casos-de-uso)
- [Especificação dos Casos de Uso](#especificação-dos-casos-de-uso)
- [Matriz de Rastreabilidade](#matriz-de-rastreabilidade)

## Introdução

Este documento apresenta as histórias de usuário e os casos de uso do Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza. As histórias de usuário capturam as necessidades dos diferentes perfis de usuários do sistema de forma concisa, enquanto os casos de uso detalham as interações entre os usuários e o sistema para realizar objetivos específicos.

## Histórias de Usuário

### Módulo: Autenticação e Controle de Acesso

#### HU-01: Acesso ao Sistema
**Como** um usuário da Assessoria de Inclusão,  
**Eu quero** acessar o sistema usando minhas credenciais institucionais,  
**Para que** eu possa utilizar as funcionalidades de acordo com meu perfil de acesso.

#### HU-02: Gerenciamento de Usuários
**Como** um administrador do sistema,  
**Eu quero** criar, editar, desativar e reativar contas de usuários,  
**Para que** eu possa gerenciar quem tem acesso ao sistema e com quais permissões.

#### HU-03: Recuperação de Senha
**Como** um usuário do sistema,  
**Eu quero** recuperar minha senha quando a esquecer,  
**Para que** eu possa retomar meu acesso ao sistema sem depender do administrador.

### Módulo: Gestão de Atendimentos

#### HU-04: Registro de Atendimentos
**Como** um técnico da Assessoria de Inclusão,  
**Eu quero** registrar detalhadamente os atendimentos realizados,  
**Para que** haja documentação completa de todas as intervenções realizadas.

#### HU-05: Categorização de Atendimentos
**Como** um técnico da Assessoria de Inclusão,  
**Eu quero** categorizar os atendimentos por tipo de necessidade,  
**Para que** eu possa organizar e filtrar os casos de acordo com características específicas.

#### HU-06: Acompanhamento de Status
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** acompanhar o status de todos os atendimentos em andamento,  
**Para que** eu possa monitorar o progresso e garantir que nenhum caso fique sem resolução.

#### HU-07: Visualização de Histórico
**Como** um técnico da Assessoria de Inclusão,  
**Eu quero** visualizar o histórico completo de cada atendimento,  
**Para que** eu possa entender todo o contexto e ações anteriores relacionadas ao caso.

#### HU-08: Solicitação de Atendimento
**Como** um coordenador de unidade,  
**Eu quero** registrar solicitações de atendimento à Assessoria de Inclusão,  
**Para que** eu possa comunicar as necessidades específicas de minha unidade.

### Módulo: Organização do Fluxo de Trabalho

#### HU-09: Visualização de Dashboard
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** ter acesso a dashboards personalizados,  
**Para que** eu possa rapidamente visualizar informações relevantes para minha função.

#### HU-10: Gestão de Tarefas
**Como** um técnico da Assessoria de Inclusão,  
**Eu quero** criar e gerenciar tarefas relacionadas aos atendimentos,  
**Para que** eu possa organizar meu trabalho e não esquecer atividades importantes.

#### HU-11: Recebimento de Notificações
**Como** um usuário do sistema,  
**Eu quero** receber notificações sobre prazos e atualizações,  
**Para que** eu possa estar ciente de eventos importantes e agir tempestivamente.

#### HU-12: Atribuição de Responsabilidades
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** atribuir responsáveis para diferentes atendimentos e tarefas,  
**Para que** haja clareza sobre quem deve resolver cada questão.

### Módulo: Geração de Relatórios

#### HU-13: Relatórios Predefinidos
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** gerar relatórios predefinidos sobre as atividades realizadas,  
**Para que** eu possa avaliar a performance da equipe e reportar aos superiores.

#### HU-14: Relatórios Personalizados
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** criar relatórios personalizados selecionando campos e filtros específicos,  
**Para que** eu possa obter exatamente as informações que necessito para análises específicas.

#### HU-15: Exportação de Relatórios
**Como** um usuário do sistema,  
**Eu quero** exportar relatórios em diferentes formatos (PDF, Excel, CSV),  
**Para que** eu possa utilizar os dados em outros contextos ou compartilhar com pessoas sem acesso ao sistema.

#### HU-16: Visualizações Gráficas
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** visualizar dados estatísticos em formato gráfico,  
**Para que** eu possa compreender tendências e padrões de forma mais intuitiva.

### Módulo: Gestão de Informações de Inclusão

#### HU-17: Cadastro de Unidades
**Como** um administrador ou gestor da Assessoria de Inclusão,  
**Eu quero** manter um cadastro atualizado das unidades do CPS e suas características de acessibilidade,  
**Para que** eu possa ter uma visão clara da infraestrutura de inclusão disponível.

#### HU-18: Repositório de Materiais
**Como** um técnico da Assessoria de Inclusão,  
**Eu quero** acessar e compartilhar materiais e recursos sobre inclusão,  
**Para que** eu possa apoiar as unidades com conhecimento especializado.

#### HU-19: Cadastro de Profissionais
**Como** um gestor da Assessoria de Inclusão,  
**Eu quero** cadastrar profissionais especializados em diferentes áreas da inclusão,  
**Para que** eu possa identificar recursos humanos capacitados para demandas específicas.

#### HU-20: Registro de Adaptações Curriculares
**Como** um técnico da Assessoria de Inclusão,  
**Eu quero** registrar adaptações curriculares implementadas,  
**Para que** elas possam servir de referência para casos similares no futuro.

## Casos de Uso

### Diagrama de Casos de Uso

```
+-------------------------------------------------------------+
|                     Sistema de Gestão AI                    |
+-------------------------------------------------------------+
                             ^
                             |
         +------------------+|+------------------+
         |                   |                   |
+--------v-------+  +--------v-------+  +--------v-------+
| Administrador  |  |    Gestor      |  |    Técnico     |
|   do Sistema   |  |  da Assessoria |  |  da Assessoria |
+----------------+  +----------------+  +----------------+
    |       |            |     |              |      |
    |       |            |     |              |      |
    |  +----v----+   +---v-----v--+       +---v------v--+
    +->| Gerenciar|   | Gerenciar  |       | Registrar   |
    |  | Usuários |   | Relatórios |       | Atendimentos|
    |  +---------+    +------------+       +------------+
    |                      |                    |
    |                      |                    |
    |  +---------+    +----v-------+       +----v-------+
    +->| Config.  |    | Visualizar |       | Atualizar  |
       | Sistema  |    | Dashboard  |       | Status     |
       +---------+    +------------+       +------------+
                           |                    |
                           |                    |
                      +----v-------+       +----v-------+
                      | Gerenciar  |       | Acessar    |
                      | Tarefas    |       | Repositório |
                      +------------+       +------------+
                           ^                    ^
                           |                    |
                     +-----v--------------------v------+
                     |     Coordenador de Unidade      |
                     +--------------------------------+
```

## Especificação dos Casos de Uso

### UC-01: Autenticar no Sistema

**Atores:** Todos os usuários (Administrador, Gestor, Técnico, Coordenador)

**Descrição:** Este caso de uso descreve o processo pelo qual os usuários se autenticam no sistema.

**Pré-condições:**
- O usuário possui credenciais válidas no sistema.
- O sistema está disponível e acessível.

**Fluxo Básico:**
1. O usuário acessa a página de login do sistema.
2. O sistema apresenta o formulário de login.
3. O usuário insere seu nome de usuário e senha.
4. O usuário clica no botão "Entrar".
5. O sistema valida as credenciais.
6. O sistema direciona o usuário para a página inicial correspondente ao seu perfil.

**Fluxos Alternativos:**
- **A1: Credenciais Inválidas**
  1. No passo 5, se as credenciais forem inválidas, o sistema exibe uma mensagem de erro.
  2. O usuário é orientado a tentar novamente ou recuperar sua senha.

- **A2: Esqueceu a Senha**
  1. No passo 2, o usuário clica em "Esqueci minha senha".
  2. O sistema exibe o formulário de recuperação de senha.
  3. O usuário insere seu e-mail institucional.
  4. O sistema envia um link para redefinição de senha.

**Pós-condições:**
- O usuário está autenticado e tem acesso às funcionalidades de acordo com seu perfil.

### UC-02: Registrar Atendimento

**Atores:** Técnico da Assessoria, Gestor da Assessoria

**Descrição:** Este caso de uso descreve o processo de registro de um novo atendimento no sistema.

**Pré-condições:**
- O usuário está autenticado com perfil de Técnico ou Gestor da Assessoria.

**Fluxo Básico:**
1. O usuário acessa o módulo de Gestão de Atendimentos.
2. O usuário seleciona a opção "Novo Atendimento".
3. O sistema apresenta o formulário de registro.
4. O usuário preenche os dados obrigatórios (data, tipo, unidade, descrição).
5. O usuário preenche os dados complementares (se necessário).
6. O usuário seleciona a categoria do atendimento.
7. O usuário clica em "Salvar".
8. O sistema valida as informações.
9. O sistema registra o atendimento e gera um número de protocolo.
10. O sistema exibe mensagem de confirmação e o número de protocolo.

**Fluxos Alternativos:**
- **A1: Dados Incompletos/Inválidos**
  1. No passo 8, se houver dados incompletos ou inválidos, o sistema destaca os campos com problema.
  2. O usuário corrige os dados e tenta salvar novamente.

- **A2: Registro como Rascunho**
  1. No passo 7, o usuário clica em "Salvar como Rascunho".
  2. O sistema salva o atendimento com status de rascunho.
  
- **A3: Anexar Documentos**
  1. Entre os passos 6 e 7, o usuário seleciona "Anexar Documentos".
  2. O sistema apresenta opções para upload de arquivos.
  3. O usuário seleciona e anexa arquivos relacionados ao atendimento.
  4. O usuário retorna ao fluxo principal.

**Pós-condições:**
- O atendimento está registrado no sistema com um número de protocolo.
- O atendimento aparece na lista de atendimentos em andamento.

### UC-03: Gerar Relatório

**Atores:** Gestor da Assessoria, Administrador do Sistema

**Descrição:** Este caso de uso descreve o processo de geração de relatórios sobre os atendimentos e atividades.

**Pré-condições:**
- O usuário está autenticado com perfil de Gestor ou Administrador.
- Existem dados registrados no sistema para o período desejado.

**Fluxo Básico:**
1. O usuário acessa o módulo de Relatórios.
2. O usuário seleciona o tipo de relatório desejado.
3. O usuário define os parâmetros (período, unidades, categorias, etc.).
4. O usuário clica em "Gerar Relatório".
5. O sistema processa a solicitação e apresenta o relatório na tela.
6. O usuário visualiza o relatório.
7. O usuário seleciona "Exportar" e escolhe o formato desejado.
8. O sistema gera o arquivo de exportação.
9. O usuário faz download do arquivo.

**Fluxos Alternativos:**
- **A1: Sem Dados para o Período**
  1. No passo 5, se não houver dados para os parâmetros selecionados, o sistema exibe uma mensagem informativa.
  2. O usuário ajusta os parâmetros e tenta novamente.

- **A2: Relatório Personalizado**
  1. No passo 2, o usuário seleciona "Relatório Personalizado".
  2. O sistema apresenta opções avançadas de configuração.
  3. O usuário seleciona campos, filtros, agrupamentos e ordenações.
  4. O usuário retorna ao fluxo principal no passo 4.

**Pós-condições:**
- O relatório é gerado conforme os parâmetros especificados.
- Se solicitado, um arquivo de exportação é disponibilizado para download.

### UC-04: Atribuir Responsável por Atendimento

**Atores:** Gestor da Assessoria

**Descrição:** Este caso de uso descreve o processo de atribuição de um técnico como responsável por um atendimento.

**Pré-condições:**
- O usuário está autenticado com perfil de Gestor da Assessoria.
- O atendimento está registrado no sistema.

**Fluxo Básico:**
1. O usuário acessa a lista de atendimentos.
2. O usuário seleciona o atendimento desejado.
3. O usuário clica na opção "Atribuir Responsável".
4. O sistema apresenta a lista de técnicos disponíveis.
5. O usuário seleciona o técnico responsável.
6. O usuário opcionalmente adiciona comentários sobre a atribuição.
7. O usuário clica em "Confirmar".
8. O sistema registra a atribuição e atualiza o histórico do atendimento.
9. O sistema notifica o técnico selecionado sobre a nova responsabilidade.

**Fluxos Alternativos:**
- **A1: Reatribuição**
  1. No passo 3, se o atendimento já possui um responsável, o sistema alerta o usuário.
  2. O usuário confirma a substituição do responsável.
  3. O sistema retorna ao fluxo principal no passo 4.

**Pós-condições:**
- O atendimento tem um técnico responsável atribuído.
- O técnico recebe notificação sobre a nova atribuição.
- O histórico do atendimento registra a atribuição/reatribuição.

### UC-05: Gerenciar Repositório de Materiais

**Atores:** Técnico da Assessoria, Gestor da Assessoria, Administrador

**Descrição:** Este caso de uso descreve o processo de gerenciamento do repositório de materiais e recursos sobre inclusão.

**Pré-condições:**
- O usuário está autenticado com perfil adequado.

**Fluxo Básico:**
1. O usuário acessa o módulo de Repositório de Materiais.
2. O sistema exibe a lista de materiais disponíveis com opções de filtro e busca.
3. O usuário seleciona "Adicionar Novo Material".
4. O sistema exibe o formulário de cadastro de material.
5. O usuário preenche os dados (título, descrição, categoria, tags).
6. O usuário faz upload do arquivo.
7. O usuário clica em "Salvar".
8. O sistema valida as informações e armazena o material.
9. O sistema exibe confirmação do cadastro.

**Fluxos Alternativos:**
- **A1: Editar Material Existente**
  1. No passo 2, o usuário seleciona um material existente e clica em "Editar".
  2. O sistema apresenta o formulário preenchido com os dados atuais.
  3. O usuário realiza as alterações necessárias.
  4. O usuário clica em "Atualizar".
  5. O sistema atualiza as informações do material.

- **A2: Remover Material**
  1. No passo 2, o usuário seleciona um material existente e clica em "Remover".
  2. O sistema solicita confirmação da ação.
  3. O usuário confirma a remoção.
  4. O sistema marca o material como inativo (sem excluí-lo fisicamente).

**Pós-condições:**
- O material está disponível no repositório para consulta (no caso de adição).
- O material foi atualizado ou removido conforme solicitado.

## Matriz de Rastreabilidade

A matriz abaixo relaciona as histórias de usuário com os requisitos funcionais definidos no SRS:

| História de Usuário | Requisitos Funcionais |
|---------------------|------------------------|
| HU-01: Acesso ao Sistema | RF-01.1: Login no Sistema |
| HU-02: Gerenciamento de Usuários | RF-01.4: Gerenciamento de Usuários |
| HU-03: Recuperação de Senha | RF-01.3: Recuperação de Senha |
| HU-04: Registro de Atendimentos | RF-02.1: Registro de Atendimentos |
| HU-05: Categorização de Atendimentos | RF-02.2: Categorização de Atendimentos |
| HU-06: Acompanhamento de Status | RF-02.3: Acompanhamento de Status |
| HU-07: Visualização de Histórico | RF-02.4: Histórico de Atendimentos |
| HU-09: Visualização de Dashboard | RF-03.1: Dashboard Personalizado |
| HU-10: Gestão de Tarefas | RF-03.2: Gestão de Tarefas |
| HU-11: Recebimento de Notificações | RF-03.3: Notificações e Alertas |
| HU-12: Atribuição de Responsabilidades | RF-02.5: Atribuição de Responsáveis |
| HU-13: Relatórios Predefinidos | RF-04.1: Relatórios Predefinidos |
| HU-14: Relatórios Personalizados | RF-04.2: Gerador de Relatórios Customizados |
| HU-15: Exportação de Relatórios | RF-04.3: Exportação de Dados |
| HU-16: Visualizações Gráficas | RF-04.4: Visualizações Gráficas |
| HU-17: Cadastro de Unidades | RF-05.1: Cadastro de Unidades |
| HU-18: Repositório de Materiais | RF-05.2: Repositório de Materiais |
| HU-19: Cadastro de Profissionais | RF-05.3: Cadastro de Profissionais Especializados |
| HU-20: Registro de Adaptações Curriculares | RF-05.4: Registro de Adaptações Curriculares |