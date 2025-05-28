# Especificação de Requisitos de Software (SRS)
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Controle de Versão

| Versão | Data | Descrição | Autor |
|--------|------|-----------|-------|
| 1.0 | 28/05/2025 | Versão inicial | [Nome do Autor] |

## Sumário
- [Introdução](#introdução)
- [Descrição Geral](#descrição-geral)
- [Requisitos Funcionais](#requisitos-funcionais)
- [Requisitos Não Funcionais](#requisitos-não-funcionais)
- [Requisitos de Interface](#requisitos-de-interface)
- [Requisitos de Dados](#requisitos-de-dados)
- [Atributos de Qualidade](#atributos-de-qualidade)
- [Apêndices](#apêndices)

## Introdução

### Propósito
Este documento especifica os requisitos do Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza (SGAI), fornecendo aos desenvolvedores as informações necessárias para o projeto e implementação, bem como para a realização de testes e validação do sistema.

### Escopo
O SGAI é uma aplicação web que visa otimizar o trabalho da Assessoria de Inclusão do CPS através da organização do fluxo de trabalho departamental, registro e acompanhamento de atendimentos, geração de relatórios e gestão de informações relacionadas à inclusão nas unidades educacionais.

### Definições, Acrônimos e Abreviações
- **CPS**: Centro Paula Souza
- **AI**: Assessoria de Inclusão
- **SGAI**: Sistema de Gestão da Assessoria de Inclusão
- **Etec**: Escola Técnica Estadual
- **Fatec**: Faculdade de Tecnologia
- **PcD**: Pessoa com Deficiência
- **NEE**: Necessidades Educacionais Especiais
- **PNE**: Pessoa com Necessidades Especiais
- **LGPD**: Lei Geral de Proteção de Dados
- **WCAG**: Web Content Accessibility Guidelines

### Referências
1. Documento de Visão e Escopo - Sistema de Gestão para Assessoria de Inclusão
2. Plano Estratégico do Centro Paula Souza
3. Legislação sobre inclusão e acessibilidade no ensino
4. Normativos internos do Centro Paula Souza
5. WCAG 2.1 - Diretrizes de Acessibilidade para Conteúdo Web

### Visão Geral do Documento
Este documento está organizado da seguinte forma: a seção Descrição Geral apresenta uma visão abrangente do sistema, suas funcionalidades principais e restrições; a seção Requisitos Funcionais detalha as capacidades específicas do sistema; as seções seguintes especificam os requisitos não funcionais, interfaces, dados e atributos de qualidade.

## Descrição Geral

### Perspectiva do Produto
O SGAI será um sistema independente desenvolvido especificamente para a Assessoria de Inclusão do CPS. Em versões futuras, poderá ser integrado a outros sistemas existentes no CPS. O sistema funcionará como uma aplicação web acessível pela rede interna do CPS, com diferentes níveis de acesso conforme o perfil do usuário.

### Funções do Produto
O sistema incluirá as seguintes funções principais:
1. Gestão de atendimentos e casos relacionados à inclusão
2. Organização e acompanhamento do fluxo de trabalho departamental
3. Geração de relatórios e estatísticas sobre as atividades realizadas
4. Gestão de informações relacionadas à inclusão nas unidades do CPS
5. Repositório de recursos e materiais sobre inclusão

### Classes e Características de Usuários
O sistema atenderá às necessidades dos seguintes perfis de usuários:

1. **Administradores do Sistema**
   - Acesso a todas as funcionalidades
   - Responsáveis pela configuração do sistema
   - Gerenciamento de usuários e permissões

2. **Gestores da Assessoria de Inclusão**
   - Visão geral de todos os casos e atividades
   - Acesso a todos os relatórios e estatísticas
   - Aprovação de processos específicos
   - Configuração de fluxos de trabalho

3. **Técnicos da Assessoria de Inclusão**
   - Registro e acompanhamento de atendimentos
   - Acesso a recursos e materiais
   - Geração de relatórios específicos
   - Comunicação com as unidades

4. **Coordenadores de Unidades (Etecs/Fatecs)**
   - Registro de demandas para a Assessoria
   - Consulta de status de atendimentos de sua unidade
   - Acesso a recursos e materiais sobre inclusão

### Ambiente Operacional
- Sistema operacional: Compatível com Windows, Linux e macOS
- Navegadores: Chrome, Firefox, Edge e Safari em suas versões atuais
- Dispositivos: Compatível com computadores desktop, notebooks e tablets
- Servidor: Sistema operacional Linux com suporte a aplicações web
- Banco de dados: Sistema de gerenciamento de banco de dados relacional

### Restrições de Design e Implementação
- Aplicação web desenvolvida seguindo padrões REST
- Interface responsiva utilizando HTML5, CSS3 e JavaScript
- Conformidade com diretrizes de acessibilidade WCAG 2.1 nível AA
- Implementação de medidas de segurança para proteção de dados sensíveis
- Integração com sistema de autenticação do CPS
- Interfaces intuitivas adequadas a usuários com diferentes níveis de experiência tecnológica

### Suposições e Dependências
- Disponibilidade de infraestrutura de servidores do CPS
- Suporte contínuo da equipe de TI para manutenção do sistema
- Acesso à rede interna do CPS para todos os usuários do sistema
- Comprometimento da equipe da Assessoria para utilização e alimentação do sistema
- Disponibilidade de dados históricos para migração ao novo sistema

## Requisitos Funcionais

### RF-01: Autenticação e Controle de Acesso

#### RF-01.1: Login no Sistema
**Descrição**: O sistema deve permitir que os usuários façam login usando credenciais institucionais do CPS.  
**Prioridade**: Alta  
**Entradas**: Nome de usuário e senha.  
**Processamento**: Validação das credenciais contra o banco de dados de usuários.  
**Saídas**: Acesso ao sistema com permissões específicas ao perfil do usuário.  

#### RF-01.2: Gerenciamento de Permissões
**Descrição**: O sistema deve permitir a configuração de diferentes níveis de acesso baseados em perfis de usuário.  
**Prioridade**: Alta  
**Entradas**: Perfil do usuário, módulos do sistema.  
**Processamento**: Associação de permissões específicas a perfis de usuário.  
**Saídas**: Matriz de permissões por perfil de usuário.  

#### RF-01.3: Recuperação de Senha
**Descrição**: O sistema deve permitir que os usuários recuperem suas senhas através de e-mail institucional.  
**Prioridade**: Média  
**Entradas**: E-mail institucional.  
**Processamento**: Verificação da existência do e-mail e envio de link seguro para redefinição.  
**Saídas**: E-mail com instruções para redefinição de senha.  

#### RF-01.4: Gerenciamento de Usuários
**Descrição**: O sistema deve permitir aos administradores criar, editar, desativar e reativar contas de usuários.  
**Prioridade**: Alta  
**Entradas**: Dados do usuário (nome, e-mail, unidade, cargo, perfil).  
**Processamento**: Registro ou atualização dos dados na base de usuários.  
**Saídas**: Confirmação da operação realizada.  

### RF-02: Gestão de Atendimentos

#### RF-02.1: Registro de Atendimentos
**Descrição**: O sistema deve permitir o registro detalhado de atendimentos realizados pela Assessoria.  
**Prioridade**: Alta  
**Entradas**: Dados do atendimento (data, tipo, unidade, responsável, descrição, etc.).  
**Processamento**: Armazenamento das informações no banco de dados.  
**Saídas**: Número de protocolo e confirmação do registro.  

#### RF-02.2: Categorização de Atendimentos
**Descrição**: O sistema deve permitir categorizar os atendimentos por tipo de necessidade ou demanda.  
**Prioridade**: Alta  
**Entradas**: Tipo de necessidade (física, visual, auditiva, intelectual, múltipla, etc.).  
**Processamento**: Classificação e armazenamento da categorização.  
**Saídas**: Atendimento categorizado no sistema.  

#### RF-02.3: Acompanhamento de Status
**Descrição**: O sistema deve permitir atualizar e acompanhar o status dos atendimentos em andamento.  
**Prioridade**: Alta  
**Entradas**: ID do atendimento, novo status, comentários.  
**Processamento**: Atualização do status e registro de histórico da alteração.  
**Saídas**: Confirmação da atualização e notificação aos interessados.  

#### RF-02.4: Histórico de Atendimentos
**Descrição**: O sistema deve manter um histórico completo de todos os atendimentos e suas alterações.  
**Prioridade**: Média  
**Entradas**: ID do atendimento.  
**Processamento**: Recuperação do histórico completo de alterações e ações.  
**Saídas**: Exibição cronológica de todas as atividades relacionadas ao atendimento.  

#### RF-02.5: Atribuição de Responsáveis
**Descrição**: O sistema deve permitir atribuir responsáveis para o acompanhamento de cada atendimento.  
**Prioridade**: Alta  
**Entradas**: ID do atendimento, usuário responsável.  
**Processamento**: Associação do usuário ao atendimento e registro no histórico.  
**Saídas**: Confirmação da atribuição e notificação ao responsável.  

### RF-03: Organização do Fluxo de Trabalho

#### RF-03.1: Dashboard Personalizado
**Descrição**: O sistema deve apresentar dashboards personalizados conforme o perfil do usuário.  
**Prioridade**: Média  
**Entradas**: Perfil do usuário, preferências de configuração.  
**Processamento**: Composição dinâmica dos elementos do dashboard conforme perfil e preferências.  
**Saídas**: Exibição de dashboard personalizado com informações relevantes.  

#### RF-03.2: Gestão de Tarefas
**Descrição**: O sistema deve permitir criar e acompanhar tarefas relacionadas aos atendimentos.  
**Prioridade**: Alta  
**Entradas**: Título, descrição, responsável, prazo, prioridade.  
**Processamento**: Registro da tarefa e associação ao atendimento ou processo.  
**Saídas**: Tarefa registrada e visível na lista de pendências do responsável.  

#### RF-03.3: Notificações e Alertas
**Descrição**: O sistema deve enviar notificações sobre prazos e atualizações de status.  
**Prioridade**: Média  
**Entradas**: Eventos do sistema (alterações, prazos próximos).  
**Processamento**: Identificação dos usuários a serem notificados e geração de alertas.  
**Saídas**: Notificações no sistema e/ou por e-mail.  

#### RF-03.4: Configuração de Fluxos de Trabalho
**Descrição**: O sistema deve permitir configurar fluxos de trabalho para diferentes tipos de atendimento.  
**Prioridade**: Baixa  
**Entradas**: Definição das etapas, responsáveis e condições de transição.  
**Processamento**: Configuração do fluxo no sistema e validação de consistência.  
**Saídas**: Fluxo de trabalho disponível para aplicação nos atendimentos.  

### RF-04: Geração de Relatórios

#### RF-04.1: Relatórios Predefinidos
**Descrição**: O sistema deve oferecer relatórios padrão para as necessidades comuns da Assessoria.  
**Prioridade**: Alta  
**Entradas**: Seleção do tipo de relatório, período, filtros específicos.  
**Processamento**: Consulta aos dados conforme parâmetros e formatação do relatório.  
**Saídas**: Exibição do relatório com opções de exportação.  

#### RF-04.2: Gerador de Relatórios Customizados
**Descrição**: O sistema deve permitir a criação de relatórios personalizados pelos usuários.  
**Prioridade**: Média  
**Entradas**: Seleção de campos, filtros, agrupamentos e ordenações.  
**Processamento**: Construção dinâmica da consulta e formatação do relatório.  
**Saídas**: Relatório personalizado com opções de salvamento e exportação.  

#### RF-04.3: Exportação de Dados
**Descrição**: O sistema deve permitir a exportação de relatórios em diversos formatos.  
**Prioridade**: Média  
**Entradas**: Relatório gerado, formato desejado (PDF, Excel, CSV).  
**Processamento**: Conversão dos dados para o formato selecionado.  
**Saídas**: Arquivo para download no formato escolhido.  

#### RF-04.4: Visualizações Gráficas
**Descrição**: O sistema deve apresentar visualizações gráficas dos dados estatísticos.  
**Prioridade**: Baixa  
**Entradas**: Tipo de gráfico, dados a serem representados, período.  
**Processamento**: Geração da representação gráfica dos dados selecionados.  
**Saídas**: Exibição de gráficos interativos com opções de exportação.  

### RF-05: Gestão de Informações de Inclusão

#### RF-05.1: Cadastro de Unidades
**Descrição**: O sistema deve manter cadastro atualizado das unidades do CPS e suas características de acessibilidade.  
**Prioridade**: Média  
**Entradas**: Dados da unidade, recursos de acessibilidade disponíveis.  
**Processamento**: Registro ou atualização dos dados da unidade.  
**Saídas**: Confirmação do cadastro/atualização.  

#### RF-05.2: Repositório de Materiais
**Descrição**: O sistema deve funcionar como repositório de materiais e recursos sobre inclusão.  
**Prioridade**: Baixa  
**Entradas**: Arquivo, título, descrição, categoria, tags.  
**Processamento**: Upload e indexação do material no sistema.  
**Saídas**: Confirmação do upload e disponibilização para consulta.  

#### RF-05.3: Cadastro de Profissionais Especializados
**Descrição**: O sistema deve permitir o cadastro de profissionais especializados em diferentes áreas da inclusão.  
**Prioridade**: Média  
**Entradas**: Dados do profissional, área(s) de especialização, unidade, contato.  
**Processamento**: Registro dos dados no sistema.  
**Saídas**: Confirmação do cadastro e disponibilização para consulta.  

#### RF-05.4: Registro de Adaptações Curriculares
**Descrição**: O sistema deve permitir o registro e acompanhamento de adaptações curriculares implementadas.  
**Prioridade**: Média  
**Entradas**: Curso, disciplina, tipo de adaptação, descrição, resultados.  
**Processamento**: Registro das informações no sistema.  
**Saídas**: Confirmação do registro e disponibilização para consulta.  

## Requisitos Não Funcionais

### RNF-01: Segurança

#### RNF-01.1: Autenticação e Autorização
**Descrição**: O sistema deve implementar autenticação segura e controle de acesso baseado em perfis.  
**Critério de Aceitação**: Conformidade com protocolos de segurança para autenticação e gestão de sessões.  

#### RNF-01.2: Proteção de Dados
**Descrição**: O sistema deve garantir a proteção de dados sensíveis conforme a LGPD.  
**Critério de Aceitação**: Dados pessoais criptografados e políticas de privacidade implementadas.  

#### RNF-01.3: Auditoria
**Descrição**: O sistema deve manter logs de todas as ações realizadas pelos usuários.  
**Critério de Aceitação**: Registros detalhados de ações críticas com possibilidade de rastreamento.  

### RNF-02: Usabilidade

#### RNF-02.1: Facilidade de Uso
**Descrição**: O sistema deve ter interface intuitiva e fácil de usar para todos os perfis de usuário.  
**Critério de Aceitação**: Avaliação positiva por pelo menos 80% dos usuários em testes de usabilidade.  

#### RNF-02.2: Design Responsivo
**Descrição**: A interface deve adaptar-se automaticamente a diferentes tamanhos de tela.  
**Critério de Aceitação**: Funcionamento adequado nos principais dispositivos e navegadores.  

#### RNF-02.3: Acessibilidade
**Descrição**: O sistema deve seguir as diretrizes de acessibilidade WCAG 2.1 nível AA.  
**Critério de Aceitação**: Conformidade verificada por ferramentas automatizadas e testes com usuários.  

### RNF-03: Desempenho

#### RNF-03.1: Tempo de Resposta
**Descrição**: O sistema deve responder a operações comuns em menos de 3 segundos.  
**Critério de Aceitação**: 95% das operações concluídas dentro do tempo especificado em testes de carga.  

#### RNF-03.2: Capacidade
**Descrição**: O sistema deve suportar até 100 usuários simultâneos sem degradação de desempenho.  
**Critério de Aceitação**: Desempenho mantido durante testes de carga com o número especificado de usuários.  

#### RNF-03.3: Disponibilidade
**Descrição**: O sistema deve garantir disponibilidade de 99% durante o horário comercial.  
**Critério de Aceitação**: Monitoramento confirma disponibilidade conforme especificado.  

### RNF-04: Manutenibilidade

#### RNF-04.1: Modularidade
**Descrição**: O sistema deve ser estruturado de forma modular para facilitar manutenção e atualizações.  
**Critério de Aceitação**: Código organizado em módulos com baixo acoplamento.  

#### RNF-04.2: Documentação
**Descrição**: O código e a arquitetura devem ser adequadamente documentados.  
**Critério de Aceitação**: Documentação técnica completa e atualizada.  

### RNF-05: Portabilidade

#### RNF-05.1: Compatibilidade com Navegadores
**Descrição**: O sistema deve funcionar corretamente nos principais navegadores web.  
**Critério de Aceitação**: Funcionamento verificado no Chrome, Firefox, Edge e Safari em suas versões atuais.  

## Requisitos de Interface

### RI-01: Interfaces de Usuário

#### RI-01.1: Tela de Login
**Descrição**: Interface para autenticação do usuário no sistema.  
**Elementos**: Campos para usuário e senha, opção "Esqueci minha senha", botão de login.  

#### RI-01.2: Dashboard Principal
**Descrição**: Página inicial após login, com visão geral personalizada ao perfil.  
**Elementos**: Resumo de atendimentos, tarefas pendentes, alertas, acesso rápido às principais funções.  

#### RI-01.3: Gestão de Atendimentos
**Descrição**: Interface para registro e acompanhamento de atendimentos.  
**Elementos**: Formulário de registro, lista de atendimentos, filtros, detalhes do atendimento.  

#### RI-01.4: Geração de Relatórios
**Descrição**: Interface para geração e visualização de relatórios.  
**Elementos**: Seleção de tipo de relatório, filtros, visualização prévia, opções de exportação.  

### RI-02: Interfaces de Software

#### RI-02.1: Integração com Sistema de E-mail
**Descrição**: Interface para envio de notificações por e-mail.  
**Requisitos**: Configuração de servidor SMTP, modelos de e-mail, gestão de envios.  

#### RI-02.2: API para Integrações Futuras
**Descrição**: Interface para integração com outros sistemas do CPS (para versões futuras).  
**Requisitos**: Documentação de endpoints, autenticação, formatos de dados.  

## Requisitos de Dados

### RD-01: Modelo de Dados

#### RD-01.1: Entidades Principais
- Usuários
- Unidades (Etecs/Fatecs)
- Atendimentos
- Tarefas
- Materiais/Recursos
- Profissionais
- Adaptações Curriculares

#### RD-01.2: Relacionamentos Principais
- Usuários pertencem a Unidades
- Atendimentos são registrados para Unidades
- Tarefas são associadas a Atendimentos
- Usuários são responsáveis por Tarefas

### RD-02: Requisitos de Backup e Recuperação

#### RD-02.1: Backup Regular
**Descrição**: O sistema deve realizar backup completo dos dados diariamente.  
**Critério de Aceitação**: Backups automatizados e verificados quanto à integridade.  

#### RD-02.2: Retenção de Dados
**Descrição**: Os dados devem ser retidos conforme política do CPS e legislação aplicável.  
**Critério de Aceitação**: Configuração de períodos de retenção e procedimentos de arquivamento.  

## Atributos de Qualidade

### AQ-01: Confiabilidade

#### AQ-01.1: Tolerância a Falhas
**Descrição**: O sistema deve continuar operando mesmo na presença de erros não críticos.  
**Critério de Aceitação**: Recuperação automática de falhas sem perda de dados.  

#### AQ-01.2: Precisão
**Descrição**: O sistema deve garantir a precisão e integridade dos dados armazenados.  
**Critério de Aceitação**: Validação de dados de entrada e verificações de integridade.  

### AQ-02: Escalabilidade

#### AQ-02.1: Crescimento de Dados
**Descrição**: O sistema deve suportar crescimento contínuo do volume de dados.  
**Critério de Aceitação**: Desempenho mantido com aumento de 30% no volume de dados anualmente.  

#### AQ-02.2: Novos Módulos
**Descrição**: A arquitetura deve permitir a adição de novos módulos sem impactar os existentes.  
**Critério de Aceitação**: Implementação bem-sucedida de módulo adicional em teste de prova de conceito.  

## Apêndices

### Apêndice A: Glossário

Listagem de termos específicos do domínio e suas definições, incluindo termos técnicos relacionados à inclusão e acessibilidade.

### Apêndice B: Modelos de Análise

Diagramas e modelos conceituais que auxiliam na compreensão do sistema:
- Diagrama de Casos de Uso
- Diagrama de Classes
- Modelo de Entidade-Relacionamento
- Diagramas de Sequência para principais fluxos

### Apêndice C: Protótipos de Interface

Mockups e protótipos das principais interfaces do sistema, incluindo:
- Tela de Login
- Dashboard Principal
- Registro de Atendimentos
- Gestão de Tarefas
- Geração de Relatórios