# Plano de Testes
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Escopo de Testes](#escopo-de-testes)
- [Estratégia de Testes](#estratégia-de-testes)
  - [Níveis de Teste](#níveis-de-teste)
  - [Tipos de Teste](#tipos-de-teste)
- [Ambiente de Testes](#ambiente-de-testes)
- [Critérios de Entrada e Saída](#critérios-de-entrada-e-saída)
- [Casos de Teste](#casos-de-teste)
  - [Autenticação e Controle de Acesso](#autenticação-e-controle-de-acesso)
  - [Gestão de Atendimentos](#gestão-de-atendimentos)
  - [Organização do Fluxo de Trabalho](#organização-do-fluxo-de-trabalho)
  - [Geração de Relatórios](#geração-de-relatórios)
  - [Gestão de Informações de Inclusão](#gestão-de-informações-de-inclusão)
- [Cronograma de Testes](#cronograma-de-testes)
- [Recursos Necessários](#recursos-necessários)
- [Riscos e Mitigação](#riscos-e-mitigação)
- [Documentação de Testes](#documentação-de-testes)
- [Aprovações](#aprovações)

## Introdução

Este Plano de Testes descreve a abordagem sistemática para verificar e validar o Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza (SGAI). O documento define a estratégia, recursos, cronograma e procedimentos necessários para garantir que o software atenda aos requisitos funcionais e não funcionais especificados nos documentos de requisitos.

O plano abrange todos os níveis de testes, desde testes unitários até testes de aceitação, e define os critérios para considerar o sistema apto para implantação em ambiente de produção.

### Objetivos

Os principais objetivos deste plano de testes são:

1. Verificar se todas as funcionalidades descritas nos requisitos funcionais estão corretamente implementadas
2. Validar a conformidade do sistema com os requisitos não funcionais
3. Identificar e documentar defeitos para correção antes da implantação
4. Verificar a integração adequada entre os componentes do sistema
5. Confirmar que o sistema é utilizável pelos diferentes perfis de usuário
6. Garantir a conformidade com as diretrizes de acessibilidade WCAG 2.1 nível AA

## Escopo de Testes

### Itens a serem testados:

1. **Módulo de Autenticação e Controle de Acesso**
   - Login no Sistema
   - Gerenciamento de Permissões
   - Recuperação de Senha
   - Gerenciamento de Usuários

2. **Módulo de Gestão de Atendimentos**
   - Registro de Atendimentos
   - Categorização de Atendimentos
   - Acompanhamento de Status
   - Histórico de Atendimentos
   - Atribuição de Responsáveis

3. **Módulo de Organização do Fluxo de Trabalho**
   - Dashboard Personalizado
   - Gestão de Tarefas
   - Notificações e Alertas
   - Configuração de Fluxos de Trabalho

4. **Módulo de Geração de Relatórios**
   - Relatórios Predefinidos
   - Gerador de Relatórios Customizados
   - Exportação de Dados
   - Visualizações Gráficas

5. **Módulo de Gestão de Informações de Inclusão**
   - Cadastro de Unidades
   - Repositório de Materiais
   - Cadastro de Profissionais Especializados
   - Registro de Adaptações Curriculares

### Fora do Escopo:

1. Testes de sistemas externos integrados (como sistema de autenticação do CPS)
2. Testes de segurança invasivos (penetration testing)
3. Testes de recuperação de desastres
4. Testes de migração de dados legados (será escopo de um plano específico)

## Estratégia de Testes

### Níveis de Teste

#### 1. Testes Unitários
- **Escopo**: Testar componentes individuais do código-fonte
- **Responsável**: Equipe de desenvolvimento
- **Ferramenta**: JUnit/Jest (dependendo da stack tecnológica escolhida)
- **Automação**: 100% dos testes unitários devem ser automatizados
- **Cobertura alvo**: Mínimo de 80% de cobertura de código

#### 2. Testes de Integração
- **Escopo**: Testar a interação entre componentes internos e externos
- **Responsável**: Equipe de desenvolvimento e QA
- **Ferramenta**: Postman/REST Assured para APIs
- **Automação**: 80% dos testes de integração devem ser automatizados
- **Foco**: Integração entre camadas e serviços

#### 3. Testes de Sistema
- **Escopo**: Testar o sistema completo conforme requisitos
- **Responsável**: Equipe de QA
- **Ferramenta**: Selenium/Cypress para UI, JMeter para performance
- **Automação**: 70% dos testes funcionais básicos automatizados
- **Foco**: Fluxos de negócio end-to-end

#### 4. Testes de Aceitação
- **Escopo**: Validação do sistema pelo usuário final
- **Responsável**: Equipe da Assessoria de Inclusão do CPS
- **Abordagem**: Cenários de uso real em ambiente de homologação
- **Foco**: Usabilidade, fluxos de trabalho e valor para o negócio

### Tipos de Teste

#### 1. Testes Funcionais
- Verificação de todas as funcionalidades especificadas nos requisitos
- Testes baseados em casos de uso e histórias de usuário
- Testes de fluxos alternativos e condições de erro

#### 2. Testes de Usabilidade
- Avaliação da interface do usuário conforme RNF-02
- Testes com diferentes perfis de usuário
- Avaliação de feedback e mensagens do sistema

#### 3. Testes de Acessibilidade
- Conformidade com WCAG 2.1 nível AA
- Testes com ferramentas automáticas (WAVE, Axe)
- Testes manuais com tecnologias assistivas (leitores de tela)

#### 4. Testes de Performance
- Tempo de resposta (RNF-03.1)
- Capacidade de usuários simultâneos (RNF-03.2)
- Comportamento sob carga
- Escalabilidade (AQ-02.1)

#### 5. Testes de Segurança
- Autenticação e autorização
- Proteção de dados sensíveis
- Validação de entrada
- Gestão de sessão

#### 6. Testes de Compatibilidade
- Navegadores: Chrome, Firefox, Edge e Safari em suas versões atuais
- Dispositivos: Compatível com computadores desktop, notebooks e tablets
- Sistemas operacionais: Windows, Linux e macOS

## Ambiente de Testes

### Ambiente de Desenvolvimento
- **Propósito**: Testes unitários e de integração durante o desenvolvimento
- **Características**:
  - Configuração simplificada em máquinas locais ou compartilhadas
  - Banco de dados local com dados de teste
  - Mocks para serviços externos
  - Depuração ativada
  - Docker para simular o ambiente completo

### Ambiente de QA
- **Propósito**: Testes de sistema e regressão
- **Características**:
  - Servidor dedicado com configuração similar à produção (escala reduzida)
  - Banco de dados isolado com dados representativos
  - Integração com versões de teste de serviços externos
  - Ferramentas de monitoramento e registro de logs

### Ambiente de Homologação
- **Propósito**: Testes de aceitação do usuário (UAT)
- **Características**:
  - Configuração idêntica à produção
  - Dados anonimizados baseados em dados reais
  - Acesso por parte dos usuários finais (equipe da Assessoria)
  - Monitoramento de comportamento e performance

### Infraestrutura Necessária
- Servidores para cada ambiente
- Ferramentas de automação de testes
- Sistema de controle de versão para artefatos de teste
- Sistema de gerenciamento de defeitos
- Ferramentas de monitoramento e análise de performance

## Critérios de Entrada e Saída

### Critérios de Entrada

#### Para Testes Unitários
- Código-fonte implementado
- Build do componente bem-sucedido
- Revisão de código concluída

#### Para Testes de Integração
- Testes unitários concluídos com sucesso
- Componentes individuais testados
- Ambiente de integração configurado

#### Para Testes de Sistema
- Testes de integração concluídos com sucesso
- Sistema completamente integrado
- Ambiente de QA configurado e disponível
- Dados de teste preparados

#### Para Testes de Aceitação
- Testes de sistema concluídos com sucesso
- Defeitos críticos e de alta prioridade corrigidos
- Ambiente de homologação preparado
- Usuários finais treinados para testes

### Critérios de Saída

#### Para Testes Unitários
- 100% dos testes executados
- Mínimo de 80% de cobertura de código
- Zero defeitos críticos ou bloqueadores

#### Para Testes de Integração
- 100% dos casos de teste executados
- Zero defeitos críticos
- Máximo de 5 defeitos de alta prioridade

#### Para Testes de Sistema
- 100% dos casos de teste executados
- Zero defeitos críticos
- Máximo de 10 defeitos de alta prioridade não resolvidos
- Todos os fluxos principais funcionando corretamente

#### Para Testes de Aceitação
- Mínimo de 90% dos casos de teste de aceitação aprovados
- Zero defeitos críticos
- Aceite formal da equipe da Assessoria

## Casos de Teste

Esta seção apresenta uma amostra representativa dos principais casos de teste que serão executados. Os casos detalhados serão mantidos em um sistema de gerenciamento de testes.

### Autenticação e Controle de Acesso

#### CT-001: Login com credenciais válidas
- **Descrição**: Verificar se o usuário consegue fazer login com credenciais válidas
- **Pré-condição**: Usuário cadastrado no sistema
- **Dados de teste**: Email: usuario@etec.sp.gov.br, Senha: SenhaValida123
- **Passos**:
  1. Acessar a página de login
  2. Preencher o campo de email
  3. Preencher o campo de senha
  4. Clicar no botão "Entrar"
- **Resultado esperado**: Usuário autenticado e redirecionado para dashboard
- **Prioridade**: Alta
- **Automatizado**: Sim

#### CT-002: Login com credenciais inválidas
- **Descrição**: Verificar se o sistema impede acesso com credenciais inválidas
- **Pré-condição**: N/A
- **Dados de teste**: Email: usuario@etec.sp.gov.br, Senha: SenhaInvalida
- **Passos**:
  1. Acessar a página de login
  2. Preencher o campo de email
  3. Preencher o campo de senha com senha incorreta
  4. Clicar no botão "Entrar"
- **Resultado esperado**: Mensagem de erro informando que as credenciais são inválidas
- **Prioridade**: Alta
- **Automatizado**: Sim

#### CT-003: Recuperação de senha
- **Descrição**: Verificar se o processo de recuperação de senha funciona corretamente
- **Pré-condição**: Usuário cadastrado no sistema
- **Dados de teste**: Email: usuario@etec.sp.gov.br
- **Passos**:
  1. Acessar a página de login
  2. Clicar em "Esqueci minha senha"
  3. Informar o email cadastrado
  4. Clicar em "Enviar link"
- **Resultado esperado**: Sistema confirma envio de email e instruções
- **Prioridade**: Alta
- **Automatizado**: Parcial

#### CT-004: Verificação de permissões por perfil
- **Descrição**: Verificar se o sistema aplica corretamente as permissões de acordo com o perfil
- **Pré-condição**: Usuário logado com perfil específico
- **Dados de teste**: Usuários com diferentes perfis (Administrador, Gestor, Técnico, Coordenador)
- **Passos**:
  1. Fazer login com usuário de perfil específico
  2. Verificar as opções disponíveis no menu
  3. Tentar acessar funcionalidades restritas via URL direta
- **Resultado esperado**: Acesso permitido apenas a funcionalidades autorizadas para o perfil
- **Prioridade**: Alta
- **Automatizado**: Parcial

### Gestão de Atendimentos

#### CT-005: Registro de novo atendimento
- **Descrição**: Verificar se é possível criar um novo atendimento
- **Pré-condição**: Usuário logado com permissão para criar atendimentos
- **Dados de teste**: Dados de um novo atendimento (unidade, tipo de necessidade, descrição)
- **Passos**:
  1. Acessar o módulo de atendimentos
  2. Clicar em "Novo Atendimento"
  3. Preencher os dados obrigatórios
  4. Clicar em "Salvar"
- **Resultado esperado**: Atendimento criado com número de protocolo gerado automaticamente
- **Prioridade**: Alta
- **Automatizado**: Sim

#### CT-006: Categorização de atendimento
- **Descrição**: Verificar se é possível categorizar corretamente um atendimento
- **Pré-condição**: Atendimento existente no sistema
- **Dados de teste**: ID de atendimento, tipo de necessidade
- **Passos**:
  1. Acessar o atendimento existente
  2. Editar o campo "Tipo de Necessidade"
  3. Salvar alterações
- **Resultado esperado**: Tipo de necessidade atualizado e registrado no histórico
- **Prioridade**: Média
- **Automatizado**: Sim

#### CT-007: Atualização de status de atendimento
- **Descrição**: Verificar se é possível atualizar o status de um atendimento
- **Pré-condição**: Atendimento existente no sistema
- **Dados de teste**: ID de atendimento, novo status
- **Passos**:
  1. Acessar o atendimento existente
  2. Alterar o status para uma transição válida
  3. Adicionar comentário (opcional)
  4. Salvar alterações
- **Resultado esperado**: Status atualizado e alteração registrada no histórico
- **Prioridade**: Alta
- **Automatizado**: Sim

#### CT-008: Visualização de histórico de atendimento
- **Descrição**: Verificar se o histórico de alterações de um atendimento é exibido corretamente
- **Pré-condição**: Atendimento existente com múltiplas alterações
- **Dados de teste**: ID de atendimento
- **Passos**:
  1. Acessar o atendimento existente
  2. Navegar para a aba "Histórico"
- **Resultado esperado**: Lista cronológica de todas as alterações, incluindo data/hora, usuário e detalhes
- **Prioridade**: Média
- **Automatizado**: Sim

### Organização do Fluxo de Trabalho

#### CT-009: Criação de tarefa associada a atendimento
- **Descrição**: Verificar se é possível criar uma tarefa vinculada a um atendimento
- **Pré-condição**: Atendimento existente no sistema
- **Dados de teste**: ID de atendimento, dados da tarefa (título, descrição, responsável, prazo)
- **Passos**:
  1. Acessar o atendimento existente
  2. Clicar em "Nova Tarefa"
  3. Preencher os dados da tarefa
  4. Clicar em "Salvar"
- **Resultado esperado**: Tarefa criada e vinculada ao atendimento, visível na lista de tarefas
- **Prioridade**: Alta
- **Automatizado**: Sim

#### CT-010: Dashboard personalizado por perfil
- **Descrição**: Verificar se o dashboard exibe informações adequadas conforme o perfil do usuário
- **Pré-condição**: Usuários cadastrados com diferentes perfis
- **Dados de teste**: Credenciais de diferentes perfis de usuário
- **Passos**:
  1. Fazer login com usuário de perfil específico
  2. Acessar o dashboard principal
  3. Verificar os componentes e informações exibidas
- **Resultado esperado**: Dashboard com informações relevantes para o perfil específico
- **Prioridade**: Média
- **Automatizado**: Parcial

#### CT-011: Sistema de notificações
- **Descrição**: Verificar se as notificações são enviadas corretamente aos usuários
- **Pré-condição**: Evento que gera notificação (nova tarefa, prazo próximo)
- **Dados de teste**: Diversos cenários que geram notificações
- **Passos**:
  1. Gerar evento que dispara notificação (ex: atribuir tarefa)
  2. Verificar se o destinatário recebe a notificação
  3. Verificar se o conteúdo da notificação é correto
- **Resultado esperado**: Notificação entregue corretamente com conteúdo adequado
- **Prioridade**: Média
- **Automatizado**: Parcial

### Geração de Relatórios

#### CT-012: Geração de relatório predefinido
- **Descrição**: Verificar se os relatórios predefinidos são gerados corretamente
- **Pré-condição**: Usuário com permissão para acessar relatórios, dados suficientes no sistema
- **Dados de teste**: Tipo de relatório, parâmetros (período, filtros)
- **Passos**:
  1. Acessar o módulo de relatórios
  2. Selecionar um relatório predefinido
  3. Configurar parâmetros (período, filtros)
  4. Clicar em "Gerar Relatório"
- **Resultado esperado**: Relatório gerado corretamente com dados precisos
- **Prioridade**: Alta
- **Automatizado**: Parcial

#### CT-013: Criação de relatório personalizado
- **Descrição**: Verificar se é possível criar e salvar um relatório personalizado
- **Pré-condição**: Usuário com permissão para criar relatórios
- **Dados de teste**: Campos para o relatório, filtros, agrupamentos
- **Passos**:
  1. Acessar o gerador de relatórios
  2. Selecionar campos desejados
  3. Configurar filtros e agrupamentos
  4. Visualizar prévia
  5. Salvar configuração
- **Resultado esperado**: Relatório personalizado salvo e disponível para uso posterior
- **Prioridade**: Média
- **Automatizado**: Parcial

#### CT-014: Exportação de relatório
- **Descrição**: Verificar se é possível exportar relatórios em diferentes formatos
- **Pré-condição**: Relatório gerado no sistema
- **Dados de teste**: Relatório existente, formato de exportação (PDF, Excel, CSV)
- **Passos**:
  1. Gerar um relatório
  2. Clicar em "Exportar"
  3. Selecionar formato desejado
  4. Iniciar download
- **Resultado esperado**: Arquivo exportado no formato correto com dados precisos
- **Prioridade**: Média
- **Automatizado**: Sim

### Gestão de Informações de Inclusão

#### CT-015: Upload de material para repositório
- **Descrição**: Verificar se é possível fazer upload de material para o repositório
- **Pré-condição**: Usuário logado com permissão para gerenciar materiais
- **Dados de teste**: Arquivo para upload, metadados (título, descrição, categoria, tags)
- **Passos**:
  1. Acessar o repositório de materiais
  2. Clicar em "Novo Material"
  3. Fazer upload de arquivo
  4. Preencher metadados
  5. Clicar em "Salvar"
- **Resultado esperado**: Material salvo e disponível no repositório
- **Prioridade**: Média
- **Automatizado**: Parcial

#### CT-016: Busca de material por palavras-chave
- **Descrição**: Verificar a funcionalidade de busca no repositório de materiais
- **Pré-condição**: Materiais cadastrados no repositório com metadados
- **Dados de teste**: Termos de busca relevantes
- **Passos**:
  1. Acessar o repositório de materiais
  2. Inserir termo de busca
  3. Executar a busca
- **Resultado esperado**: Lista de materiais que correspondem ao termo buscado
- **Prioridade**: Média
- **Automatizado**: Sim

## Cronograma de Testes

| Fase | Duração | Marcos |
|------|---------|--------|
| **Planejamento de Testes** | 2 semanas | Plano de testes aprovado, Casos de teste identificados |
| **Preparação de Ambiente** | 1 semana | Ambientes configurados, Dados de teste preparados |
| **Testes Unitários** | Contínuo | Integração com CI/CD, Cobertura mínima alcançada |
| **Testes de Integração** | 3 semanas | Interfaces entre componentes verificadas |
| **Testes de Sistema** | 4 semanas | Funcionalidades completas testadas, Correções aplicadas |
| **Testes de Performance** | 2 semanas | Métricas de desempenho verificadas, Otimizações implementadas |
| **Testes de Segurança** | 2 semanas | Vulnerabilidades identificadas e corrigidas |
| **Testes de Aceitação** | 2 semanas | Validação pelo usuário, Aceite formal |
| **Testes de Regressão** | 1 semana | Garantia de estabilidade após correções |

## Recursos Necessários

### Equipe

| Função | Quantidade | Responsabilidades |
|--------|------------|-------------------|
| Gerente de QA | 1 | Coordenar atividades de teste, reportar status |
| Analistas de QA | 3 | Executar casos de teste, documentar resultados |
| Desenvolvedores | 2 | Testes unitários, suporte à automação |
| Especialista em Automação | 1 | Implementar frameworks de automação |
| Especialista em Performance | 1 | Testes de carga e desempenho |
| Representantes do Usuário | 2 | Testes de aceitação, validação de usabilidade |

### Ferramentas

| Categoria | Ferramentas |
|-----------|-------------|
| Gerenciamento de Testes | JIRA, TestRail |
| Automação de UI | Selenium, Cypress |
| Automação de API | Postman, RestAssured |
| Testes Unitários | JUnit, Jest |
| Testes de Performance | JMeter, Gatling |
| Testes de Acessibilidade | WAVE, Axe |
| CI/CD | Jenkins, GitHub Actions |
| Monitoramento | Grafana, Prometheus |

## Riscos e Mitigação

| Risco | Probabilidade | Impacto | Estratégia de Mitigação |
|-------|--------------|---------|-------------------------|
| Atrasos no cronograma de testes | Média | Alto | Priorização de casos de teste, Parallelização de atividades |
| Ambientes de teste instáveis | Média | Alto | Procedimento de configuração automatizado, Backups frequentes |
| Testes incompletos por falta de dados | Alta | Médio | Geração de dados de teste representativos, Mocks e stubs |
| Falha em detectar problemas críticos | Baixa | Crítico | Revisão por pares dos casos de teste, Cobertura de cenários críticos |
| Indisponibilidade de usuários para testes de aceitação | Média | Alto | Agendamento antecipado, Treinamento de backups |
| Integração com sistemas externos não disponível | Alta | Alto | Mocks de serviços externos, Ambientes de teste isolados |

## Documentação de Testes

### Artefatos a serem produzidos

1. **Plano de Testes** (este documento)
   - Estratégia geral e abordagem de testes

2. **Casos de Teste Detalhados**
   - Para cada módulo, documentados no sistema de gerenciamento de testes

3. **Scripts de Automação**
   - Código fonte dos testes automatizados, mantido no repositório de código

4. **Dados de Teste**
   - Conjunto de dados para execução dos testes, incluindo dados de carga

5. **Relatórios de Execução**
   - Resultados da execução dos testes, incluindo métricas e status

6. **Registro de Defeitos**
   - Documentação detalhada de cada defeito encontrado

7. **Relatório Final de Testes**
   - Sumário executivo dos resultados, incluindo recomendações

### Gestão de Defeitos

Os defeitos serão classificados segundo a seguinte matriz:

| Severidade | Definição |
|------------|-----------|
| **Crítica** | Impede o uso do sistema ou módulo; Comprometimento de dados; Problemas de segurança críticos |
| **Alta** | Funcionalidade principal com falha grave; Alternativas complexas ou inexistentes |
| **Média** | Falha em funcionalidade com alternativa razoável; Problemas de usabilidade impactantes |
| **Baixa** | Problemas menores de UI; Falhas em funcionalidades não essenciais; Melhorias estéticas |

O ciclo de vida dos defeitos seguirá o fluxo: Aberto → Atribuído → Em Análise → Em Desenvolvimento → Pronto para Teste → Fechado/Rejeitado

## Aprovações

| Nome | Papel | Assinatura | Data |
|------|------|-----------|------|
| [Nome] | Gerente de Projeto | _____________ | __/__/____ |
| [Nome] | Gerente de QA | _____________ | __/__/____ |
| [Nome] | Representante do CPS | _____________ | __/__/____ |
| [Nome] | Coordenador da Assessoria | _____________ | __/__/____ |