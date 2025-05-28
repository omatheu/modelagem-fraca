# Documento de Visão e Escopo
## Sistema de Gestão para Assessoria de Inclusão do Centro Paula Souza

## Sumário
- [Introdução](#introdução)
- [Escopo e Alinhamento Estratégico](#escopo-e-alinhamento-estratégico)
- [Definições, Acrônimos e Abreviações](#definições-acrônimos-e-abreviações)
- [Análise de Contexto](#análise-de-contexto)
- [Detalhamento da Necessidade](#detalhamento-da-necessidade)
- [Partes Interessadas](#partes-interessadas)
- [Necessidades das Partes Interessadas ou Usuários](#necessidades-das-partes-interessadas-ou-usuários)
- [Objetivos de Negócio](#objetivos-de-negócio)
- [Visão Geral do Produto](#visão-geral-do-produto)
- [Requisitos não Funcionais](#requisitos-não-funcionais)
- [Suposições e Dependências](#suposições-e-dependências)
- [Custos](#custos)
- [Outros Requisitos do Produto](#outros-requisitos-do-produto)
- [Requisitos da Solução](#requisitos-da-solução)
- [Requisitos de Documentação](#requisitos-de-documentação)
- [Restrições](#restrições)
- [Riscos](#riscos)
- [Documentos de Referência](#documentos-de-referência)

## Introdução

Este documento descreve a visão e o escopo do Sistema de Gestão para a Assessoria de Inclusão do Centro Paula Souza (CPS). O propósito é definir e comunicar, em alto nível, as características gerais do sistema proposto, articulando as necessidades e funcionalidades necessárias para otimizar o trabalho da Assessoria de Inclusão.

O sistema visa prover uma plataforma digital integrada que permita organizar o fluxo de trabalho departamental, registrar e acompanhar atendimentos, gerar relatórios e facilitar a gestão de informações relacionadas à inclusão nas unidades do CPS.

## Escopo e Alinhamento Estratégico

O sistema proposto alinha-se à missão do Centro Paula Souza de oferecer educação pública profissional e tecnológica de qualidade, promovendo a inclusão e acessibilidade. A Assessoria de Inclusão desempenha papel fundamental ao garantir que alunos com necessidades especiais tenham acesso adequado à educação, e este sistema visa fortalecer esse trabalho.

O escopo do projeto inclui:
- Desenvolvimento de uma plataforma web para gestão interna da Assessoria de Inclusão
- Módulos para registro e acompanhamento de atendimentos
- Ferramentas para organização do fluxo de trabalho
- Sistema de geração de relatórios e estatísticas
- Gestão de informações relacionadas à inclusão nas unidades do CPS

Estão fora do escopo:
- Integração com sistemas acadêmicos existentes (prevista para fases futuras)
- Atendimento direto aos alunos (o sistema é para uso administrativo da Assessoria)
- Substituição de outros sistemas de gestão do CPS

## Definições, Acrônimos e Abreviações

- **CPS**: Centro Paula Souza
- **AI**: Assessoria de Inclusão
- **Etec**: Escola Técnica Estadual
- **Fatec**: Faculdade de Tecnologia
- **PcD**: Pessoa com Deficiência
- **NEE**: Necessidades Educacionais Especiais
- **SGAI**: Sistema de Gestão da Assessoria de Inclusão (nome proposto para o sistema)
- **PNE**: Pessoa com Necessidades Especiais

## Análise de Contexto

O Centro Paula Souza, presente em 345 municípios do estado de São Paulo, administra 228 Escolas Técnicas, 79 Faculdades de Tecnologia e 468 Classes Descentralizadas, totalizando mais de 317 mil alunos matriculados. A Assessoria de Inclusão é responsável por garantir que alunos com necessidades especiais tenham o suporte adequado para sua plena integração ao ambiente educacional.

Atualmente, a gestão dos atendimentos e o acompanhamento das ações de inclusão são realizados de forma manual ou através de ferramentas não específicas, resultando em:
- Dificuldade em acompanhar casos específicos
- Perda de histórico de atendimentos
- Falta de padronização nos processos
- Dificuldade na obtenção de dados para relatórios e tomada de decisão
- Atrasos no atendimento às demandas das unidades

## Detalhamento da Necessidade

### Descrição do Problema

A Assessoria de Inclusão do CPS enfrenta desafios significativos na organização e registro de suas atividades devido à ausência de uma ferramenta tecnológica específica para sua área de atuação. Os processos atuais, majoritariamente manuais, comprometem a eficiência do departamento e limitam sua capacidade de resposta às demandas das unidades educacionais.

### Parte(s) afetada(s)

- Equipe da Assessoria de Inclusão do CPS
- Coordenadores de unidades Etec e Fatec
- Alunos com necessidades especiais
- Professores e funcionários das unidades
- Gestão do Centro Paula Souza

### Impacto

O impacto da ausência de um sistema adequado reflete-se em:
- Menor eficiência nos atendimentos prestados
- Dificuldade no acompanhamento de casos em andamento
- Limitações na geração de dados estatísticos para prestação de contas
- Obstáculos à visibilidade do trabalho realizado pela Assessoria
- Desafios na identificação de padrões e necessidades recorrentes

### Solução de Sucesso

A implementação bem-sucedida do Sistema de Gestão para a Assessoria de Inclusão resultará em:
- Redução de 50% no tempo de registro e processamento de atendimentos
- Organização centralizada de todas as informações relativas à inclusão
- Capacidade de gerar relatórios automáticos de atividades e atendimentos
- Melhoria significativa no acompanhamento de casos e ações implementadas
- Maior transparência e visibilidade do trabalho da Assessoria
- Tomada de decisões baseada em dados concretos

## Alternativas

### Alternativa 1: Adaptação de Sistema Existente

Adaptar um sistema de CRM (Customer Relationship Management) ou ticket genérico para atender às necessidades da Assessoria de Inclusão.
- **Vantagens**: Menor tempo de implementação, menor custo inicial.
- **Desvantagens**: Pouca flexibilidade para requisitos específicos, limitações na personalização, possível resistência dos usuários por não ser uma ferramenta específica.

### Alternativa 2: Melhoria dos Processos Manuais Atuais

Padronização e melhoria dos processos manuais existentes, com uso de planilhas e documentos compartilhados.
- **Vantagens**: Custo mínimo, implementação imediata.
- **Desvantagens**: Não resolve o problema de forma estrutural, mantém fragilidades como dificuldade de consolidação de dados e geração de relatórios.

## Partes Interessadas

### Partes Interessadas

| Parte Interessada | Descrição | Papel no Projeto |
|-------------------|-----------|------------------|
| Assessoria de Inclusão do CPS | Departamento responsável pelas políticas de inclusão | Usuário principal e cliente direto |
| Direção do CPS | Alta gestão da instituição | Patrocinador e tomador de decisão estratégica |
| Coordenadores de Unidades | Gestores das Etecs e Fatecs | Usuários indiretos e fornecedores de informações |
| Professores | Corpo docente das unidades | Beneficiários indiretos |
| Alunos com NEE | Estudantes com necessidades educacionais especiais | Beneficiários finais |
| Equipe de TI do CPS | Responsáveis pela infraestrutura tecnológica | Suporte técnico e manutenção |

## Detalhamento dos Representantes das Partes Interessadas

| Nome | Representando | Papel | Responsabilidades |
|------|--------------|-------|-------------------|
| [Nome do Coordenador] | Assessoria de Inclusão | Coordenador do projeto | Validação de requisitos, aprovação de entregas |
| [Nome do Representante] | Direção do CPS | Patrocinador | Aprovação de recursos e decisões estratégicas |
| [Nome do Representante] | Equipe de TI | Gerente Técnico | Garantir compatibilidade com infraestrutura existente |

## Usuários

| Tipo de Usuário | Descrição | Necessidades | Envolvimento |
|----------------|-----------|-------------|--------------|
| Gestores da Assessoria de Inclusão | Coordenadores do departamento | Visão geral, relatórios, aprovações | Alto |
| Técnicos da Assessoria | Equipe operacional | Registro e acompanhamento de casos | Alto |
| Coordenadores de Unidades | Gestores locais das Etecs e Fatecs | Consulta e cadastro de necessidades | Médio |
| Administradores do Sistema | Equipe de TI | Manutenção e configuração do sistema | Médio |

## Necessidades das Partes Interessadas ou Usuários

### Registro e Acompanhamento de Atendimentos

- **Descrição**: Necessidade de registrar e acompanhar atendimentos realizados pela Assessoria de Inclusão
- **Prioridade**: Alta
- **Preocupações**: Garantir histórico completo, facilidade de uso e rápida recuperação de informações

### Organização do Fluxo de Trabalho

- **Descrição**: Necessidade de organizar o fluxo de trabalho departamental, com definição clara de responsabilidades e prazos
- **Prioridade**: Alta
- **Preocupações**: Visibilidade das tarefas pendentes, alertas de prazos, distribuição equilibrada de atividades

### Geração de Relatórios e Estatísticas

- **Descrição**: Necessidade de gerar relatórios de atividades e estatísticas para prestação de contas e planejamento
- **Prioridade**: Média
- **Preocupações**: Flexibilidade na geração de relatórios, acesso a dados históricos, visualização gráfica

### Gestão de Informações de Inclusão

- **Descrição**: Necessidade de gerenciar informações relacionadas à inclusão nas unidades do CPS
- **Prioridade**: Média
- **Preocupações**: Classificação adequada das informações, facilidade de consulta, segurança dos dados sensíveis

## Objetivos de Negócio

| ID | Objetivo | Problema/Oportunidade | Prioridade | Métricas |
|----|----------|----------------------|-----------|----------|
| OBJ-01 | Aumentar a eficiência dos atendimentos | Processos manuais lentos e propensos a erros | Alta | Redução de 50% no tempo de processamento |
| OBJ-02 | Melhorar o acompanhamento de casos | Dificuldade em manter histórico de atendimentos | Alta | 100% dos casos com histórico completo disponível |
| OBJ-03 | Facilitar a geração de relatórios | Demora e imprecisão na compilação manual de dados | Média | Redução de 70% no tempo de geração de relatórios |
| OBJ-04 | Padronizar processos de trabalho | Falta de procedimentos padronizados | Média | 90% dos processos seguindo fluxo definido no sistema |
| OBJ-05 | Ampliar visibilidade do trabalho da Assessoria | Trabalho realizado tem pouca visibilidade institucional | Baixa | Aumento de 30% no reconhecimento institucional |

## Visão Geral do Produto

### Perspectiva do Produto

O Sistema de Gestão para a Assessoria de Inclusão será uma aplicação web acessível internamente pela rede do CPS, com diferentes níveis de acesso conforme o perfil do usuário. O sistema centralizará todas as informações relativas ao trabalho da Assessoria, desde o registro inicial de casos até a geração de relatórios estatísticos.

O produto visa substituir os processos manuais e o uso de ferramentas genéricas, oferecendo uma solução específica para as necessidades da Assessoria de Inclusão. Em fases futuras, poderá integrar-se com outros sistemas do CPS para facilitar o fluxo de informações.

### Características-Chave do Produto

1. **Gestão de Atendimentos**
   - Registro detalhado de atendimentos
   - Categorização por tipo de necessidade
   - Acompanhamento de status e histórico
   - Atribuição de responsáveis

2. **Organização de Fluxo de Trabalho**
   - Dashboards personalizados por perfil
   - Visualização de tarefas pendentes e prazos
   - Notificações automáticas
   - Workflow configurável para diferentes tipos de atendimento

3. **Geração de Relatórios**
   - Relatórios predefinidos para necessidades comuns
   - Gerador de relatórios customizáveis
   - Exportação em diversos formatos (PDF, Excel, CSV)
   - Visualizações gráficas e dashboards estatísticos

4. **Gestão de Informações de Inclusão**
   - Cadastro de unidades e suas características de acessibilidade
   - Repositório de materiais e recursos para inclusão
   - Mapeamento de profissionais especializados
   - Registro de adaptações curriculares implementadas

## Requisitos não Funcionais

1. **Segurança**
   - Autenticação integrada com o sistema do CPS
   - Proteção de dados sensíveis conforme LGPD
   - Registro de log de atividades (audit trail)

2. **Usabilidade**
   - Interface intuitiva adequada para usuários com variados níveis de familiaridade tecnológica
   - Design responsivo para acesso via diferentes dispositivos
   - Conformidade com diretrizes de acessibilidade WCAG 2.1

3. **Desempenho**
   - Tempo de resposta inferior a 3 segundos para operações comuns
   - Capacidade para suportar até 100 usuários simultâneos
   - Disponibilidade de 99% em horário comercial

4. **Escalabilidade**
   - Capacidade de crescimento para atender aumento no volume de dados
   - Arquitetura que permita adição de novos módulos

## Suposições e Dependências

- Disponibilidade de infraestrutura de servidores do CPS
- Suporte da equipe de TI para implantação e manutenção
- Acesso à rede interna do CPS para todos os usuários do sistema
- Comprometimento da equipe da Assessoria para migração de dados existentes
- Disponibilidade de treinamento para usuários finais

## Custos

Os custos estimados incluem:
- Desenvolvimento do software
- Treinamento dos usuários
- Configuração inicial e migração de dados
- Manutenção e suporte por 12 meses

Os valores específicos serão definidos após análise detalhada de requisitos e elaboração do planejamento do projeto.

## Outros Requisitos do Produto

### Padrões e Normativos Aplicáveis

- Lei Geral de Proteção de Dados Pessoais (LGPD)
- Diretrizes de acessibilidade WCAG 2.1
- Normativos internos do Centro Paula Souza
- Normas e diretrizes do MEC relacionadas à inclusão

## Requisitos da Solução

Os requisitos detalhados da solução serão documentados no Documento de Especificação de Requisitos de Software (SRS), incluindo:
- Requisitos funcionais completos
- Casos de uso detalhados
- Protótipos de interface
- Regras de negócio específicas
- Requisitos de integração

## Requisitos de Documentação

A documentação do projeto incluirá:
- Manual do usuário
- Guia de administração do sistema
- Documentação técnica para manutenção
- Material de treinamento
- Documentação da API (para integrações futuras)

## Restrições

- Orçamento limitado para desenvolvimento
- Prazo de implementação desejado de 6 meses
- Necessidade de compatibilidade com infraestrutura existente no CPS
- Sistema deverá operar dentro da rede interna do CPS, com acesso controlado

## Riscos

| Risco | Probabilidade | Impacto | Estratégia de Mitigação |
|-------|--------------|---------|-------------------------|
| Resistência à adoção por usuários | Média | Alto | Envolvimento desde o início, treinamento adequado |
| Requisitos incompletos ou imprecisos | Alta | Alto | Validações frequentes, protótipos, feedback contínuo |
| Atrasos no cronograma | Média | Médio | Metodologia ágil, entregas incrementais |
| Limitações técnicas da infraestrutura | Baixa | Médio | Avaliação técnica prévia, planejamento de contingência |
| Rotatividade na equipe do projeto | Baixa | Alto | Documentação adequada, compartilhamento de conhecimento |

## Documentos de Referência

- Plano Estratégico do Centro Paula Souza
- Relatórios anteriores da Assessoria de Inclusão
- Legislação sobre inclusão e acessibilidade no ensino
- Documentação dos sistemas existentes no CPS
- Manuais de processos internos da Assessoria de Inclusão